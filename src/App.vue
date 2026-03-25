<template>
  <div class="container py-4">
    <header class="bg-gradient text-white p-4 rounded-3 mb-4" style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);">
      <h1 class="display-4">Менеджер книг</h1>
      <p class="lead">Управляй своей библиотекой</p>
    </header>

    <main>
      <AddBookForm @add-book="addBook" />
      <BookFilters
        v-model:searchQuery="searchQuery"
        v-model:filter="currentFilter"
        :books="books"
      />

      <div v-if="filteredBooks.length === 0" class="text-center py-5 text-muted">
        <p class="display-1">📚</p>
        <p class="h4">Книги не найдены :(</p>
        <p>Добавьте первую книгу или измените параметры поиска</p>
      </div>

      <div v-else class="row">
        <div v-for="book in filteredBooks" :key="book.id" class="col-md-6 col-lg-4">
          <BookCard
            :book="book"
            @toggle="toggleBook(book.id)"
            @delete="deleteBook(book.id)"
            @rate="rateBook(book.id, $event)"
          />
        </div>
      </div>
    </main>
  </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue'
import AddBookForm from './components/AddBookForm.vue'
import BookFilters from './components/BookFilters.vue'
import BookCard from './components/BookCard.vue'

// Загрузка из localStorage
const books = ref([])
const savedBooks = localStorage.getItem('books')
if (savedBooks) {
  books.value = JSON.parse(savedBooks)
}

const currentFilter = ref('all')
const searchQuery = ref('')

watch(books, (newBooks) => {
  localStorage.setItem('books', JSON.stringify(newBooks))
}, { deep: true })

const addBook = (bookData) => {
  const newBook = {
    id: Date.now(),
    ...bookData,
    completed: false,
    rating: 0
  }
  books.value.push(newBook)
}

const toggleBook = (id) => {
  const book = books.value.find(b => b.id === id)
  if (book) {
    book.completed = !book.completed
    if (!book.completed) book.rating = 0
  }
}

const rateBook = (id, rating) => {
  const book = books.value.find(b => b.id === id)
  if (book && book.completed) {
    book.rating = rating
  }
}

const deleteBook = (id) => {
  if (confirm('Удалить книгу?')) {
    books.value = books.value.filter(b => b.id !== id)
  }
}

const filteredBooks = computed(() => {
  return books.value
    .filter(book => {
      if (currentFilter.value === 'unread') return !book.completed
      if (currentFilter.value === 'read') return book.completed
      return true
    })
    .filter(book => {
      if (!searchQuery.value) return true
      const query = searchQuery.value.toLowerCase()
      return book.title.toLowerCase().includes(query) ||
             book.author.toLowerCase().includes(query)
    })
})
</script>

<style>
/* Весь кастомный CSS удалён, стили от Bootstrap */
</style>