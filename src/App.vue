<template>
  <div id="app">
    <header><h1>Planetary Grid</h1></header>
    <section>
      <form>
        <label for="searchByName">Search planets by name: </label>
        <input
          type="text"
          name="searchByName"
          id="searchByName"
          v-model="searchTerm"
        />
      </form>
    </section>
    <section>
      <h3>Total results: {{ totalResults }}</h3>
      <planet-table :planetsArray="planetsArray" />
    </section>
    <nav>
      <button @click="updatePage(1)" class="firstButton">First</button>
      <button
        v-if="computedPrevPage"
        @click="updatePage(computedPrevPage)"
        class="prevButton"
      >
        Previous
      </button>
      <button
        v-if="computedNextPage"
        @click="updatePage(computedNextPage)"
        class="nextButton"
      >
        Next
      </button>
      <button @click="updatePage(computedLastPage)" class="lastButton">
        Last
      </button>
    </nav>
  </div>
</template>
<script>
import axios from "axios";
import planetTable from "./components/PlanetTable.vue";
export default {
  name: "quPlanetGrid",
  components: {
    planetTable,
  },
  data() {
    return {
      totalResults: 0,
      resultsPerPage: 10,
      currentPage: 1,
      planetsArray: [],
      firstPage: 1,
      searchTerm: "",
    };
  },
  computed: {
    computedLastPage() {
      return Math.ceil(this.totalResults / this.resultsPerPage);
    },
    computedNextPage() {
      if (this.currentPage < this.computedLastPage) {
        return this.currentPage + 1;
      } else {
        return null;
      }
    },
    computedPrevPage() {
      if (this.currentPage > this.firstPage) {
        return this.currentPage - 1;
      } else {
        return null;
      }
    },
  },
  mounted() {
    this.requestPlanets(this.currentPage, this.searchTerm);
  },
  methods: {
    updatePage(page) {
      this.currentPage = page;
      this.requestPlanets(this.currentPage, this.searchTerm);
    },
    requestPlanets(page, searchTerm) {
      let paramString = "";
      const that = this;
      if (!Number.isNaN(page)) {
        paramString += `?page=${page}`;
      }
      if (searchTerm !== "") {
        paramString += `&search=${searchTerm}`;
      }
      axios
        .get(`https://swapi.dev/api/planets/${paramString}`)
        .then((result) => {
          that.totalResults = result.data.count;
          that.planetsArray = result.data.results;
        });
    },
  },
  watch: {
    searchTerm() {
      this.currentPage = 1;
      this.requestPlanets(this.currentPage, this.searchTerm);
    },
  },
};
</script>
<style scoped lang="scss">
table {
  thead {
    font-weight: bold;
    td {
      padding-right: 0.5rem;
    }
  }
}
nav {
  margin-top: 1rem;
  margin-bottom: 1rem;
  display: flex;
  justify-content: flex-start;
  gap: 1rem;
}
</style>
