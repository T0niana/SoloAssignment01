<script setup lang="ts">
import { computed, ref } from 'vue'
import TodoList from './TodoList.vue'
import type { Todo } from '../types/Todo.ts'

type Filter = 'all' | 'open' | 'done'

// To Do Items die schon existieren
const todos = ref<Todo[]>([
  { id: 1, text: 'Vue App bauen', done: true },
  { id: 2, text: 'Styling anpassen', done: false },
  { id: 3, text: 'Documentation schreiben', done: false },
])

// Neues Todo startet immer leer
const newTodoText = ref('')
// Filter startet immer auf all
const activeFilter = ref<Filter>('all')
// Nächste ID ist 4 weil 1-3 schon definiert wurden
let nextId = 4

// computed gibt filtered todos zurück, je nachdem welcher Filter aktiv ist
const filteredTodos = computed(() => {
  if (activeFilter.value === 'open') {
    return todos.value.filter((todo) => !todo.done)
  }

  if (activeFilter.value === 'done') {
    return todos.value.filter((todo) => todo.done)
  }

  return todos.value
})

function addTodo() {
  // Falls der user white space dazu eingibt, dann wird der entfernt
  const text = newTodoText.value.trim()
  // Wenn der text nicht leer ist, dann das neue Todo hinzufügen
  if (!text) return
  //Das neue Todo kommt ins array
  todos.value.push({
    id: nextId++,
    text,
    done: false,
  })
  // Für neue Todos ist das input feld wieder leer
  newTodoText.value = ''
}


function toggleTodo(id: number) {
  // Finde das Todo mit der gegebenen ID
  const todo = todos.value.find((todo) => todo.id === id)
  // Wenn das Todo existiert, dann den done status umkehren (also gemacht -> zu erledigen und erledigen -> zu gemacht)
  if (todo) {
    todo.done = !todo.done
  }
}

// Entfernt das Todo mit der gegebenen ID aus dem Array
function removeTodo(id: number) {
  todos.value = todos.value.filter((todo) => todo.id !== id)
}
</script>

<template>
  <main class="todo-app">
    <h1>Meine Todos</h1>

    <form class="add-form" @submit.prevent="addTodo">
      <input
        v-model="newTodoText"
        type="text"
        placeholder="Neues Todo"
        aria-label="Neues Todo"
      />
      <button class="add-button" type="submit">Hinzufügen</button>
    </form>

    <div class="filters" aria-label="Todos filtern">
      <button
        type="button"
        :class="{ active: activeFilter === 'all' }"
        @click="activeFilter = 'all'"
      >
        Alle
      </button>

      <button
        type="button"
        :class="{ active: activeFilter === 'open' }"
        @click="activeFilter = 'open'"
      >
        Offen
      </button>

      <button
        type="button"
        :class="{ active: activeFilter === 'done' }"
        @click="activeFilter = 'done'"
      >
        Erledigt
      </button>
    </div>

    <TodoList
      :todos="filteredTodos"
      @toggle="toggleTodo"
      @remove="removeTodo"
    />
  </main>
</template>

<style scoped>
.todo-app {
  width: min(100% - 2rem, 40rem);
  margin: 3rem auto;
  padding: 2rem;
  border: 1px solid #BAC095;
  border-radius: 0.75rem;
}

.add-form {
  display: flex;
  gap: 0.5rem;
  margin-bottom: 1rem;
}

.add-form input {
  min-width: 0;
  flex: 1;
  padding: 0.75rem;
}

button {
  padding: 0.65rem 0.9rem;
  cursor: pointer;
  border-radius: 0.5rem;
  background-color: #BAC095;
}

.add-button {
  background-color: #636B2F;
  color: white;
}

.filters {
  display: flex;
  gap: 0.5rem;
}


.filters .active {
  background: #3D4127;
  color: white;
}
</style>
