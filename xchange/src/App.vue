<template>
    <Header/>
    <Exchange-component :currencies=this.currencies   />
    <Footer/>
</template>

<script>
import Header from './components/Header.vue'
import ExchangeComponent from './components/ExchangeComponent.vue'
import Footer from './components/Footer.vue'
import Axios from 'axios'

export default {
  name: 'App',
  components: {
    Header,
    ExchangeComponent,
    Footer
  },

  data(){ 
    return{
    currencies: {},
      }

    },

  created()
  {
    this.loadFiatCurrencies()
  },
  methods:
  {
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
      console.log("I was called")  
      })

    },

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

    unloadCryptoCurrencies(){

    }
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
</style>
