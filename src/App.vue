<template>
  <div id="app">
      <TodoHeader></TodoHeader>
      <TodoInput></TodoInput>
      <TodoList></TodoList>
      <TodoFooter></TodoFooter>
  </div>
</template>

<script>
import TodoHeader from './components/TodoHeader.vue'
import TodoFooter from './components/TodoFooter.vue'
import TodoInput from './components/TodoInput.vue'
import TodoList from './components/TodoList.vue'
import 'vue-use-vuex';
import Vue from 'vue';
import Vuex from 'vuex';

const store = new Vuex.Store({
  state: {
    count: 0,
    todoItems: [],
  },
  mutations: {
    increment (state) {
      state.count++;
    },
    decrease(state){
      state.count--;
    },
    setInitialTodo(state, todoItemList){
      state.todoItems = todoItemList;
      // console.log(state.todoItems);
    },
    addTodo(state, todoItem) {
      localStorage.setItem(localStorage.length, todoItem);
      state.todoItems.push(todoItem);
    },
    clearAll(state){
      localStorage.clear();
      state.todoItems = [];
    },
    removeTodo(state, data){
      console.log(data);
      
      var length = localStorage.length;
      localStorage.removeItem(data.index);
      for(var i = data.index+1; i < length; i++){
        var temp = localStorage.getItem(i);
        console.log(temp);
        localStorage.removeItem(i);
        localStorage.setItem(i-1, temp);
      }

      state.todoItems.splice(data.index,1);
      console.log(state.todoItems);
    },
    updateItem(state, data){
      console.log(data);
      localStorage.removeItem(data.index);
      localStorage.setItem(data.index, data.newName); 
      state.todoItems[data.index] = data.newName;
      console.log(state.todoItems[data.index]);
    },
  }
});

Vue.use(Vuex);

export default {
  store,
  components: {
    'TodoHeader': TodoHeader,
    'TodoFooter': TodoFooter,
    'TodoInput' : TodoInput,
    'TodoList' : TodoList,
  },
  created(){
            if(localStorage.length > 0 ){
                var list = [];
                for(var i = 0; i < localStorage.length; i++){
                    list = list.concat(localStorage.getItem(i));
                }
                
                this.$store.commit('setInitialTodo', list);
            }
    },
}
</script>

<style>
body{
        text-align : center;
        background-color: #f6f6f8;
    }
    input {
        border-style: groove;
        width: 200px;
    }
    botton {
        border-style: groove;
    }
    .shadow {
        box-shadow: 5px 10px 10px rgba(0, 0, 0, 0.03)
    }
</style>
