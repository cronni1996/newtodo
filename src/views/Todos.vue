<template>
  <div >
    <router-link to="/">Home</router-link>
    <h2 >ToDo application </h2>
    <addTodo
        @add-Todo="addTodo"
    />
    <hr>

      <LoaderTodo v-if="loading" />

    <TodoList
        v-else-if="todos.length"
        v-bind:todos="todos"
        @remove-todo="removeToDo"
    />
    <p v-else>No Todos!!!</p>
  </div>
</template>

<script>
import TodoList from "@/components/TodoList";
// import addTodo from "@/components/addTodo";
import AddTodo from "@/components/addTodo";
import LoaderTodo from "@/components/LoaderTodo"

export default {
  name: 'App',
  data(){
    return{
      todos: [],
      loading:  true
    }
  },
  mounted() {
    fetch('https://jsonplaceholder.typicode.com/todos?_limit=3')
        .then(response =>response.json())
        .then( json =>  {
          setTimeout(() =>{
            this.todos = json
            this.loading= false
          }, 1000)

        })
  },



  methods:{
    removeToDo(id){
      this.todos = this.todos.filter(t => t.id !== id)
    },
    addTodo(todo){
      this.todos.push(todo)
    }
  },
  components: {AddTodo, TodoList, LoaderTodo}
}
</script>

<style>
</style>
