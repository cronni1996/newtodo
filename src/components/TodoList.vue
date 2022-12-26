<template>
  <div>
    <ul>

      <ToDoItems
          v-for="(todo, i ) of todos" :key=i
          v-bind:todo="todo"
          v-bind:index=i
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