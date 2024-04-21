<script setup>
import { ref } from 'vue'

const searchQuery = ref('')
const searchResults = ref([])
const isLoading = ref(false)
const error = ref(null)
const isDarkTheme = ref(false)
const currentPage = ref(1)
const totalPages = ref(0)

const searchWikipedia = async (query, page) => {
  const encodedQuery = encodeURIComponent(query)
  const endpoint = `https://en.wikipedia.org/w/api.php?action=query&list=search&prop=info&inprop=url&utf8=&format=json&origin=*&srlimit=10&srsearch=${encodedQuery}&sroffset=${(page - 1) * 10}`

  try {
    isLoading.value = true
    const response = await fetch(endpoint)
    const data = await response.json()

    if (data.query && data.query.search) {
      searchResults.value = data.query.search
      error.value = null
      totalPages.value = Math.ceil(data.query.searchinfo.totalhits / 10)
    } else {
      searchResults.value = []
      error.value = 'No results found.'
    }
  } catch (err) {
    console.error('Error fetching data:', err)
    searchResults.value = []
    error.value = 'An error occurred while fetching data.'
  } finally {
    isLoading.value = false
  }
}

const toggleTheme = () => {
  isDarkTheme.value = !isDarkTheme.value
}

const submitSearch = () => {
  if (searchQuery.value.trim() !== '') {
    currentPage.value = 1 // Reset current page to 1 when performing a new search
    searchWikipedia(searchQuery.value, currentPage.value)
  } else {
    searchResults.value = []
    error.value = 'Please enter a valid search term.'
  }
}

const goToPage = (page) => {
  if (page >= 1 && page <= totalPages.value) {
    currentPage.value = page
    searchWikipedia(searchQuery.value, currentPage.value)
  }
}
</script>

<template>
  <div class="container" :class="{ 'dark-theme': isDarkTheme }">
    <div class="header-container">
      <h1>Search Wikipedia</h1>
      <button id="theme-toggler" @click="toggleTheme">
        {{ isDarkTheme ? 'Light' : 'Dark' }} Mode
      </button>
    </div>

    <form @submit.prevent="submitSearch">
      <input
        v-model.trim="searchQuery"
        type="text"
        id="search-input"
        placeholder="Search Wikipedia..."
      />
      <button type="submit">Search</button>
    </form>

    <div id="search-results">
      <div v-if="isLoading" class="spinner">
        <div class="bounce1"></div>
        <div class="bounce2"></div>
        <div class="bounce3"></div>
      </div>
      <p v-if="error" class="error-message">{{ error }}</p>
      <div v-if="searchResults.length" class="results-container">
        <div v-for="result in searchResults" :key="result.pageid" class="result-item">
          <h3 class="result-title">
            <a :href="`https://en.wikipedia.org/?curid=${result.pageid}`" target="_blank" rel="noopener">{{ result.title }}</a>
          </h3>
          <p class="result-snippet" v-html="result.snippet"></p>
        </div>
      </div>

      <!-- Pagination controls -->
      <div class="pagination">
        <button @click="goToPage(currentPage - 1)" :disabled="currentPage === 1">Previous</button>
        <span>{{ currentPage }} / {{ totalPages }}</span>
        <button @click="goToPage(currentPage + 1)" :disabled="currentPage === totalPages">Next</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.centered-form {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}
.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 2rem;
  background-color: #f9f9f9;
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
 
}

.header-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
}

#theme-toggler {
  border: none;
  background: transparent;
  cursor: pointer;
  color: #0074d9;
  font-weight: bold;
}

#search-input {
  font-size: 1.2rem;
  padding: 0.8rem 1rem;
  border: 2px solid #ccc;
  border-radius: 8px;
  flex-grow: 1;
  width: 100%;  
}

@media screen and (min-width: 600px) {
  #search-input {
    width: 70%; 
  }
}

@media screen and (min-width: 900px) {
  #search-input {
    width: 60%;  
  }
}


button[type='submit'] {
  font-size: 1.2rem;
  padding: 0.8rem 1.5rem;
  background: linear-gradient(135deg, #0074d9, #00bfff);
  color: #fff;
  border: none;
  border-radius: 25px;
  cursor: pointer;
  transition: background 0.3s ease;
  margin-left: 1rem;  
  margin-top: 1rem; 
  margin-bottom: 1rem;  
  
}

button[type='submit']:hover {
  background: linear-gradient(135deg, #0056b3, #00a3e0);
}

.error-message {
  color: #e74c3c;
}

.results-container {
  margin-top: 1rem;
}

.result-item {
  padding: 1rem;
  background-color: #fff;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  margin-bottom: 1rem;
}

.result-title {
  font-size: 1.5rem;
  margin: 0 0 0.5rem 0;
  color: #00a3e0;
  transition: color 0.3s ease;
}

.result-title a {
  text-decoration: none;
  color: inherit;
}

.result-title a:hover {
  color: #0074d9;
}


.result-snippet {
  margin-top: 0;
}

.spinner {
  display: flex;
  justify-content: center;
  align-items: center;
}

.spinner > div {
  width: 10px;
  height: 10px;
  background-color: #0074d9;
  border-radius: 100%;
  display: inline-block;
  animation: bounce 1.4s infinite ease-in-out both;
}

.spinner .bounce1 {
  animation-delay: -0.32s;
}

.spinner .bounce2 {
  animation-delay: -0.16s;
}

@keyframes bounce {
  0%, 100% {
    transform: scale(0);
  }
  50% {
    transform: scale(1);
  }
}

 
.dark-theme {
  background-color: #282c34;
  color: #fff;
}

.dark-theme #search-input {
  background-color: #454545;
  color: #fff;
  border-color: #fff;
}

.dark-theme #search-input:focus {
  border-color: #0074d9;
}
 

.dark-theme button[type='submit'] {
  background-color: #0074d9;
}

.dark-theme .error-message {
  color: #ff6b6b;
}

.dark-theme .result-item {
  background-color: #454545;
  color: #fff;
}
.pagination {
  margin-top: 1rem;
  text-align: center;
}

.pagination button {
  font-size: 1rem;
  padding: 0.5rem 1rem;
  background-color: #0074d9;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  margin-right: 0.5rem;
  margin-left: 0.5rem;
  
}

.pagination button:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}
</style>
