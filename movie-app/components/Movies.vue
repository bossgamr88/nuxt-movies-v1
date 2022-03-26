<template>
  <div>
    <div v-if="isLoading" class="loading">
      <v-progress-circular :size="70" :width="7" color="dark" indeterminate>
      </v-progress-circular>
    </div>
    <div v-else>
      <v-container>
        <div class="search">
          <v-text-field placeholder="Search Here..."></v-text-field>
          <v-btn style="margin-left:20px;" color="error">Clear</v-btn>
        </div>
        <v-row>
          <v-col
            xl="3"
            md="4"
            sm="6"
            xs="12"
            v-for="movie in movies"
            :key="movie.id"
          >
            <v-card>
              <!-- <v-img
                class="image"
                height="250"
                src="https://images.unsplash.com/photo-1512070853659-f08f0efae470?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1070&q=80"
              ></v-img> -->
              <v-img
                class="image"
                height="250"
                :src="`https://www.themoviedb.org/t/p/w500/${movie.poster_path}`"
              >
              </v-img>
              <v-card-title>
                <!-- <h4>Moive title</h4> -->
                <!-- <h4>{{ movie.title }}</h4> -->
                <h4>{{ movie.title.slice(0,10) }} {{movie.title.length > 10 ? '.....' : ''}}</h4>
              </v-card-title>
              <v-card-text>
                <nuxt-link
                  :to="{ name: 'movieid', params: { movieid: movie.id } }"
                >
                  <v-btn color="primary">Read More</v-btn>
                </nuxt-link>
              </v-card-text>
              <!-- <span id="vote">3.5</span> -->
              <span id="vote">{{ movie.vote_average }}</span>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      movies: [],
      isLoading: false,
      searchMoive:[],
      search:''
    };
  },
  methods: {
    async getMovies() {
      this.isLoading = true;
      // https://api.themoviedb.org/3/movie/550?api_key=d3de84b80bb26bc66fdf225d5f5593fd
      // https://www.themoviedb.org/movie/550-fight-club?language=th
      // https://api.themoviedb.org/3/movie/550?api_key=d3de84b80bb26bc66fdf225d5f5593fd&language=th
      const response = await axios.get(
        "https://api.themoviedb.org/3/movie/now_playing?api_key=d3de84b80bb26bc66fdf225d5f5593fd&language=th&page=1"
      );
      // console.log(response);
      console.log(response.data.results);
      this.movies = response.data.results;
      this.isLoading = false;
    },
      async searchMovies() {
      this.isLoading = true;
      const response = await axios.get(
        "https://api.themoviedb.org/3/movie/now_playing?api_key=d3de84b80bb26bc66fdf225d5f5593fd&language=th&page=1"
      );
      this.searchMoive = response.data.results;
      this.isLoading = false;
    },
  },
  async fetch() {
    await this.getMovies();
  },
};
</script>

<style>
.image {
  position: relative;
}
#vote {
  position: absolute;
  top: 10px;
  left: 10px;
  background-color: red;
  padding: 5px;
  border-radius: 10px;
}
.loading {
  display: flex;
  justify-content: center;
  margin: 50px 0px;
}
.search{
  display: flex;
  width: 50%;
  margin:20px 0px;
  align-items: center;
}
</style>