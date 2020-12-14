<template>
    <Header/>
    <Exchange-component :currencies=this.currencies   />
</template>

<script>
import Header from './components/Header.vue'
import ExchangeComponent from './components/ExchangeComponent.vue'
import Axios from 'axios'

export default {
  name: 'App',
  components: {
    Header,
    ExchangeComponent
  },

  data(){ return{
          ratesToEuro: [],
          currencies: {}
        }

    },

  created()
  {
    Axios.get("https://api.exchangeratesapi.io/latest")
    .then((response)=>{

     var newCurrencies = {}

      for (const [key, value] of Object.entries(response.data.rates)) {
      newCurrencies[key] = value;
      }
      newCurrencies[response.data.base] = 1;
      this.currencies = newCurrencies;

})
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
