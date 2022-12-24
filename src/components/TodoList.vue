<template>
  <div>
    <ul>

      <ToDoItems
          v-for="(todo, index ) of todos" :key=index
          v-bind:todo="todo"
          v-bind:index=index
          v-on:remove-todo="removeToDo"
      />
    </ul>
  </div>
</template>

<script>
import ToDoItems from "@/components/ToDoItems";

export default {
  props: ['todos'],
  components: {
    ToDoItems
  },
  methods: {
    removeToDo(id) {
      this.$emit('remove-todo', id)
    }
  }

}
</script>

<style>
</style>