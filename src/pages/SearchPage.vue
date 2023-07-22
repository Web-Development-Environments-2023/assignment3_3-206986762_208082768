<template>
  <div class="container">
    <div id="background"></div>
    <div class="d-flex align-items-center mb-3">
      <div class="flex-grow-1 mr-2">
        <b-form-input
          v-model="text"
          placeholder="Search"
          @focus="openRecentSearches"
          class="w-100"
        ></b-form-input>
      </div>
      <div>
        <button @click="search" class="btn btn-primary ml-2">Search</button>
      </div>
    </div>

    <b-form-select
      v-model="selectedCuisine"
      :options="cuisineOptions"
      placeholder="Select Cuisine"
      class="mb-3"
    ></b-form-select>

    <b-form-select
      v-model="selectedDiet"
      :options="dietOptions"
      placeholder="Select Diet"
      class="mb-3"
    ></b-form-select>

    <b-form-select
      v-model="selectedIntolerance"
      :options="intoleranceOptions"
      placeholder="Select Intolerance"
      class="mb-3"
    ></b-form-select>
    <b-form-select
      v-model="resultsPerPage"
      :options="resultsPerPageOptions"
      placeholder="Results Per Page"
      class="mb-3"
    ></b-form-select>

    <div
      v-if="showRecentSearches && recentSearches.length > 0"
      class="mt-3 position-relative"
    >
      <small
        class="text-danger font-weight-bold"
        style="text-shadow: 2px 2px 4px rgba(248, 244, 5, 0.89)"
      >
        Recent Searches
      </small>

      <div class="d-flex justify-content-between align-items-center mt-3">
        <button
          class="btn btn-danger btn-sm close-button"
          @click="closeRecentSearches"
        >
          <span aria-hidden="true">&times;</span>
        </button>
        <button @click="clearRecentSearches" class="btn btn-danger btn-sm">
          Clear Recent Searches
        </button>
      </div>

      <ul class="list-group mt-3" ref="recentSearchesList">
        <li
          v-for="search in recentSearches"
          :key="search"
          class="list-group-item recent-search-item"
          @click="selectRecentSearch(search)"
        >
          {{ search }}
        </li>
      </ul>
    </div>


    <table width="100%">
      <br><br>
      <tr>
        <td width="100%">
          <RecipePreviewList ref="previewList"  title="Search Result"></RecipePreviewList>
        </td>
      </tr>
    </table>

  </div>
</template>

<!-- #######################################################################################################
############################################ scripts ################################################## -->
<script>
import {
  cuisineOptions,
  dietOptions,
  intoleranceOptions,
  resultsPerPageOptions,
} from "../assets/searchOptions.js";
export default {
  name: "SearchPage",
  components: {
    RecipePreviewList,
  },
  
  data() {
    return {
      text: "",
      recentSearches: [],
      showRecentSearches: false,
      selectedCuisine: null,
      cuisineOptions, // Use the imported cuisineOptions
      selectedDiet: null,
      dietOptions, // Use the imported dietOptions
      selectedIntolerance: null,
      intoleranceOptions, // Use the imported intoleranceOptions
      resultsPerPage: null,
      resultsPerPageOptions, // Use the imported resultsPerPageOptions
    };
  },
  mounted() {
    this.recentSearches = this.getRecentSearches();
    this.resultsPerPage = 5;
  },

  methods: {
    saveSearch(query) {
      const searches = localStorage.getItem("recentSearches");
      const existingSearches = searches ? JSON.parse(searches) : [];
      existingSearches.unshift(query);
      const limitedSearches = existingSearches.slice(0, 10);
      localStorage.setItem("recentSearches", JSON.stringify(limitedSearches));
      this.recentSearches = limitedSearches;
    },
    getRecentSearches() {
      const searches = localStorage.getItem("recentSearches");
      return searches ? JSON.parse(searches) : [];
    },
    async search() {
      console.log("Search initiated with query:", this.text);
      this.closeRecentSearches();
      this.saveSearch(this.text);
      this.$refs.previewList.searchQuery(this.text, number, cusineType, dietType, intoleranceType, sortBy)
    },

    openRecentSearches(event) {
      event.stopPropagation();
      if (this.recentSearches.length > 0) {
        this.showRecentSearches = true;
      }
    },
    closeRecentSearches() {
      this.showRecentSearches = false;
    },
    clearRecentSearches() {
      localStorage.removeItem("recentSearches");
      this.recentSearches = [];
    },
    selectRecentSearch(search) {
      this.text = search; // Update the search bar with the selected recent search
    },
  },
};
</script>

<!-- #######################################################################################################
################################################# css ################################################## -->

<style lang="scss">
.list-group {
  position: absolute;
  width: calc(100% - 1rem);
  z-index: 1;
  max-width: 100%;
}

.container {
  margin-top: 80px;
  max-width: 100%;
}

#background {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-repeat: no-repeat;
  margin: 0;
  background-image: url("../assets/background.jpg");
  background-size: cover;
  background-position: center;
  z-index: -1;
  opacity: 0.9;
}

.close-button {
  margin-right: 0.5rem;
}

.recent-search-item {
  cursor: pointer;
}
</style>
