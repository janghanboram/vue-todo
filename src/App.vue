<template>
  <div id="app">
    <todo-header></todo-header>
    <todo-input v-on:addTodo="addTodo"></todo-input>
    <todo-list v-bind:propsdata="todoItems" 
                v-on:removeTodo="removeTodo"
                v-on:editTodo="editTodo"></todo-list>
    <todo-footer v-on:removeAll="clearAll"></todo-footer>
  </div>
</template>

<script>
import TodoHeader from './assets/components/TodoHeader.vue';
import TodoInput from './assets/components/TodoInput.vue';
import TodoList from './assets/components/TodoList.vue';
import TodoFooter from './assets/components/TodoFooter.vue';

export default {
  data(){
    return{
        todoItems: []
    }
  },
  created() {
    if (localStorage.length > 0){
        for(var i=0; i<localStorage.length ; i++){
            this.todoItems.push(localStorage.getItem(localStorage.key(i)));
        }
    }
  },
  methods:{
    addTodo(todoItem){
      
      localStorage.setItem(todoItem,todoItem);
      this.todoItems.push(todoItem);
    },
    clearAll(){
      localStorage.clear();
      this.todoItems = [];
    },
    removeTodo(todoItem,index){
      let key = localStorage.key(index);
      localStorage.removeItem(key,todoItem);
      this.todoItems.splice(index,1);
    },
    editTodo(todoItem){
      let key = localStorage.key(todoItem.index);
      localStorage.setItem(key, todoItem.new);
      this.todoItems[todoItem.index] = todoItem.new;
    }
  },
  components:{
    'todo-header': TodoHeader,
    'todo-input': TodoInput,
    'todo-list': TodoList,
    'todo-footer': TodoFooter
  }
}
</script>

<style>
body{
    text-align: center;
    background-color: #F6F6F8;
}
input{
  border-style: groove;
  width: 200px;
}
button{
  border-style: groove;
}
.shadow{
  box-shadow: 5px 10px 10px rgba(0, 0, 0, 0.03)
}
</style>
