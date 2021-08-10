<template>

  <div id="CoinNowApp">
    <b-card
      bg-variant="Light"
      class="text-center"
      text-variant="dark"
      title="Coin Now"
    >
      <b-card-text> Simple app to see cryptocurrency prices live </b-card-text>
      <a>Follow us: </a>
      <a class="ml-1" href="https://alonemazin.online/">
        <b-avatar
          v-b-tooltip.hover.bottom
          title="Mazin"
          src="https://avatars.githubusercontent.com/u/10653747?s=400&u=1c0965db1f58472912b95fe496f3bb01be83cc00&v=4"
           target="_blank"
      /></a>
      <a class="ml-1" href="https://instagram.com/n5y">
        <b-avatar
          v-b-tooltip.hover.bottom
          title="Fahad"
          src="https://i.top4top.io/p_2048otvst1.jpg"
           target="_blank"
          
      /></a>
      <a class="ml-1" href="https://www.xahmed.com/">
        <b-avatar
          v-b-tooltip.hover.bottom
          title="Ahmed."
          src="https://avatars.githubusercontent.com/u/79005095?s=400&u=ae9211181f8ed20a9e2d2834d4d0b1a6ec6bd72b&v=4"
          target="_blank"
      /></a>
      <br>
      <b-button v-b-toggle.Cryptocurrency class="m-1 mt-3 mb-3">Cryptocurrency Filter ⚙️</b-button>
      <b-button v-b-toggle.Coins class="m-1 mt-3 mb-3">Coins Filter 💸</b-button>
      <b-collapse id="Cryptocurrency">
        <b-card title="Cryptocurrency Filter ⚙️" class="text-left">
          <b-form-input v-model="newCryptocurrency" v-on:keyup.enter="AddCryptocurrency(newCryptocurrency)" placeholder="Exmp: BTC, ETH ..."></b-form-input>
          <b-list-group class="mt-2">
            <b-list-group-item v-for="Cryptocurrency in CryptocurrencyList" :key="Cryptocurrency">
              {{Cryptocurrency.toUpperCase()}}
              <b-button @click="RemoveCryptocurrency(Cryptocurrency)" class="float-right" style="display: flex;align-items: center;height: 24px;" variant="link">Remove</b-button>
            </b-list-group-item>
          </b-list-group>
        </b-card>
      </b-collapse>
      <b-collapse id="Coins">
        <b-card title="Coins Filter 💸" class="text-left">
          <b-form-input v-model="newCoin" v-on:keyup.enter="AddCoin(newCoin)" placeholder="Exmp: EUR, USD, SAR ..."></b-form-input>
          <b-list-group class="mt-2">
            <b-list-group-item v-for="Coin in CoinsList" :key="Coin">
              {{Coin}}
              <b-button @click="RemoveCoin(Coin)" class="float-right" style="display: flex;align-items: center;height: 24px;" variant="link">Remove</b-button>
            </b-list-group-item>
          </b-list-group>
        </b-card>
      </b-collapse>
    </b-card>

    <b-card bg-variant="Light" class="text-left mt-2" text-variant="dark">
      <b-container class="text-center">
        <h5 class="text-left mb-3 mt-1">Live now 🔴 | Refresh every 1 second</h5>
        <b-row cols="3" style="justify-content: space-between;">
          <b-card
            v-for="(result, index) in results"
            :key="index"
            :title="index"
            class="mb-2"
            style="max-width: 200px; min-height: 200px"
          >
          <hr>
            <b-card-text v-for="Coin in CoinsList" :key="Coin">
              <div v-if="result[Coin] != null">
                {{Coin}}: {{ result[Coin] }} 
              </div>
               </b-card-text>
          </b-card>
        </b-row>
      </b-container>
    </b-card>

    <div class="mt-4" style="display: flex;justify-content: space-between;">
 
   <a href="https://google.com">Source code</a>


      
  

      <a :href="url">For Developers</a>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "app",
  data: () => ({
    results: [],
    CryptocurrencyList: ["BTC","ETH","BNB","ADA","XRP","DOGE","DOT","UNI"],
    CoinsList: ["EUR","USD","SAR"],
    newCoin: null,
    newCryptocurrency: null,
    url: null
  }),
//BTC,ETH,BNB,ADA,XRP,DOGE,DOT,UNI,BCH,LTC,LINK,USDT
  mounted() {
    this.upDate();
    this.timer = setInterval(this.upDate, 1000);
  },
  methods: {
    upDate: function () {
      this.url = `https://min-api.cryptocompare.com/data/pricemulti?fsyms=${this.CryptocurrencyList}&tsyms=${this.CoinsList}`
      axios
        .get(
          this.url
        )
        .then((response) => {
          this.results = response.data;
        });
    },
    AddCryptocurrency(cc){
      if (!this.CryptocurrencyList.includes(cc)){
        this.CryptocurrencyList.push(cc)
        this.upDate()
        this.newCryptocurrency = null
      }else{
        alert("The coin is already exist")
      }
    },
    RemoveCryptocurrency(cc){
      if (this.CryptocurrencyList.length !== 1){
        this.CryptocurrencyList = this.CryptocurrencyList.filter(function(e) { return e !== cc })
      }else{
        alert("The list can't be empty")
      }
    },
    AddCoin(c){
      if (!this.CoinsList.includes(c)){
        this.CoinsList.push(c)
        this.upDate()
        this.newCoin = null
      }else{
        alert("The coin is already exist")
      }
    },
    RemoveCoin(c){
      if (this.CoinsList.length !== 1){
        this.CoinsList = this.CoinsList.filter(function(e) { return e !== c })
      }else{
        alert("The list can't be empty")
      }
    }
  },
};
</script>


<style>
@import url("https://fonts.googleapis.com/css2?family=Cairo&display=swap");

body {
  font-family: Cairo;
}

#CoinNowApp {
  padding: 10px;
  margin: auto;
  max-width: 900px;
}
</style>
