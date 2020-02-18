<template>
    <div>
        {{count}}   
        <button @click='onIncrease'>증가</button>
        <button @click='onDecrease'>감소</button>
    </div>
</template>

<script>
export default {
    computed: {
        count() {
            return this.$store.state.count
        }
    },
    methods: {
        onIncrease() {
            this.$store.commit('increment');
        },
        onDecrease(){
            this.$store.commit('decrease');
        }
        
    },
}
</script>

<style >
    
</style>