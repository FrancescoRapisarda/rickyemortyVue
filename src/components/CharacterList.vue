<template>
  <div class="app">
    <div class="search-container">
      <input type="text" v-model="searchTerm" placeholder="Digita nome personaggio..." />
    </div>
    <div class="characters">
      <div v-for="character in filteredCharacters" :key="character.id" class="character-card">
        <img :src="character.image" :alt="character.name" class="character-image" />
        <p class="character-name">{{ character.name }}</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'

const characters = ref([])
const searchTerm = ref('')

const fetchCharacters = async () => {
  try {
    const response = await fetch('https://rickandmortyapi.com/api/character')
    const data = await response.json()
    characters.value = data.results
  } catch (error) {
    console.error('Error fetching characters:', error)
  }
}

// Filter characters based on search term
const filteredCharacters = computed(() => {
  return characters.value.filter((character) =>
    character.name.toLowerCase().includes(searchTerm.value.toLowerCase())
  )
})

onMounted(fetchCharacters)
</script>

<style scoped>
.app {
  display: flex;
  flex-direction: column;
  align-content: center;
  align-items: center;
  width: 90vw;
}

.search-container {
  margin-top: 30px;
  margin-bottom: 50px;
}

.character-card {
  display: inline-block;
  width: 200px;
  margin: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  padding: 10px;
}

.character-image {
  width: 100%;
  border-radius: 5px;
}

.character-name {
  text-align: center;
  margin-top: 10px;
}
</style>
