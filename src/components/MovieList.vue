<template>
  <div class="">
    <div class="container">
      <b-jumbotron class="search-movie-jumbotron">
        <b-row>
          <b-col cols="12" class="title-container">
            <h1>Vue Movies-DB</h1>
          </b-col>
          <b-col cols="12" class="search-input-container">
            <b-input-group class="mb-3 mt-3">
              <b-form-input
                id="input-large"
                size="lg"
                v-on:keyup.enter="searchMovies()"
                v-model="searchdata"
                placeholder="Search Movies"
              ></b-form-input>
              <b-input-group-append>
                <b-button
                  size="lg"
                  variant="outline-primary"
                  v-on:click="searchMovies()"
                >
                  Search
                </b-button>
              </b-input-group-append>
            </b-input-group>
          </b-col>
        </b-row>
      </b-jumbotron>

      <b-jumbotron class="list-movie-jumbotron text-center">
        <h5>results</h5>
        <div class="row">
          <div
            v-for="movie in moviesdata.Search"
            :key="movie.Title"
            class="card"
          >
            <b-button
              class="movie-btn"
              variant="light"
              v-b-modal.modal-1
              v-on:click="viewSummary(movie.Title)"
            >
              <div class="text-center">
                <img
                  class="card-img-top"
                  :src="movie.Poster"
                  alt="Card image cap"
                  style="width: 200px; height: 230px"
                />
                <div class="card-body">
                  <h5 class="">{{ movie.Title }}</h5>
                  <p class="card-text"></p>
                </div>
              </div>
            </b-button>
          </div>
          <div v-for="movie in newmoviesdata" :key="movie.title" class="card">
            <div class="text-center">
              <img
                class="card"
                :src="movie.posterurl"
                alt="Card image cap"
                style="width: 200px; height: 230px"
              />
              <div class="card-body">
                <h5 class="">{{ movie.title }}</h5>
                <p class="card-text"></p>
              </div>
            </div>
          </div>
        </div>
      </b-jumbotron>
    </div>
    <div class="">
      <div class="col-4 offset-4 container">
        <h3>Add a Movie</h3>
        <b-col cols="12" class="add-movie-input-container">
          <b-form-input
            class="mb-2"
            v-model="movietitle"
            placeholder="Title"
          ></b-form-input>
          <b-form-input
            class="mb-2"
            v-model="movieyear"
            placeholder="Year"
          ></b-form-input>
          <b-form-input
            class="mb-2"
            v-model="movieposterlink"
            placeholder="Poster URL"
          ></b-form-input>
          <b-button variant="outline-primary" v-on:click="addMovie()">
            Submit
          </b-button>
        </b-col>
      </div>
    </div>
    <div class="search-movie-jumbotron"></div>
    <div>
      <b-modal ok-only id="modal-1" title="Summary">
        <h5 class="my-4">{{ moviesummary.Plot }}</h5>
      </b-modal>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchdata: "",
      moviesdata: [],
      moviesummary: [],
      newmoviesdata: [],
      movietitle: null,
      movieyear: null,
      movieposterlink: null,
    };
  },
  methods: {
    searchMovies() {
      fetch(
        "https://www.omdbapi.com/?apikey=e0620bd4&s=" +
          this.searchdata +
          "&plot=full"
      )
        .then((response) => response.json())
        .then((data) => (this.moviesdata = data));
    },
    viewSummary(Title) {
      fetch(
        "https://www.omdbapi.com/?apikey=e0620bd4&t=" + Title + "&plot=full"
      )
        .then((response) => response.json())
        .then((data) => (this.moviesummary = data));
    },
    addMovie() {
      var movie = {
        title: this.movietitle,
        year: this.movieyear,
        posterurl: this.movieposterlink,
      };
      this.newmoviesdata.push(movie);
    },
  },
};
</script>

<style scoped>
.list-group {
  margin-bottom: 15px;
}
.list-group-item:hover {
  background-color: #eee;
}
.search-movie-jumbotron {
  background-color: white;
  padding: 5rem 2rem 1rem 2rem;
}
.list-movie-jumbotron {
  background-color: white;
  padding: 0rem 2rem 1rem 2rem;
}
.card {
  width: 234.4px;
  margin: 14px;
}
.movie-btn {
  padding: 0.75rem;
}
.modal-dialog {
  max-width: 500px;
  margin: 1.75rem auto;
  margin-top: 4rem;
}
</style>