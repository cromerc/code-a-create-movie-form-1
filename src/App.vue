<script setup>
import { ref } from "vue";
import { StarIcon } from "@heroicons/vue/24/solid";
import { items } from "./movies.json";
const movies = ref(items);

const genres = ["Action", "Comedy", "Crime", "Drama"];

const addMovieVisible = ref(false);

const name = ref("");
const description = ref("");
const image = ref("");
const selectedGenre = ref([]);
const inTheaters = ref(false);

function updateRating(movieIndex, rating) {
  movies.value[movieIndex].rating = rating;
}

function showAddMovie() {
  addMovieVisible.value = true;
}

function cancelAddMovie() {
  clearInput();
  addMovieVisible.value = false;
}

function addMovie() {
  clearInput();
}

function clearInput() {
  name.value = "";
  description.value = "";
  image.value = "";
  selectedGenre.value = [];
  inTheaters.value = false;
}
</script>

<template>
  <div class="fixed z-10 left-1/2 top-1/2 w-1/4" v-show="addMovieVisible">
    <div class="p-2 relative text-white bg-gray-200 dark:bg-gray-900 -translate-x-1/2 -translate-y-1/2 rounded-lg">
      <div class="grid grid-cols-1 font-bold m-2 gap-2">
        <div>Name</div>
        <div><input class="text-black w-full" type="text" v-model="name" /></div>

        <div class="pt-4">Description</div>
        <div><textarea class="text-black w-full" v-model="description" /></div>

        <div class="pt-4">Image</div>
        <div><input class="text-black w-full" type="text" v-model="image" /></div>

        <div class="pt-4">Genres</div>
        <div>
          <select class="text-black w-full" v-model="selectedGenre" multiple>
            <option v-for="genre in genres">{{ genre }}</option>
          </select>
        </div>

        <div class="pt-4">In theaters <input class="text-black" type="checkbox" v-model="inTheaters" /></div>

        <div class="grid grid-cols-2 font-bold pt-4">
          <div class="flex justify-center">
            <button @click="cancelAddMovie"
              class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">Cancel</button>
          </div>
          <div class="flex justify-center">
            <button @click="addMovie"
              class="bg-green-500 hover:bg-green-700 text-white font-bold py-2 px-4 rounded">Create</button>
          </div>
        </div>
      </div>
    </div>
  </div>
  <div class="app flex flex-col space-y-4">
    <div class="">
      <button @click="showAddMovie" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">Add
        Movie</button>
    </div>
    <div class="movie-list">
      <div class="movie-item" v-for="(movie, movieIndex) in movies" :key="movie.id">
        <div class="movie-item-image-wrapper">
          <div class="movie-item-star-wrapper">
            <StarIcon id="rating" class="movie-item-star-rating-icon"
              :class="[movie.rating ? 'text-yellow-500' : 'text-gray-500']" />
            <div class="movie-item-star-content-wrapper">
              <span v-if="movie.rating" id="rating-stars" class="movie-item-star-content-rating-rated">
                {{ movie.rating }}
              </span>
              <span v-else class="movie-item-star-content-rating-not-rated">
                -
              </span>
            </div>
          </div>
          <img :src="movie.image" class="movie-item-image" alt="" />
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
              Rating: ({{ movie.rating }}/5)
            </span>

            <div class="movie-item-star-icon-wrapper">
              <button v-for="star in 5" :key="star" class="movie-item-star-icon-button" :class="[
                  star <= movie.rating ? 'text-yellow-500' : 'text-gray-500',
                ]" :disabled="star === movie.rating" @click="updateRating(movieIndex, star)">
                <StarIcon class="movie-item-star-icon" />
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
