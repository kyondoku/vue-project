<template>
    <div id="app">
        <input type="text" v-model="todo" @keyup.enter="reg">
        <button @click="reg">등록</button>
    </div>
</template>
<script>

export default {
    data() {
        return {
            todo: ''
        }
    },
    methods: {
        reg() {
            if(this.todo == '') {
                alert('내용을 입력해 주세요.')
                return
            }
            this.$emit('sendTodo', this.todo)
            this.todo = ''
        }
    }
}
</script>