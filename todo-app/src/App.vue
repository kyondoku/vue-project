<template>
  <div id="app">
    <TodoHeader/>
    <TodoInput @sendTodo="parentSendTodo"/>
    {{ todoList }}
  </div>
</template>

<script>
import TodoHeader from './components/TodoHeader';
import TodoInput from './components/TodoInput';

export default {
  name: 'App',
  components: { TodoHeader, TodoInput },
  data() {
    return {
      id: 0,
      todoList: [] 
    }
  },
  methods: {
    parentSendTodo(todo) {
      var obj = {
        id: ++this.id,
        todo: todo
      }
      this.todoList.push(obj)
      }
    }
  }
</script>

<style>

</style>
