<template>
    <div id="mainDiv">
    <div class="tableCoins">
        <h2>Top 10 cryptocurrencies by market cap</h2>
        <table>
            <tr id="title">
                <td>Name</td>
                <td>Value USD</td>
                <td>Market cap</td>
                <td>High 24h</td>
                <td>Price change 1h</td>
                <td>Price change 24h</td>
                <td>Price change 7d</td>
            </tr>
            <tr v-for="coin in coins">
                <td @click="popup(coin)" id="cryptoName">{{ coin.name }}</td>
                <td>{{ coin.current_price }}</td>
                <td>{{ coin.market_cap }}</td>
                <td>{{ coin.high_24h }}</td>
                <td>{{ coin.price_change_percentage_1h_in_currency.toFixed(4) }}%</td>
                <td>{{ coin.price_change_percentage_24h_in_currency.toFixed(4) }}%</td>
                <td>{{ coin.price_change_percentage_7d_in_currency.toFixed(4) }}%</td>
            </tr> 
        </table>
    </div>
    <div class="popup" v-if="result && !error">
        <h3>{{ result.name }}</h3>
        <p>Price EUR: {{ result.market_data.current_price.eur }} </p>
        <p>Price CAD: {{ result.market_data.current_price.cad }} </p>
    </div>
    <div class="popup" v-if="!result && !error">
        <p>Click on crypto currency.</p>
    </div>
    <div class="popup" v-if="!result && error">
        <p>Error: {{ error }}</p>
    </div>
    
</div>
</template>

<script setup>
import {ref} from 'vue'
const emit = defineEmits(['finish'])
let coins = ref([])
let result = ref(null)
let error = ref(null)

fetch('https://api.coingecko.com/api/v3/coins/markets?vs_currency=eur&order=market_cap_desc&per_page=10&page=1&sparkline=false&price_change_percentage=1h%2C24h%2C7d')
    .then(resp => resp.json())
    .then(result => {
        coins.value = result
        setTimeout(function() {emit('finish')}, 1000)
})

function popup(coin) {
    if(coin) {
        fetch(`https://api.coingecko.com/api/v3/coins/${coin.id}`)
            .then(resp => resp.json())
            .then(coin => coin.error ?  handleError('Greska u zvanju API-ja.') : this.result = coin)
            .catch(error => this.error = error.message)
    }
}

function handleError(msg) {
    this.result = null
    throw new Error(msg)
}


</script>



<style scoped lang="scss">
#mainDiv {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    gap: 15px;
    margin-top: 20px;
    margin-left: 20px;
    text-align: center;
}

table {
    display: inline-block;
    border: 1px solid black;
    border-collapse: collapse;
    td {
        border: 2px solid black;
        padding: 10px;
        background-color: rgba(77, 82, 80, 0.521);
        transition: 0.2s;
    }
    td:hover {
        background-color: rgba(107, 112, 111, 0.438);
    }
    #title td {
        background-color: rgb(94, 105, 100);
    }
    #cryptoName {
        background-color: rgb(47, 179, 93);
        transition: 0.2s;
    }
    
    #cryptoName:hover {
        background-color: rgb(53, 134, 82);
        cursor: pointer;
    }
}

</style>