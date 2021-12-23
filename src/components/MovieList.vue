<template>
  <div class="movie-list">
    <div class="list-container">
      <ul>
        <ol
          v-on:click="onMovieSelected(item)"
          v-for="(item, index) in movieList"
          :key="index"
          :class="{ active: item.id === id }"
        >
          {{
            item.title
          }}
        </ol>
      </ul>
    </div>
  </div>
</template>

<script lang='ts'>
import { defineComponent } from 'vue'
import { MovieData } from '../types'

export default defineComponent({
  name: "MovieList",
  props: {
    movieList: Array,
    id: Number,
  },
  methods: {
    onMovieSelected: function (data: MovieData) {
      this.$emit("set-selected-movie-id", data.id);
    },
  },
});
</script>

<style>
.movie-list {
  width: 25%;
  background-color: maroon;
  color: white;
  height: inherit;
  overflow: scroll;
}
.movie-list ul {
  padding: 0;
}

.movie-list ul ol {
  padding: 10px 0 10px 0;
  cursor: pointer;
}

.movie-list ul ol:hover {
  background: grey;
}

.movie-list ul ol.active {
  background: black;
}

.list-container {
  height: inherit;
}
</style>