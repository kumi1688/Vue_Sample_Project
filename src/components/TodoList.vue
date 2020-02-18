<template>
    <section>
        <transition-group name='list' tag='ul'>
            <li v-for="(todoItem, index) in this.$store.state.todoItems" :key='todoItem' class='shadow'> 
                <i class='checkBtn fas fa-check' aria-hidden='true'></i>
                    {{ todoItem }}
                <span class='updateBtn' type='botton' @click='showUpdateModal(todoItem, index)'>
                    <i class='checkBtn fas fa-edit' aria-hidden='true'></i>
                </span>
                <span class="removeBtn" type='botton' @click='removeTodo(todoItem, index)'>
                    <i class='far fa-trash-alt' aria-hidden="true"></i>
                </span>
                
                <modal v-if="showModal" @close='showModal=false'>
                    <h3 slot='header'>변경</h3>
                    <span slot='body'>
                        <input v-model="selectedItem" type='text' placeholder='type new name'
                        @keyup.enter="updateItem"> 
                        <button type='button' @click='updateItem'>변경</button>
                    </span>
                    <span slot='footer' @click='showModal = false'>
                        <i class='closeModalBtn fas fa-times' aria-hidden="true"></i>
                    </span>
                </modal>
            </li>
        </transition-group>

    </section>
</template>

<script>
import Modal from './common/Modal.vue';

    export default {
        data() {
            return { showModal: false, selectedIndex: -1, selectedItem: ''}
        },
        methods: {
            removeTodo(todoItem, index){
                var data = {todoItem: todoItem, index: index};
                this.$store.commit('removeTodo', data);
            },
            showUpdateModal(todoItem, index){
                this.selectedIndex = index;
                this.selectedItem = todoItem;
                console.log(todoItem, index);
                this.showModal = !this.showModal;
            },
            updateItem(){
                var data = {newName: this.selectedItem, index: this.selectedIndex};
                console.log(data);
                this.$store.commit('updateItem', data);
                this.showModal = !this.showModal;
                this.selectedIndex = -1;
                this.selectedItem = '';
            }
        },
        props: ['propsdata'],
        components: {
            'modal' : Modal,
        }
    }
</script>

<style scoped>
    ul{
        list-style-type: none;
        padding-left: 0px;
        margin-top: 0;
        text-align: left;
    }

    li{
        display : flex;
        min-height: 50px;
        height: 50px;
        line-height: 50px;
        margin : 0.5rem, 0;
        padding: 0, 0.9rem;
        background: white;
        border-radius: 5px;
    }
    .checkBtn{
        line-height: 45px;
        color: #62acde;
        margin-right: 5px;
    }
    .removeBtn{
        margin-left: 10px;
        color: #de4343;
    }
    .updateBtn{
        margin-left: auto;
        color: #de4343;
    }
    .list-enter-active, .list-leave-active{
        transition: all 0.5s;
    }
    .list-enter, .list-leave-to {
        opacity: 0;
        transform: translateY(30px);
    }
</style>