<!-- eslint-disable vue/multi-word-component-names -->
<template>
    <h1>Results</h1>
    <div>
        <p>Expensive price</p>
        <p>Intrinsic value ${{ DDMprice }}</p>
        <p>Cheap price</p>
        <p>{{ PEratio[0] }}</p>

    </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import axios from 'axios'; 


export default defineComponent({
    props: {
       ticker: String,
    },
    data() {
        return {
            dividend: 2.54,
            roe: 0.36,
            divdgrowthrate:0.1,
            DDMprice: 0,
            PEratio: [] as JSON[],
        }
    },
    mounted() {
        // this.DDMprice = Math.round(this.dividend / (this.roe - this.divdgrowthrate))
        axios.get('https://financialmodelingprep.com/api/v3/ratios/AAPL?limit=1&apikey=64d87be0d4d4f512a1a72a98b3038d1c').then(res => this.PEratio = res.data)
        console.log(typeof this.PEratio[0])
    },
})
</script>
<style></style>