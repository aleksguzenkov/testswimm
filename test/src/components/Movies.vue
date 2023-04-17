<template>
  <h1>TV Seriest Database</h1>
  <input type="text" v-model="search" />
  <div class="movies__columns">
    <div class="movies__results">
      <h2>Search Results</h2>
      <MoviesList :movies="allMovies" @like="onLike" :fav-ids="favIds" />
    </div>
    <div class="movies__favorites">
      <h2>My Favorites</h2>
      <MoviesList :movies="favorites" @like="onLike" :fav-ids="favIds" />
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed, ref, watch } from 'vue'
import MoviesList from '@/components/MoviesList.vue'
import { useDebounce, useStorage } from '@vueuse/core'

const search = ref('')
const debouncedSearch = useDebounce(search, 500)
const allMovies = ref([])

const favorites = useStorage('favorites', []) // returns Ref<number>

const fetchMovies = async () => {
  try {
    const response = await fetch(
      `https://api.tvmaze.com/search/shows?q=${search.value.toLowerCase()}`
    )
    const data = await response.json()
    console.log('data', data)
    allMovies.value = data.map(({ show }) => show)
  } catch (error) {
    console.error(error)
  }
}

const favIds = computed(() => favorites.value.map(({ id }) => id))

const onLike = (movie: Object) => {
  favorites.value = favIds.value.includes(movie.id)
    ? favorites.value.filter((m) => m.id != movie.id)
    : [...favorites.value, movie]
}

watch(
  debouncedSearch,
  async () => {
    await fetchMovies()
  },
  { immediate: true }
)
</script>

<style scoped>
.movies__columns {
  display: flex;
  width: 100%;
}

.movies__results,
.movies__favorites {
  flex: 1;
}
</style>
