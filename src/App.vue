<template>
  <div class="container-fluid d-flex justify-content-between box-shadow pt-2"
       :class="isDark ? 'bg-light text-dark' : 'bg-dark text-light'">
    <h4 class="mt-1">Handy Home Page
      <DarkModeToggle :is-dark="isDark" @darkModeToggle="toggleDarkMode"/>
    </h4>
    <small class="fa-pull-right mb-2">
      <input class="form-control border-end-0 border small" type="search" placeholder="Search" id="searchInput"
             v-model="searchQuery">
    </small>
  </div>
  <DataReader :json-data="filteredData" :search-query="searchQuery" :is-dark="isDark" :cols="cols"/>
</template>

<script>
import DataReader from "@/components/DataReader.vue";
import DarkModeToggle from '@/components/DarkModeToggle.vue';
import {jsonData} from '@/data.js';

export default {
  name: 'App',
  components: {
    DataReader,
    DarkModeToggle
  },
  data() {
    return {
      jsonData: jsonData,
      searchQuery: '',
      isDark: process.env.VUE_APP_IS_DARK === 'true',
      cols: parseInt(process.env.VUE_APP_COLS || 3)
    };
  },
  methods: {
    toggleDarkMode(event) {
      this.isDark = event;
    }
  },
  computed: {
    filteredData() {
      if (!this.searchQuery) {
        return this.jsonData;
      } else {
        let searchQuery = this.searchQuery.toLowerCase();
        return this.jsonData.filter(function (record) {
          if (record.title.toLowerCase().includes(searchQuery)) {
            return true;
          } else {
            for (const url of record.urls) {
              if (url.title.toLowerCase().includes(searchQuery)) {
                return true;
              }
            }

            return false;
          }
        });
      }
    }
  }
}
</script>

<style>
.dark-mode {
  background-color: #1a1a1a !important;
  color: #ffffff;
}
</style>
