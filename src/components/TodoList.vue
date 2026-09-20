<script setup lang="ts">
import TodoItem from './TodoItem.vue'
import type { Todo } from '../types/Todo.ts'

// Die gefilterte Todo-Liste kommt als Prop von TodoApp.vue.
defineProps<{
  todos: Todo[]
}>()

// TodoList reicht die Events von TodoItem an TodoApp weiter.
defineEmits<{
  toggle: [id: number]
  remove: [id: number]
}>()
</script>

<template>
  <ul v-if="todos.length" class="todo-list">
    <TodoItem
      v-for="todo in todos"
      :key="todo.id"
      :todo="todo"
      @toggle="$emit('toggle', $event)"
      @remove="$emit('remove', $event)"
    />
  </ul>

  <p v-else>Keine Todos in diesem Filter.</p>
</template>

<style scoped>
.todo-list {
  margin: 1rem 0;
  padding: 0;
  list-style: none;
}
</style>
