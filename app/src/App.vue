<template>
  <h1>Stock Value Calculator</h1>

  <div class="stockInput">
    <input class="input" type="text" @change="start" v-model="ticker" placeholder="us stock ticker">
    <button class="myButton" @click="start" :disabled="isCalculating">Calculate</button>
  </div>
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
  /* background-color: #bef4a5;
  background-size: cover; */
}

body {
  background-color: #c6f3b1;
  /* background-size: cover; */
}

h1 {
  color: #4b8f29;
  font-size: 2.5em;
}

.stockInput input, .stockInput button {
  height: 24px;
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
  border:1px solid #4b8f29;
  margin:0;
}
.stockInput .input {
  color: #888484;
}

.stockInput .myButton {
  /* box-shadow: 0px 0px 14px -7px #3e7327; */
	background:linear-gradient(to bottom, #77b55a 5%, #72b352 100%);
	background-color:#77b55a;
	/* border:1px solid #4b8f29; */
  color:#ffffff;
  font-family:Arial;
	font-size:13px;
	font-weight:bold;
}

        
</style>
