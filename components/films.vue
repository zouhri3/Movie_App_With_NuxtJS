<template>
  <v-container class="my-11">
    <v-form class="d-flex fill-height mb-4">
      <v-col md="6" class="search pa-0">
        <v-text-field
          class="rounded-r-0"
          dense
          outlined
          height="45"
          placeholder="search"
          v-model.lazy="search"
          color="red"
        >
        </v-text-field>
        <v-btn
          absolute
          z-index="2"
          :disabled="!search"
          color="red"
          height="45"
          class="rounded-l-0 btn"
          @click="search = null"
        >
          clear search</v-btn
        >
      </v-col>
    </v-form>
    <v-row v-if="$fetchState.pending">
      <v-col
        v-for="(item, index) in 8"
        :key="index"
        cols="12"
        sm="6"
        md="4"
        lg="3"
      >
        <v-sheet>
          <v-skeleton-loader
            class="mx-auto"
            max-width="300"
            type="card"
          ></v-skeleton-loader>
        </v-sheet>
      </v-col>
    </v-row>

    <v-row v-else>
      <v-col
        cols="12"
        sm="6"
        md="4"
        lg="3"
        v-for="(movie, index) in searchMovie"
        :key="index"
      >
        <v-card>
          <v-hover v-slot="{ hover }">
            <v-img
              :src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`"
              :alt="movie.title"
              class="relative"
              height="375"
            >
              <p
                class="
                  vote_average
                  red
                  pa-3
                  rounded-l-0 rounded
                  font-weight-bold
                  yellow--text
                "
              >
                {{
                  movie.vote_average.toString().length > 2
                    ? movie.vote_average
                    : movie.vote_average + ".0"
                }}
              </p>
              <v-expand-transition>
                <p
                  v-if="hover"
                  class="
                    transition-fast-in-fast-out
                    red
                    darken-4
                    v-card--reveal
                    white--text
                    px-3
                    my-0
                    pt-14
                  "
                  style="height: 100%"
                >
                  {{ movie.overview.slice(0, 200) }}
                  <span v-if="movie.overview.length > 200">...</span>
                </p>
              </v-expand-transition>
            </v-img>
          </v-hover>

          <v-card-title>
            {{ movie.title.slice(0, 20) }}
            <span v-if="movie.title.length > 25">...</span>
          </v-card-title>
          <v-card-subtitle class="text-uppercase pb-1">
            released:
            {{
              new Date(movie.release_date).toLocaleString("en-us", {
                month: "long",
                day: "numeric",
                year: "numeric",
              })
            }}
          </v-card-subtitle>
          <v-card-actions>
            <v-btn class="red my-1 white--text" nuxt :to="`/movies/${movie.id}`"
              >get more info</v-btn
            >
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
import axios from "axios";
export default {
  data() {
    return {
      search: null,
      movies: [],
    };
  },

  async fetch() {
    await this.getMovies();
    return;
  },

  methods: {
    async getMovies() {
      const data = axios.get(
        `https://api.themoviedb.org/3/movie/now_playing?api_key=37ed43a4f8eaa2abd75f9283692947bc&language=en-US&page=1`
      );
      const result = await data;
      result.data.results.forEach((movie) => {
        this.movies.push(movie);
      });
    },
  },
  computed: {
    searchMovie() {
      if (!this.search || !this.movies) return this.movies || [];
      return this.movies.filter((item) => {
        const patern = new RegExp(this.search, "i");
        return (
          item.title.match(patern) || item.vote_average.toString().match(patern)
        );
      });
    },
  },
};
</script>

<style lang="scss">
.search {
  position: relative;

  .btn {
    right: 0;

    top: 0;
  }
}
.vote_average {
  position: absolute;

  top: 0;
  left: 0;
  z-index: 22;
}
.v-card--reveal {
  align-items: center;
  bottom: 0;
  justify-content: center;
  opacity: 0.6;
  position: absolute;
  width: 100%;
}
</style>