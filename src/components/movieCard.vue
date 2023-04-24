<script>
export default {
  name: "MovieCard",

  // reactive data
  props: {
    movie: {
      type: Object,
      required: true,
    },
    favMovies: {
      type: Array,
      required: true,
    },
  },

  // emit the event to the parent component
  computed: {
    // check if the movie is in the favorite movies
    isFavorite() {
      return this.favMovies.some((fav) => fav.id === this.movie.id);
    },

    // get the date from the movie release date
    getDate() {
      const date = new Date(this.movie.release_date);
      const year = date.getFullYear();
      const month = date.getMonth() + 1;
      const day = date.getDate();
      return `${year}/${month}/${day}`;
    },
  },

  methods: {
    // emit the event to the parent component to add the movie to the favorite movies
    addFavoriteMovie() {
      this.$emit("add-favorite-movie", this.movie);
    },

    // emit the event to the parent component to remove the movie from the favorite movies
    removeFavoriteMovie() {
      this.$emit("remove-favorite-movie", this.movie);
    },
  },
};
</script>

<template lang="">
  <!--  main card  -->
  <div class="card">
    <!-- background image -->
    <img :src="movie.poster" class="card-img-top" alt="..." />

    <!-- overlay to add darkness for more visiblity of content -->
    <div class="overlay"></div>

    <!-- favorite icon -->
    <span class="fav" v-if="isFavorite" @click="removeFavoriteMovie">⭐️</span>

    <!-- not favorite icon -->
    <span class="fav" v-else @click="addFavoriteMovie">☆</span>

    <!-- card content -->
    <div class="card-body">
      <!-- title of the movie -->
      <h3 class="card-title">{{ movie.title }}</h3>

      <!-- genres of the movie  -->
      <div class="genres">
        <span class="tag" v-for="(genre, index) in movie.genres" :key="index">{{
          genre
        }}</span>
      </div>

      <!-- other information of movie  -->
      <div class="date">Release On: {{ getDate }}</div>
      <p class="card-text">{{ movie.overview }}</p>
    </div>
  </div>
</template>

<style lang="css" scoped>
.card {
  position: relative;
  --width: 29rem;
  --height: 35rem;
  width: var(--width);
  height: var(--height);
  border-radius: 10px;
  overflow: hidden;
}

@media screen and (max-width: 1080px) {
  .card {
    --width: 40rem;
    --height: 30rem;
  }
}

@media screen and (max-width: 720px) {
  .card {
    --width: 25rem;
    --height: 30rem;
  }
}

@media screen and (max-width: 480px) {
  .card {
    --width: 20rem;
    --height: 25rem;
  }
}

.fav {
  position: absolute;
  top: 1rem;
  right: 1rem;
  font-size: 2rem;
  color: #fff;
  z-index: 100;
  cursor: pointer;
}

.overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1;
  background: rgb(9, 9, 9);
  background: -moz-linear-gradient(
    55deg,
    rgba(9, 9, 9, 0.6895351890756303) 0%,
    rgba(255, 255, 255, 0) 100%
  );
  background: -webkit-linear-gradient(
    55deg,
    rgba(9, 9, 9, 0.6895351890756303) 0%,
    rgba(255, 255, 255, 0) 100%
  );
  background: linear-gradient(
    55deg,
    rgba(9, 9, 9, 0.6895351890756303) 0%,
    rgba(255, 255, 255, 0) 100%
  );
  filter: progid:DXImageTransform.Microsoft.gradient(startColorstr="#090909",endColorstr="#ffffff",GradientType=1);
}

.card-img-top {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: left top;
  transition: all 0.5s ease;
}

.card:hover .card-img-top {
  transform: scale(1.2);
}

.card-body {
  position: absolute;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  align-items: flex-start;
  bottom: 0;
  left: 0;
  text-align: left;
  color: #fff;
  z-index: 10;
  -ms-overflow-style: none; /* IE and Edge */
  scrollbar-width: none; /* Firefox */
}
.card-body::-webkit-scrollbar {
  display: none;
}

.card-title {
  font-size: 2.5rem;
  font-weight: 700;
  transition: all 0.5s ease;
}

.date {
  font-size: 1rem;
  font-weight: 500;
  margin-bottom: 0.5rem;
}

.card:hover .date {
  height: 0;
  overflow: hidden;
}

.genres {
  display: flex;
  flex-wrap: wrap;
  margin-bottom: 1rem;
  transition: all 0.5s ease;
}

.tag {
  margin-right: 0.5rem;
  margin-top: 0.5rem;
  padding: 0.2rem 0.5rem;
  border-radius: 100px;
  background-color: transparent;
  color: #fff;
  font-size: 0.8rem;
  font-weight: 500;
  text-transform: capitalize;
  border: 1px solid #fff;
}

.tag:hover {
  background-color: #fff;
  color: #000;
  cursor: default;
}

.card-text {
  font-size: 1.3rem;
  font-weight: 400;
  height: 0;
  opacity: 0;
  transition: all 0.5s ease;
  padding-bottom: 2rem;
}

.card:hover .card-text {
  opacity: 1;
  height: calc(var(--height) - 14rem);
}

.card:hover .card-title {
  font-size: 2rem;
}

.card:hover .card-body {
  overflow-y: scroll;
}

@media screen and (max-width: 480px) {
  .card-title {
    font-size: 2rem;
  }
  .card-text {
    font-size: 1.2rem;
  }

  .card:hover .card-title {
    font-size: 1.5rem;
  }
  .card:hover .tag {
    background-color: #fff;
    color: #000;
    font-size: 0.7rem;
  }
}
</style>
