<template>
    <Header/>
        <div class="main-container">
        <Exchange-component :currencies=this.currencies   />
        <HistoryComponent :history=this.history />
        </div>
    <Footer/>
</template>

<script>
import Header from './components/Header.vue'
import ExchangeComponent from './components/ExchangeComponent.vue'
import Footer from './components/Footer.vue'
import Axios from 'axios'
import HistoryComponent from './components/HistoryComponent.vue'

export default {
  name: 'App',
  components: {
    Header,
    ExchangeComponent,
    HistoryComponent,
    Footer
  },

  data(){ 
    return{
    currencies: {},
    history:[]
      }},

  created()
  {
    this.loadFiatCurrencies()
  },


  methods:
  {

    /**
     * Method that takes no parameters and loads fiat currencies into the currencies property. 
     */
    loadFiatCurrencies()
    {
   Axios.get("https://api.exchangeratesapi.io/latest?base=USD")
    .then((response)=>{
     let newCurrencies = {}
          for (const [key, value] of Object.entries(response.data.rates)) {
          newCurrencies[key] = value;
          }
      newCurrencies[response.data.base] = 1;
      this.currencies = newCurrencies;
      })

    },


    /**
     * Method that takes no parameters and adds crypto currencies into the currencies property.
     */
    loadCryptoCurrencies() 
    {  
    Axios.get("https://api.binance.com/api/v3/ticker/price")
    .then((response) =>{
     let keys = []
     for (const [key,value] of Object.entries(response.data))
     {
        keys.push(key)
        this.currencies[value.symbol] = (1/value.price)
     }    
    })


    },

  }

}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.main-container{
    position: relative;
    display: block;
    height: auto;
    width: 100%;
    color: white;
    text-shadow: 1px 1px 3px rgba(0, 0, 0, 0.466);

}

</style>

