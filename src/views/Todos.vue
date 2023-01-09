<template>
  <div>
    <router-link to="/">Home</router-link>
    <h2>ToDo application </h2>
    <addTodo
        @add-Todo="addTodo"
    />
    <select v-model="filter">
      <option value="all">All</option>
      <option value="completed">Completed</option>
      <option value="not-completed">Not completed</option>
    </select>
    <hr>

    <LoaderTodo v-if="loading"/>

    <TodoList
        v-else-if="filterTodo.length"
        v-bind:todos="filterTodo"
        @remove-todo="removeToDo"
    />
    <p v-else>No Todos!!!</p>
  </div>
</template>

<script>

import TodoList from "@/components/TodoList";
import AddTodo from "@/components/addTodo";
import LoaderTodo from "@/components/LoaderTodo"
import axios from "axios"

export default {
  name: 'App',
  data() {
    return {
       todos: [],

      newTodo: null,
      loading: true,
      filter: "all"
    }
  },
  mounted() {


    fetch('http://localhost:3000/items')
        .then(response => response.json())
        .then(json => {this.todos
          setTimeout(() => {
            this.todos = json
            this.loading = false
          }, 1)

        })



  },






  computed:{
    filterTodo(){
      if (this.filter === "all") return this.todos
      if (this.filter === "completed"){return this.todos.filter(t => t.completed)}
      if (this.filter === "not-completed"){return this.todos.filter(t => !t.completed)}
      else  return console.log('!!!не отработал фильтр!!!')
    }
  },

  methods: {
    removeToDo(id) {
      this.todos = this.todos.filter(t => t.id !== id)
    },

    // removeToDo(x) {
    //   this.todos.splice(x,1);
    //   this.saveTodo();
    // },


    //
    // addTodo(todo) {
    //   this.todos.push(todo)
      //this.saveTodo()
    // },


     addTodo( todos) {
       const ttt= axios.post(`http://localhost:3000/items`,todos );
       this.todos = [...this.items, ttt ];
        this.todo = "";

    },


    // addTodo() {
    //   // ensure they actually typed something
    //   if(!this.todo) return;
    //   this.todo.push(this.todo);
    //   this.todo = '';
    //   this.saveTodo();
    // },
  //добавление в json






  },
  components: {AddTodo, TodoList, LoaderTodo}
}











</script>

<style>
</style>
