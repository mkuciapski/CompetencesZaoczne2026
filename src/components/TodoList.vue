<script setup lang="ts">
defineProps<{ title: string; todos: { ID: number; Title: string; Completed: boolean }[]; fontSize?: string }>()
</script>
<template>
  <div class="todo-list card">
    <slot name="header">
      <span :class="fontSize">{{ title }}</span>
    </slot>

    <div v-for="todo in todos" :key="todo.ID">
      <slot name="todo" :todoItem="todo">
        <p class="border-1 rounded p-2 m-2 max-w-100" :class="{ 'line-through': todo.Completed }">{{ todo.Title }}</p>
      </slot>
    </div>

    <div class="summary">
      <h2 class="text-xl font-bold mt-4 mb-2">Podsumowanie</h2>
      <p>Łączna liczba zadań: {{ todos.length }}</p>
      <p>Liczba ukończonych zadań: {{ todos.filter((todo) => todo.Completed).length }}</p>
      <p>Liczba nieukończonych zadań: {{ todos.filter((todo) => !todo.Completed).length }}</p>
    </div>
  </div>
</template>

<style scoped>
.card {
  border: 2px solid #ccc;
  max-width: 20rem;
  margin: 1rem;
  border-radius: 8px;
  padding: 16px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}
</style>
