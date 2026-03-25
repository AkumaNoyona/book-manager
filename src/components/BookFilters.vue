<template>
  <div class="card p-4 mb-4">
    <div class="mb-3">
      <input
        v-model="searchQuery"
        type="text"
        class="form-control"
        placeholder="Поиск по названию или автору..."
      />
    </div>

    <div class="btn-group mb-3 w-100">
      <button
        v-for="option in filterOptions"
        :key="option.value"
        @click="$emit('update:filter', option.value)"
        :class="['btn', filter === option.value ? 'btn-primary' : 'btn-outline-secondary']"
      >
        {{ option.label }}
      </button>
    </div>

    <div class="text-muted small">
      <p class="mb-0">Всего: {{ total }} | Прочитано: {{ completed }} | Осталось: {{ total - completed }}</p>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps(['filter', 'books'])
defineEmits(['update:filter'])
const searchQuery = defineModel('searchQuery')

const filterOptions = [
  { value: 'all', label: 'Все' },
  { value: 'unread', label: 'Непрочитанные' },
  { value: 'read', label: 'Прочитанные' }
]

const total = computed(() => props.books.length)
const completed = computed(() => props.books.filter(b => b.completed).length)
</script>