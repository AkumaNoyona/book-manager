<template>
  <div class="card mb-3" :class="{ 'bg-light': book.completed }">
    <div class="card-body">
      <div class="d-flex justify-content-between align-items-start">
        <div>
          <h5 class="card-title" :class="{ 'text-decoration-line-through text-muted': book.completed }">
            {{ book.title }}
          </h5>
          <h6 class="card-subtitle mb-2 text-muted">{{ book.author }}</h6>
          <span class="badge bg-secondary">{{ book.genre }}</span>
        </div>
        <div>
          <div v-if="book.completed" class="mb-2">
            <span
              v-for="star in 5"
              :key="star"
              class="fs-5 text-warning"
              style="cursor: pointer"
              @click="$emit('rate', star)"
            >
              {{ star <= book.rating ? '★' : '☆' }}
            </span>
          </div>
          <div class="btn-group" role="group">
            <button
              @click="$emit('toggle')"
              :class="['btn', book.completed ? 'btn-secondary' : 'btn-success']"
            >
              {{ book.completed ? 'Прочитано' : 'Отметить прочитанной' }}
            </button>
            <button @click="$emit('delete')" class="btn btn-danger">
              ✕
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps(['book'])
defineEmits(['toggle', 'delete', 'rate'])
</script>