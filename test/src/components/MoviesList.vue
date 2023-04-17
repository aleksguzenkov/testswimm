<template>
  <ul>
    <li v-for="movie in movies" :key="movie.id" class="movies-list__item">
      favIds: {{ favIds }}
      <MoviesListItem :movie="movie" @like="emit('like', movie)" :fav-ids="favIds" />
    </li>
  </ul>
</template>

<script setup lang="ts">
import MoviesListItem from '@/components/MoviesListItem.vue'
const props = defineProps<{
  movies: []
  favIds: []
}>()

const emit = defineEmits<{
  (e: 'like', movie: Object): void
}>()
</script>

<style scoped>
.movies-list__item {
  list-style: none;
  max-width: 300px;
}
</style>
