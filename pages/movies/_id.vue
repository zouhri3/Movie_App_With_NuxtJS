<template>
  <v-container>
    <v-sheet
      v-if="$fetchState.pending"
      class="d-flex align-center ma-0"
      min-height="100vh"
    >
      <v-skeleton-loader
        class="mx-auto"
        min-width="300"
        type="card"
      ></v-skeleton-loader>
    </v-sheet>
    <v-card v-else flat>
      <v-btn class="red white--text my-6" min-width="150" nuxt to="/"
        >back</v-btn
      >
      <v-row align="center">
        <v-col sm="6" md="4" cols="12">
          <v-img
            :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`"
            :aspect-ratio="16 / 20"
            class="rounded"
          >
          </v-img>
        </v-col>
        <v-col sm="6" md="8" cols="12">
          <v-card-title class="red--text">
            {{ movie.title }}
          </v-card-title>
          <v-card-subtitle class="py-2">
            <span
              class="
                text-capitalize
                font-italic font-weight-bold
                text-decoration-underline
                mr-3
                d-inline-block
                warning--text
              "
            >
              Tagline:
            </span>

            {{ movie.tagline }}
          </v-card-subtitle>
          <v-card-subtitle class="py-2">
            <span
              class="
                text-capitalize
                font-italic font-weight-bold
                text-decoration-underline
                mr-3
                d-inline-block
                warning--text
              "
            >
              released:
            </span>
            {{
              new Date(movie.release_date).toLocaleString("en-us", {
                month: "long",
                day: "numeric",
                year: "numeric",
              })
            }}
          </v-card-subtitle>
          <v-card-subtitle class="py-2">
            <span
              class="
                text-capitalize
                font-italic font-weight-bold
                text-decoration-underline
                mr-3
                d-inline-block
                warning--text
              "
              >duration:</span
            >
            {{ movie.runtime }} minutes
          </v-card-subtitle>
          <v-card-text
            ><span
              class="
                text-capitalize
                font-italic font-weight-bold
                text-decoration-underline
                mr-3
                d-inline-block
                red--text
              "
              >overview :</span
            >
            {{ movie.overview }}
          </v-card-text>
        </v-col>
      </v-row>
    </v-card>
  </v-container>
</template>

<script>
import axios from "axios";

export default {
  name: "movie",

  data() {
    return {
      movie: [],
    };
  },

  async fetch() {
    await this.getSingleMovie();
  },

  methods: {
    async getSingleMovie() {
      const data = axios.get(
        `https://api.themoviedb.org/3/movie/${this.$route.params.id}?api_key=37ed43a4f8eaa2abd75f9283692947bc&language=en-US`
      );
      const result = await data;
      this.movie = result.data;
    },
  },
};
</script>

