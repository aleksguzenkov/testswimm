<template>
  <div class="movies-list-item">
    <img :src="movie.image.medium" :alt="movie.name" class="movies-list-item__image" />
    <div class="movies-list-item__content">
      <h3 class="movies-list-item__name">{{ movie.name }}</h3>
      <p class="movies-list-item__rating">Rating</p>
      <p class="movies-list-item__genre">Genre</p>
    </div>
    <div>
      <button class="movies-list-item__like" @click="emit('like')">Like {{ isLiked }}</button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue'

const props = defineProps<{
  movie: Object
  favIds: []
}>()

const emit = defineEmits<{
  (e: 'like'): void
}>()

const isLiked = computed(() => {
  console.log(props.favIds, props.movie.id)

  return props.favIds.includes(props.movie.id)
})
</script>

<style scoped>
.movies-list-item {
  display: grid;
  grid-template-columns: 70px 1fr min-content;
  padding: 10px;
  box-shadow: -8px 10px 5px 0px rgba(0, 0, 0, 0.1);
  margin-bottom: 10px;
  border-radius: 5px;
}

.movies-list-item__image {
  max-width: 70px;
}

.movies-list-item__content {
  padding-left: 15px;
  display: grid;
  grid-template-rows: min-content 1fr min-content;
  max-height: 100px;
}

.movies-list-item__name,
.movies-list-item__rating,
.movies-list-item__genre {
  margin: 0;
}

.movies-list-item__like {
  border: 0;
  background: transparent;
  display: flex;
  box-shadow: 2px 2px 5px 0px rgba(0, 0, 0, 0.1);
}
</style>
