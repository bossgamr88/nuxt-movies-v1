<template>
  <div>
    <!-- {{ $route.params }} -->
    <!-- {{ $route.params.movieid }} -->
    <v-container>
     <Nuxt-link to="/"> <v-btn color="error">
        Go Back
      </v-btn>
    </Nuxt-link>
      <div v-if="isLoading" class="loading">
        <v-progress-circular :size="70" :width="7" color="dark" indeterminate>
        </v-progress-circular>
      </div>
      <div v-else>
        <v-row style="padding: 10px 0px">
          <v-col xl="6" md="6" sm="6" xs="12">
            <v-card>
              <v-img
                id="image"
                height="300"
                :src="`https://www.themoviedb.org/t/p/w500/${movies.poster_path}`"
              ></v-img>
            </v-card>
          </v-col>
          <v-col xl="6" md="6" sm="6" xs="12">
            <!-- <h3>title</h3> -->
            <h3>{{ movies.title }}</h3>
            <h4>
              <span style="text-decoration: underline">
                <i>Tag Line:</i>
              </span>
              <!-- movie tag -->
              {{ movies.tagline }}
            </h4>
            <h4>
              <span style="text-decoration: underline">
                <i>Realse:</i>
              </span>
              <!-- date -->
              {{ movies.release_date }}
            </h4>
            <h4>
              <span style="text-decoration: underline">
                <i>Duration:</i>
              </span>
              <!-- runtime -->
              {{ movies.release_date }}
            </h4>
            <p>
              <span style="text-decoration: underline">
                <i>Overview:</i>
              </span>
              <!-- Lorem ipsum dolor sit amet, consectetur adipisicing elit. Unde
              ipsa laboriosam ratione, optio fuga voluptas. Iste tempore
              maiores, ipsum earum repudiandae ab sint temporibus laborum harum
              itaque neque officiis! Harum. -->
              {{ movies.overview }}
            </p>
          </v-col>
        </v-row>
      </div>
    </v-container>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      movies: [],
      isLoading: false,
    };
  },
  methods: {
    async getSingleMovies() {
      this.isLoading = true;
      const response = await axios.get(
        `https://api.themoviedb.org/3/movie/${this.$route.params.movieid}?api_key=d3de84b80bb26bc66fdf225d5f5593fd&language=th`
      );
      //   this.movies = response.data.results;
      this.movies = response.data;
      console.log(response.data);
      this.isLoading = false;
      console.log(this.movies);
    },
  },
  async fetch() {
    await this.getSingleMovies();
  },
};
</script>

<style>
.loading {
  display: flex;
  justify-content: center;
  margin: 50px 0px;
}
</style>