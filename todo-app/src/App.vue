<template>
  <div id="app">
    <TodoHeader/>
    <!--TodoInput에서 emit한 이름을 적어야됨 -->
    <TodoInput @sendTodo="parentSendTodo" />
    <TodoList :todoList="todoList"
              @removeItem="parentDelTodo"
    />
    <TodoFooter @removeAll="parentDelAll"/>
  </div>
</template>

<script>
import TodoHeader from './components/TodoHeader'
import TodoInput from "./components/TodoInput";
import TodoList from "./components/TodoList";
import TodoFooter from "./components/TodoFooter";


export default {
  name: 'App',
  //import한 것을 객체화 한다고 생각하자
  components: {
    //이렇게 넣으면 TodoHeader : 'TodoHeader'로 적용된다
    TodoHeader,
    TodoInput,
    TodoList,
    TodoFooter
  },
  created() {
    this.selTodolist();
  },
  //현재 페이지에서 사용하는 변수 등
  data(){
    return{
      id : 0,
      todoList: []
    }
  },
  methods:{
    selTodolist() {
      this.$http.get('/api/selTodoList').then((res) => {
      this.todoList = res.data
     })
    },
    //자식에서 했던 것을 받아오는 것, js로 하려면 이벤트 걸어서 엔터했을때 등등등
    //emit에서 this.todo를 보냈으니까 parentSendTodo에서 todo를 받을 수 있음
    parentSendTodo(todo){
      var obj ={
        todo : todo
      }
      
      this.$http.post('/api/todo', obj).then(res => {
        console.log(res)
        if(res.data == 1) {
          this.selTodolist(); 
        }
      }, error => {
        console.log(`error : ${error}`)
      });
    },

    parentDelTodo(i_todo){
      console.log('i_todo : ' + i_todo)
      this.$http.delete('/api/todo', {
        params: {
          i_todo
        }
      }).then(res => {
        console.log(res.data)
        if(res.data == 1) {
          this.selTodolist();
        }
      })
    },
    parentDelAll(){
      this.$http.delete('/api/todo').then(res => {
        this.selTodolist();
      })
    }
  }
}
</script>

<style>
body{
  text-align: center;
  background-color: #f6f6f6;
}
input{
  border-style: groove;
}
button{
  border-style: groove;
}
.shadow{
  box-shadow: 5px 10px 10px rgba(0, 0, 0, 0.03);
}
</style>