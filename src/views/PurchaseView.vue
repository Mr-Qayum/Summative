<script setup>
import axios from "axios";
import { ref } from "vue";
import Modal from "../components/Modal.vue";
import { useRouter } from "vue-router";

const router = useRouter();
const genre = ref(28);
const search = ref("");
const movies = ref(null);
const showModal = ref(false);
const selectedRecordId = ref(0);

const toggleModal = (id) => {
  showModal.value = !showModal.value;
  selectedRecordId.value = id;
};

const getMovies = async () => {
  movies.value = (
    await axios.get("https://api.themoviedb.org/3/discover/movie", {
      params: {
        api_key: import.meta.env.VITE_TMDB_API_KEY,
        region: "US",
        language: "en",
        include_adult: false,
        with_genres: genre.value,
      },
    })
  ).data;
};

const getSearch = async () => {
  movies.value = (
    await axios.get("https://api.themoviedb.org/3/search/movie", {
      params: {
        api_key: import.meta.env.VITE_TMDB_API_KEY,
        region: "US",
        language: "en",
        include_adult: false,
        query: search.value,
      },
    })
  ).data;
}
</script>

<template>
  <div>
    <input type="search" placeholder="Enter search items" v-model="search" />
    <button @click="getSearch()">Search</button>
    <select v-model="genre">
      <option value="28">Action</option>
      <option value="10751">Family</option>
      <option value="878">Science Fiction</option>
      <option value="12">Adventure</option>
      <option value="14">Fantasy</option>
      <option value="10770">TV Movie</option>
      <option value="16">Animation</option>
      <option value="36">History</option>
      <option value="53">Thriller</option>
      <option value="35">Comedy</option>
      <option value="27">Horror</option>
      <option value="10752">War</option>
      <option value="80">Crime</option>
      <option value="10402">Music</option>
      <option value="37">Western</option>
      <option value="99">Documentary</option>
      <option value="9648">Mystery</option>
      <option value="18">Drama</option>
      <option value="10749">Romance</option>
    </select>
    <button @click="getMovies()">Get</button>
    <button @click="router.push('/cart')">Cart</button>
    <div v-if="movies" class="tiles">
      <div v-for="movie in movies.results" class="tile">
        <img
          :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`"
          @click="toggleModal(movie.id)"
        />
      </div>
    </div>
  </div>
  <Modal v-if="showModal" :id="selectedRecordId" @toggleModal="toggleModal()" />
</template>

<style scoped>
.tiles {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
}

img {
  width: 200px;
}
</style>
