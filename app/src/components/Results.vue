<!-- eslint-disable vue/multi-word-component-names -->
<template>
    <div v-if="stockRatios.length > 0">
        <h1>Results</h1>
        <!-- <p>Expensive price</p>
        <p>Intrinsic value ${{ DDMprice }}</p>
        <p>Cheap price</p> -->
        <!-- <p>{{ stockMeta[0]['date'] }}</p> -->
        <p v-if="stockMeta">Company:  {{ stockMeta}}</p>
        <p>Data date: {{ stockRatios[0].date }}</p>
        <p>PE ratio: {{ stockRatios[0]['priceEarningsRatio'].toFixed(2) }}</p>
        <p v-if="industryPE">Industry PE Ratio: {{ industryPE }}</p>

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

export interface IndustryPERatio {
    date: string,
    industry : string,
    exchange : string,
    pe : string
}

export default defineComponent({
    props: {
       ticker: { type: String, required: true },
    },
    data() {
        return {
            stockMeta: {} as Meta,
            industryPE: {} as IndustryPERatio, 
            stockRatios: [] as Ratios[],
            peers: [],
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
                console.log(this.stockMeta)
            })
        },

        getStockRatios() {
            this.ratioUrl = 'https://financialmodelingprep.com/api/v3/ratios/' + this.ticker + '?limit=1&apikey=64d87be0d4d4f512a1a72a98b3038d1c'
            axios.get(this.ratioUrl).then((response) => {this.stockRatios = response.data})
        },
        getIndustryPER() {
            this.industryPEUrl = 'https://financialmodelingprep.com/api/v4/industry_price_earning_ratio?date='+ this.stockRatios[0].date+'&exchange='+ this.stockMeta.exchange +'&apikey=64d87be0d4d4f512a1a72a98b3038d1c'
            axios.get(this.industryPEUrl).then((response) => {
                this.industryPE = response.data.filter((e: IndustryPERatio) => { e.industry == this.stockMeta.industry})
                console.log(this.industryPE)
            })

        },
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
        console.log(this.stockRatios.length)
        console.log(this.stockMeta)
        // console.log(this.stockMeta[0]['date'])
        }
        // console.log(this.roe)
        
    },
)
</script>
<style></style>