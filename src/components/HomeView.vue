
<script>
import axios from "axios";
// import Navbar from "@/components/Navbar.vue";
import "primeicons/primeicons.css";
export default {
  data() {
    return {
      query: "",
      data: [],
    };
  },
  methods: {
    async getMovies() {
      await axios
        .get(
          `https://www.omdbapi.com/?i=tt3896198&apikey=cb5171b9&s=${this.query}`
        )
        .then((response) => (this.data = response.data.Search))
        .catch((err) => console.error(err));
    },
    truncateString(str, num) {
      if (str.length <= num) {
        return str;
      }
      return str.slice(0, num) + "...";
    },
  },
};
</script>



<template>
  <!-- Navbar -->
  <div class="flex items-center justify-between px-5 mt-5 lg:px-48">
    <div class="font-poppins text-white flex items-center gap-8">
      <router-link to="/"
        ><h1
          class="text-2xl lg:text-3xl font-semibold text-green-600 underline cursor-pointer"
        >
          Movie Hunter
        </h1></router-link
      >
    </div>
    <div class="flex items-center">
      <form @submit.prevent="getMovies">
        <input
          type="search"
          class="rounded bg-transparent border border-neutral-500 text-white px-2 mr-3 lg:px-10 lg:py-2"
          placeholder="Search..."
          v-model="query"
        />
        <button type="submit">
          <i class="pi pi-search text-white lg:text-xl"></i>
        </button>
      </form>
    </div>
  </div>
  <div class="border border-b-neutral-900 mt-5"></div>

  <!-- Main Body -->
  <div class="text-white">
    <div v-if="data && data.length > 0">
      <ul class="flex flex-wrap justify-around mt-8">
        <li v-for="(movie, index) in data" :key="index" class="py-4">
          <img
            class="rounded-lg w-[160px] lg:w-[200px]"
            :src="movie.Poster"
            :alt="movie.Title"
          />
          <h1 class="font-semibold text-xl lg:text-2xl">
            {{ truncateString(movie.Title, 14) }}
          </h1>
          <div class="flex gap-5">
            <h2 class="uppercase font-medium">{{ movie.Type }}</h2>
            <p>{{ movie.Year }}</p>
          </div>
        </li>
      </ul>
    </div>
    <div v-else>
      <h1 class="font-bold text-2xl text-center mt-4 lg:text-3xl">
        No Movies Found.
      </h1>
    </div>
  </div>
</template>
