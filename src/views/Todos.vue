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

export default {
  name: 'App',
  data() {
    return {
      todos: [],
      loading: true,
      filter: "all"
    }
  },
  mounted() {
    fetch('https://jsonplaceholder.typicode.com/todos?_limit=3')
        .then(response => response.json())
        .then(json => {
          setTimeout(() => {
            this.todos = json
            this.loading = false
          }, 1000)

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
    addTodo(todo) {
      this.todos.push(todo)
    }
  },
  components: {AddTodo, TodoList, LoaderTodo}
}
</script>

<style>
</style>
