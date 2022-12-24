<template>

  <li class="lio">
 <span v-bind:class="{done: todo.completed}">
<input type="checkbox"
       @change="bulew">
   <strong>{{index+1}}</strong>
   {{todo.title}}
 </span>
    <button class="bt"
    v-on:click="$emit('remove-todo', todos.id )">
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