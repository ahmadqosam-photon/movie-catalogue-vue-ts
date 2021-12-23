<template>
  <div class="homepage-container">
    <MovieList
      @set-selected-movie-id="setSelectedMovieId"
      :movieList="data"
      :id="selectedMovieId"
    />
    <MoviePreview
      :image="preview.image"
      :title="preview.title"
      :description="preview.desc"
    />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import axios, { AxiosResponse } from "axios";
import MovieList from "@/components/MovieList.vue";
import MoviePreview from "@/components/MoviePreview.vue";

import { BASE_IMAGE_URL } from "../constants";
import { MovieData } from '../types'

export default defineComponent({
  name: "Home Page",
  components: {
    MovieList,
    MoviePreview,
  },
  data() {
    return {
      preview: {},
      data: [],
      selectedMovieId: 0,
    };
  },
  mounted() {
    axios
      .get(
        "https://api.themoviedb.org/3/discover/movie?api_key=58817b1d581d4e9595c92a28c167872f&language=en-US&sort_by=popularity.desc"
      )
      .then((response: AxiosResponse) => {
        this.data = response.data.results;
      });
  },
  methods: {
    setSelectedMovieId(id: string) {
      this.selectedMovieId = id;
    },
  },
  watch: {
    data(movieList: MovieData[]) {
      movieList.length > 0 && this.setSelectedMovieId(movieList[0].id);
    },
    selectedMovieId(id: string) {
      const selectedMovie = this.data.find((movie) => movie.id === id);
      const { poster_path, title, overview } = selectedMovie;
      this.preview = {
        image: `${BASE_IMAGE_URL}${poster_path}`,
        title,
        overview,
      };
    },
  },
});
</script>

<style >
.homepage-container {
  display: flex;
  justify-content: space-evenly;
  height: inherit;
}
</style>