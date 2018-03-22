<template>
  <div>
    <div>
      <button
        v-for="(_, i) in 17"
        :key="i"
        @click="needles.push(i)">
        {{ i }}
      </button>
    </div>

    <select v-model="selectedSearchMode">
      <option
        v-for="searchMode in searchModes"
        :key="searchMode">
        {{ searchMode }}
      </option>
    </select>
    <p>needles: {{ needles }}</p>
    <button @click="fetchSeed()">
      send
    </button>
    <ul id="results">
      <li
        v-for="result in results"
        :key="result.seed">
        {{ result.seed }}
      </li>
    </ul>
  </div>
</template>
<script>
import axios from 'axios';

export default {
  data() {
    return {
      needles: [],
      searchModes: [
        'usm/sfmt/seed', 'sm/sfmt/seed',
        'usm/sfmt/seed/id', 'sm/sfmt/seed/id'],
      selectedSearchMode: 'usm/sfmt/seed',
      results: [],
    };
  },
  methods: {
    async fetchSeed() {
      const url = `https://rng-api.odanado.com/${this.selectedSearchMode}`;
      const response = await axios.get(url, {
        params: { needle: this.needles.join() },
      });
      this.results = response.data.results;
    },
  },
};
</script>

<style scoped>
ul {
  list-style-type: none;
}
</style>
