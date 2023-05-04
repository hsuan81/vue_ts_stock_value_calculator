<!-- eslint-disable vue/multi-word-component-names -->
<template>
    <div class="result" v-if="stockRatios.length > 0">
        <h2 v-if="stockMeta.company">{{ stockMeta.company }}</h2>
        <!-- <p>Expensive price</p>
        <p>Intrinsic value ${{ DDMprice }}</p>
        <p>Cheap price</p> -->
        <!-- <p>{{ stockMeta[0]['date'] }}</p> -->
        <ul class="text">
            <!-- <li v-if="stockMeta">Company:  {{ stockMeta.company}}</li> -->
            <li v-if="stockRatios.length > 0">Data date: {{ stockRatios[0].date }}</li>
            <li v-if="stockRatios.length > 0">PE ratio: {{ stockRatios[0]['priceEarningsRatio'].toFixed(2) }}</li>
            <li v-if="prices.length > 0">Latest close price: {{ prices[0]['date'] }} {{ prices[0]['closePrice'] }}</li>
            <li v-if="prices.length > 0">Previous close price: {{ prices[1]['date'] }} {{ prices[1]['closePrice']  }}</li>

        </ul>
        <!-- <p v-if="stockMeta">Company:  {{ stockMeta.company}}</p>
        <p v-if="stockRatios">Data date: {{ stockRatios[0].date }}</p>
        <p v-if="stockRatios">PE ratio: {{ stockRatios[0]['priceEarningsRatio'].toFixed(2) }}</p>
        <p v-if="prices">Latest close price: {{ prices[0]['date'] }} {{ prices[0]['closePrice'] }}</p>
        <p v-if="prices">Previous close price: {{ prices[1]['date'] }} {{ prices[1]['closePrice']  }}</p> -->



    </div>
</template>

<script lang="ts">
import { defineComponent, PropType } from 'vue';
import axios from 'axios'; 

export interface Ratios {
    symbol: string,
    date: string,
    priceEarningsRatio: number,
}

export interface Meta {
    symbol: string,
    company: string,
    exchange: string,
    sector: string,
    industry: string,
}

export interface Price {
    // symbol: string,
    date: string,
    closePrice: string,
}

export default defineComponent({
    props: {
       ticker: { type: String, required: true },
    },
    data() {
        return {
            stockMeta: {} as Meta,
            stockRatios: [] as Ratios[],
            prices: [] as Price[],
            priceUrl: "",
            metaUrl: "",
            ratioUrl: "",
            industryPEUrl: "",
        }
    },
    methods: {
        getStockMeta() {
            // this.ticker = this.ticker?.toUpperCase()
            // console.log(this.ticker?.toUpperCase())
            /* the ticker doesn't need to be uppercase to be embeded into the url */
            this.metaUrl = 'https://financialmodelingprep.com/api/v3/profile/'+ this.ticker + '?apikey=64d87be0d4d4f512a1a72a98b3038d1c'
            axios.get(this.metaUrl).then((response) => {
                this.stockMeta.company = response.data[0].companyName
                this.stockMeta.exchange = response.data[0].exchangeShortName
                this.stockMeta.symbol = response.data[0].symbol
                this.stockMeta.sector = response.data[0].sector
                this.stockMeta.industry = response.data[0].industry
                // console.log(this.stockMeta)
            })
        },

        getStockRatios() {
            this.ratioUrl = 'https://financialmodelingprep.com/api/v3/ratios/' + this.ticker + '?limit=1&apikey=64d87be0d4d4f512a1a72a98b3038d1c'
            axios.get(this.ratioUrl).then((response) => {this.stockRatios = response.data})
        },

        getStockPrice() {
            this.priceUrl = 'https://financialmodelingprep.com/api/v3/historical-price-full/' + this.ticker + '?serietype=line&apikey=64d87be0d4d4f512a1a72a98b3038d1c'
            axios.get(this.priceUrl).then((response) => {this.prices = response.data.historical.slice(0,2).map((e: {date: string, close: number}):Price => {return {date: e.date, closePrice: e.close.toString()}})})
            // axios.get(this.priceUrl).then((response) => {console.log(response.data.historical.slice(0,2).map((e: {date: string, close: number}):Price => {return {date: e.date, closePrice: e.close.toString()}}))})
            // console.log(this.prices)
        }
    },
    mounted() {
        // this.DDMprice = Math.round(this.dividend / (this.roe - this.divdgrowthrate))
        // axios.get('https://financialmodelingprep.com/api/v3/ratios/AAPL?&apikey=64d87be0d4d4f512a1a72a98b3038d1c').then((response) => {
        //     this.stockMeta = response.data})
            // this.Ratios.push(response.data[0])
            // console.log(this.stockMeta[0])
        // this.getStockMeta()
        console.log(this.stockMeta)
        this.getStockMeta()
        this.getStockRatios()
        this.getStockPrice()
        console.log(this.stockRatios.length)
        console.log(this.stockMeta)
        // console.log(this.stockMeta[0]['date'])
        }
        // console.log(this.roe)
        
    },
)
</script>
<style>

.result {
    background-color: white;
    block-size: 200px;
    display: flex;
	flex-direction: row;
	flex-wrap: nowrap;
	justify-content: center;
	align-items: stretch;
	align-content: stretch;
    margin: auto;
    border-radius:20px;
    text-align: justify;
    margin-block-start: 20px;
    width: 60%;
}
h2 {
  color: #4b8f29;
  font-size: 2em;
  text-align: center;
  /* text-decoration: underline solid; */
}

li {
    list-style-type: none;
    font-family:Arial;
    font-size:1em;
    margin: 20px auto;
}

li:hover {
    background-color: #4b8f29;
    color: white;
    border-radius: 2px;
}
</style>