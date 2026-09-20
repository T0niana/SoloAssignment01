<script setup lang="ts">
import type { Todo } from '../types/Todo'

// Wird von der parent komponente weiter gegeben, soll auch nur von parent verändert werden. 
defineProps<{
  todo: Todo
}>()

// TodoItem reicht die Events an TodoList weiter, die sie dann an TodoApp weiter gibt.
defineEmits<{
  toggle: [id: number]
  remove: [id: number]
}>()
</script>

<template>
  <li class="todo-item">
    <label>
      <input
        type="checkbox"
        :checked="todo.done"
        @change="$emit('toggle', todo.id)"
      />

      <span :class="{ done: todo.done }">
        {{ todo.text }}
      </span>
    </label>

    <button type="button" @click="$emit('remove', todo.id)">
      Löschen
    </button>
  </li>
</template>

<style scoped>
.todo-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1rem;
  padding: 0.75rem 0;
  border-bottom: 1px solid #BAC095;
}

label {
  display: flex;
  align-items: center;
  gap: 0.75rem;
}

.done {
  color: #717558;
  text-decoration: line-through;
}

button {
  background-color: #bb1d5435;
  color: #bb1d54;
  cursor: pointer;
  border: none;
  border-radius: 0.5rem;
  padding: 0.25rem;
}
</style>
