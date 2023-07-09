
<template>
    <div>
        <h1>Forex Market Sentiment</h1>
        <div>
            <label for="currency">Select Currency:</label>
            <select id="currency" v-model="selectedCurrency" @change="fetchMarketSentiment">
                <option v-for="currency in pairNames" :value="currency">{{ currency }}</option>
            </select>
        </div>
        <div>
            <h2>Market Sentiment for {{ selectedCurrency }}</h2>
            <ul>
                <li v-for="pair in filteredPairs" :key="pair">{{ pair }}</li>
            </ul>
        </div>
        <div>
            <h2>Last 5 Selected Currencies:</h2>
            <ul>
                <li v-for="currency in favoriteCurrencies" :key="currency.symbol">{{ currency }}</li>
            </ul>
        </div>
    </div>
</template>
  
<script>
import axios from 'axios';
import Utils from '../utils/config.json';


export default {
    data() {
        return {
            selectedCurrency: '',
            marketSentiment: [],
            pairNames: [],
            favoriteCurrencies: [],
            refreshInterval: null,
            userSession: ''
        };
    },
    computed: {
        filteredPairs() {
            //console.log('market ', this.marketSentiment.filter(pair => pair.name.includes(this.selectedCurrency)));
            return this.marketSentiment.filter(pair => pair.name.includes(this.selectedCurrency));
        },

    },
    methods: {

        loginAccount() {

            const credentials = {
               username : Utils.username,
               password : Utils.password
            };

            axios
                .get(`https://www.myfxbook.com/api/login.json?email=${credentials.username}&password=${credentials.password}`)
                .then(response => {
                    // console.log('login ',response);
                    this.userSession = response.data.session;

                    this.fetchMarketSentiment();
                })
                .catch(error => {
                    console.error('Login failed:', error);
                });
        },
        fetchMarketSentiment() {
            const apiKey = this.userSession; // Replace with your Myfxbook API key
            console.log(apiKey)
            if (!this.userSession) {
                return; // Exit if API token is not available
            }
            const url2 = `https://www.myfxbook.com/api/get-community-outlook.json?session=${apiKey}`
            axios
                .get(url2)
                .then(response => {
                    this.marketSentiment = response.data.symbols;
                    if (this.pairNames.length == 0) { this.createPairNamesList(); }

                })
                .catch(error => {
                    console.error('Error fetching market sentiment:', error);
                });
        },
        updateFavoriteCurrencies() {
            const favorites = [...this.favoriteCurrencies, this.getSelectedCurrencyInfo()];
            this.favoriteCurrencies = favorites.slice(-5); // Keep the last 5 selected currencies

            localStorage.setItem('favoriteCurrencies', JSON.stringify(this.favoriteCurrencies));
        },
        getSelectedCurrencyInfo() {
            //return this.marketSentiment.find(currency => currency.name === this.selectedCurrency);
            return this.selectedCurrency;
        },
        createPairNamesList() {
            this.pairNames = this.marketSentiment.map(pair => pair.name);
             console.log(this.pairNames);
        },
    },
    created() {
        this.loginAccount();

        this.selectedCurrency = "EURUSD"; //this.currencies[0].symbol; // Set initial currency selection
        let getLocalstorage = JSON.parse(localStorage.getItem('favoriteCurrencies'));
        this.favoriteCurrencies = getLocalstorage
        this.fetchMarketSentiment(); // Fetch market sentiment initially

        this.refreshInterval = setInterval(() => {
            this.fetchMarketSentiment(); // Refresh market sentiment every 60 minutes
        }, 60 * 60 * 1000);
    },
    beforeDestroy() {
        clearInterval(this.refreshInterval); // Clear the refresh interval before the component is destroyed
    },
    watch: {
        selectedCurrency() {
            this.updateFavoriteCurrencies();
        },
    },
};
</script>
  