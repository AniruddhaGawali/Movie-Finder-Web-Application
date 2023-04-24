<script>
export default {
  name: "SearchBar",

  data() {
    return {
      search: "",
      movies: [],
    };
  },

  // reactive data
  props: {
    changeSearch: {
      type: Function,
      required: true,
    },
    allGenres: {
      type: Array,
      required: true,
    },
    filter: {
      type: Array,
      required: true,
    },
    isFavOn: {
      type: Boolean,
      required: true,
    },
  },

  // emit the event to the parent component
  emits: ["show-fav", "change-filter", "remove-all"],

  // methods
  methods: {
    isGenreActive(genre) {
      return this.filter.includes(genre);
    },
  },

  // watch the search reactive data and emit the event to the parent component
  watch: {
    search(newSearch) {
      this.changeSearch(newSearch);
    },
  },
};
</script>

<template>
  <div>
    <!-- main searchbar -->
    <div class="box">
      <span class="seach-icon">🔍</span>
      <input
        type="text"
        class="search-input"
        placeholder="Search Movie"
        v-model="search"
      />
      <!-- button for favorite movies -->
      <b-button
        :variant="isFavOn ? 'outline-primary' : 'outline-warning'"
        class="btn"
        @click="$emit('show-fav')"
      >
        {{ isFavOn ? "🎬 All Movies" : "🌟 Favorite Movies" }}
      </b-button>
    </div>

    <!-- genres filter buttons -->
    <div class="genres">
      <span
        v-for="(genre, index) in allGenres"
        class="tag"
        :class="{ active: isGenreActive(genre) }"
        :key="index"
        @click="$emit('change-filter', genre)"
      >
        {{ genre }}
      </span>

      <!-- remove all genres filter button -->
      <span
        class="tag"
        style="color: red; border-color: red"
        @click="$emit('remove-all')"
        >X</span
      >
    </div>
  </div>
</template>

<style scoped>
.box {
  position: relative;
  display: flex;
  flex-wrap: wrap;
  width: 100%;
  justify-content: center;
  align-items: center;
  margin: 5rem 0rem;
  margin-bottom: 1rem;
}

.genres {
  display: flex;
  flex-wrap: wrap;
  margin-bottom: 1rem;
  transition: all 0.5s ease;
  align-items: center;
  justify-content: center;
  margin: 3rem 2rem;
  margin-top: 0;
}

.tag {
  margin-right: 0.5rem;
  margin-top: 0.5rem;
  padding: 0.2rem 0.5rem;
  border-radius: 100px;
  background-color: transparent;
  color: #000;
  font-size: 0.8rem;
  font-weight: 500;
  text-transform: capitalize;
  cursor: pointer;
  border: 1px solid #000;
}

.tag.active,
.tag:hover.active {
  background-color: #000;
  color: #fff;
}

.tag:hover {
  background-color: #ccc;
  color: #000;
}

.btn {
  height: 50px;
  font-size: 1.3rem;
  margin-left: 1rem;
  font-weight: bold;
}

.search-input {
  width: 50%;
  height: 50px;
  border: 1px solid #ccc;
  border-radius: 0 5px 5px 0;
  padding: 0px 10px;
  font-size: 1.2rem;
  border-left: 0;
}

.search-input:focus {
  outline: none;
}

.seach-icon {
  font-size: 1.5rem;
  color: #ccc;
  height: 50px;
  padding: 6px 10px;
  border: 1px solid #ccc;
  border-radius: 5px 0 0 5px;
  border-right: 0;
}

@media screen and (min-width: 900px) {
  .search-input {
    width: 70%;
  }
}
</style>
