<script setup>
import { PencilIcon, StarIcon, TrashIcon, EyeIcon } from "@heroicons/vue/24/solid";
import {useRouter} from "vue-router";

const props = defineProps(['movie'])
const emit = defineEmits(['update:rating', 'edit', 'remove'])


const updateRating = (rating) => {
  emit('update:rating', { id: props.movie.id, rating })
}
const removeMovie = () => {
  emit('remove', props.movie.id)
}

const editMovie = () => {
  emit('edit', props.movie)
}
</script>

<template>
  <div class="movie-item">
    <div class="movie-item-image-wrapper">
      <div class="movie-item-star-wrapper">
        <StarIcon class="movie-item-star-rating-icon" :class="[movie.rating ? 'text-yellow-500' : 'text-gray-500']" />
        <div class="movie-item-star-content-wrapper">
          <span v-if="movie.rating" :class="[
            movie.rating
              ? 'movie-item-star-content-rating-rated'
              : 'movie-item-star-content-rating-not-rated',
          ]">
            {{ movie.rating ?? "-" }}
          </span>
        </div>
      </div>
      <img :src="movie.image" class="movie-item-image" :alt="movie.name" />
    </div>

    <div class="movie-item-content-wrapper">
      <div class="movie-item-title-wrapper">
        <h3 class="movie-item-title">{{ movie.name }}</h3>
        <div class="movie-item-genres-wrapper">
          <span v-for="genre in movie.genres" :key="`${movie.id}-${genre}`" class="movie-item-genre-tag">{{ genre
            }}</span>
        </div>
      </div>
      <div class="movie-item-description-wrapper">
        <p class="movie-item-description">{{ movie.description }}</p>
      </div>
      <div class="movie-item-rating-wrapper">
        <span class="movie-item-rating-text">
          Rating: ({{ movie.rating ?? "-" }}/5)
        </span>

        <button v-for="r in 5" @click="updateRating(r)" :disabled="movie.rating === r"
          :class="{ 'cursor-not-allowed': movie.rating === r }">
          <StarIcon class="movie-item-star-icon" :class="{
            '!text-gray-500': !r || r > movie.rating,
          }" />
        </button>
      </div>
      <div class="movie-item-crud-wrapper">
        <button
            class="border border-black rounded-full p-1"
            @click="editMovie()"
            title="Edit movie"
        >
          <PencilIcon class="w-[18px] text-black" />
        </button>
        <button
            class="border border-red-600 rounded-full p-1"
            title="Delete movie"
            @click="removeMovie()"
        >
          <TrashIcon class="w-[18px] text-red-600" />
        </button>
        <RouterLink
            class="border border-black rounded-full p-1"
            title="Go to movie"
            :to="{name: 'movie', params: {id: movie.id}}"
        >
          <EyeIcon class="w-[18px] text-black" />
        </RouterLink>
      </div>
    </div>
  </div>
</template>