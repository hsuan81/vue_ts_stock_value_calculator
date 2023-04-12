<template>
  <h1>Stock Value Calculator</h1>
  <button @click="start">Calculate</button>
  <Results v-if="isCalculating"/>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import Results  from "./components/Results.vue";


export default defineComponent({
  name: 'App',
  components: {
    Results,
  },
  data() {
    return {
      isCalculating: false,
    }
  },
  methods: {
    start() {
      this.isCalculating = true
    }
  }
});
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
