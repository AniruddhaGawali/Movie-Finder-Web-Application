<script>
import axios from "axios";

export default {
  name: "home",
  mounted() {
    // fetch up the movies from json file
    axios({
      method: "GET",
      url: "assets/json/movies.json",
    }).then(
      (response) => {
        this.allMovies = JSON.parse(JSON.stringify(response.data));
      },
      (error) => {
        console.error(error);
      }
    );

    // fetch up the Favorite Movies from local storage
    localStorage.getItem("favMovies") &&
      (this.favMovies = JSON.parse(localStorage.getItem("favMovies")));
  },
  data() {
    return {
      allMovies: [],
      favMovies: [],
      genreFilter: [],
      search: "",
      isFavOn: false,
    };
  },
  components: {
    // searchbar,
    SearchBar: () =>
      import(/* webpackChunkName: "searchbar" */ "../components/searchBar.vue"),

    // movieCard,
    MovieCard: () =>
      import(/* webpackChunkName: "movieCard" */ "../components/movieCard.vue"),
  },
  methods: {
    // add the search value to the seach reactive data
    changeSearch(search) {
      this.search = search;
    },

    // add the factorite movie to the favMovies reactive data and save it into local storage
    addFavoriteMovie(movie) {
      this.favMovies.push(movie);
      localStorage.setItem("favMovies", JSON.stringify(this.favMovies));
    },

    // remove the factorite movie from the favMovies reactive data and save it into local storage
    removeFavoriteMovie(movie) {
      this.favMovies = this.favMovies.filter((fav) => fav.id !== movie.id);
      localStorage.setItem("favMovies", JSON.stringify(this.favMovies));
    },

    // toggle the isFavOn reactive data to show/hide the favorite movies
    toggleFav() {
      this.isFavOn = !this.isFavOn;
    },

    // add the genre to the genreFilter reactive data
    addFilter(genre) {
      if (!this.genreFilter.includes(genre)) {
        this.genreFilter.push(genre);
      } else {
        this.genreFilter = this.genreFilter.filter((g) => g !== genre);
      }
    },

    // clear all the genreFilter reactive data
    clearFilter() {
      this.genreFilter = [];
    },
  },

  computed: {
    // showCaseMoveis is the movies that will be shown in the home page
    showCaseMoveis() {
      if (this.isFavOn) {
        if (!this.search) {
          if (this.genreFilter.length !== 0) {
            // return the favorite movies that have the genreFilter
            return this.favMovies.filter((movie) =>
              movie.genres.some((genre) => this.genreFilter.includes(genre))
            );
          }

          // return the favorite movies without the genreFilter
          return this.favMovies;
        }
        return this.favMovies.filter((movie) => {
          if (this.genreFilter.length !== 0) {
            // return the favorite movies that have the genreFilter and the search value
            return (
              movie.title.toLowerCase().includes(this.search.toLowerCase()) &&
              movie.genres.some((genre) => this.genreFilter.includes(genre))
            );
          }

          // return the favorite movies that have the search value
          return movie.title.toLowerCase().includes(this.search.toLowerCase());
        });
      } else {
        if (!this.search) {
          if (this.genreFilter.length !== 0) {
            // return all the movies that have the genreFilter
            return this.allMovies.filter((movie) =>
              movie.genres.some((genre) => this.genreFilter.includes(genre))
            );
          }
          // return all the movies without the genreFilter
          return this.allMovies;
        }
        return this.allMovies.filter((movie) => {
          if (this.genreFilter.length !== 0) {
            return (
              // return all the movies that have the genreFilter and the search value
              movie.title.toLowerCase().includes(this.search.toLowerCase()) &&
              movie.genres.some((genre) => this.genreFilter.includes(genre))
            );
          }
          // return all the movies that have the search value
          return movie.title.toLowerCase().includes(this.search.toLowerCase());
        });
      }
    },

    // allGenres is all the genres that the movies have
    allGenres() {
      const genres = [];
      this.allMovies.forEach((movie) => {
        movie.genres.forEach((genre) => {
          if (!genres.includes(genre)) {
            genres.push(genre);
          }
        });
      });
      return genres;
    },
  },
};
</script>

<template>
  <div>
    <!--  the header of the home page -->
    <h1 class="herotitle">
      <img
        src="https://media.giphy.com/media/LITTI5SuHLpu9hvdFL/giphy.gif"
        alt="🍿"
        class="title-img"
        v-if="isFavOn"
      />

      <img
        src="https://media.giphy.com/media/VeBcPQ8afEgvTWQMt3/giphy.gif"
        alt="🎬"
        class="title-img"
        v-else
      />
      <span v-if="isFavOn" style="color: #ffc107"> Favorite Movie !</span>
      <span v-else> Movie Finder !</span>
    </h1>

    <!-- the search bar -->
    <SearchBar
      :changeSearch="changeSearch"
      :allGenres="allGenres"
      :filter="genreFilter"
      :isFavOn="isFavOn"
      @show-fav="toggleFav"
      @change-filter="addFilter"
      @remove-all="clearFilter"
    />

    <!-- the movies grid -->
    <div class="continer grid" v-if="showCaseMoveis.length > 0">
      <MovieCard
        v-for="movie in showCaseMoveis"
        :movie="movie"
        :key="movie.id"
        :favMovies="favMovies"
        @add-favorite-movie="addFavoriteMovie"
        @remove-favorite-movie="removeFavoriteMovie"
      />
    </div>

    <!-- if there is no movies to show -->
    <div v-else>
      <img
        src="https://media.giphy.com/media/tZrhmYNNISQiCrlVSs/giphy-downsized-large.gif"
        alt=""
      />
      <h3 v-if="isFavOn">Add Something to Favorite</h3>
    </div>
  </div>
</template>

<style scoped>
.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-auto-rows: minmax(100px, auto);
  align-content: center;
  justify-items: center;
  grid-gap: 5rem;
  padding-bottom: 5rem;
}

.herotitle {
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  margin: 5rem 0rem;
  font-size: 4rem;
  font-weight: bold;
}

.title-img {
  width: 5rem;
  height: 5rem;
  margin-right: 1rem;
  object-fit: fill;
}

@media screen and (max-width: 1440px) {
  .grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media screen and (max-width: 1080px) {
  .grid {
    grid-template-columns: repeat(1, 1fr);
  }
}

@media screen and (max-width: 768px) {
  .herotitle {
    font-size: 2rem;
  }
}
</style>
