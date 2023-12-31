<!-- DarkModeToggle.vue -->

<template>
  <button @click="toggleDarkMode" class="btn btn-primary btn-sm"
          :class="isDarkMode ? 'text-light bg-dark' : 'text-dark bg-light'">
    {{ isDarkMode ? 'Light Mode' : 'Dark Mode' }}
  </button>
</template>

<script>
export default {
  props: {
    isDark: {
      type: Boolean,
      required: false,
      default: false
    }
  },
  data() {
    return {
      isDarkMode: this.isDark,
    };
  },
  methods: {
    toggleDarkMode() {
      this.isDarkMode = !this.isDarkMode;
      this.$emit('darkModeToggle', this.isDarkMode);
      document.querySelectorAll('.darkable').forEach(el => el.classList.toggle('dark-mode', this.isDarkMode));
      document.documentElement.classList.toggle('dark-mode', this.isDarkMode);
    }
  },
};
</script>

<style scoped>
/* Add scoped styles if needed */
</style>
