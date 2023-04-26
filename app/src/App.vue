<template>
  <h1>Stock Value Calculator</h1>
  <input type="text" v-model="ticker" placeholder="stock ticker">
  <button @click="start" :disabled="isCalculating">Calculate</button>
  <Results v-if="isCalculating" :ticker="ticker"/>
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
      ticker: ""  ,
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
