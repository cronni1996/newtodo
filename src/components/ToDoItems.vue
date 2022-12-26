<template>

  <li class="lio">
 <span v-bind:class="{done: todos.completed}">
<input type="checkbox"
       @change="bulew">
   <strong>{{index+1}}</strong>
   {{todo.title }}
 </span>
    <button class="bt"
    v-on:click="$emit('remove-todo', todo.id )">
      X</button>
  </li>
</template>

<script>



export default {
  props: {
    todo: {
      type: Object,
      required: true
    },
    index: Number
  },
  data() {
    return {
      todos: this.todo
    }
  },

  methods: {
    bulew() {
      this.todos.completed = !this.todos.completed
    }
  } ,

  filters:{
    uppercase(value) {
return value.toUpperCase()
    }
  }
}


</script >

<style scoped>

.lio {
  border: 1px solid #ccc;
  display: flex;
  justify-content: space-between;
  padding: .5rem 2rem;
  margin-bottom: 1rem;
}
.bt{
  background: red;
  color: white;
  border-radius: 50%;
  font-weight: bold;
}

input{
  margin-right: 1rem;
}
.done{
  text-decoration: line-through;
}
</style>