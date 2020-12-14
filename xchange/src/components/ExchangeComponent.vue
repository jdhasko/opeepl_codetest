<template>
    <div class="main-container">
        <div class="exchange-container">
                    <h2>Fiat and crypto currency exchange</h2>
                    <div class="flex-container">
                                <div class="input-container">
                                    <input v-model="fromValue" @change="onChangeInput"/> 
                                       <select v-model="fromCurrency" @change="onChangeInput" >
                                            <option v-for="c in Object.keys(currencies)" :key="c" :value="c">
                                                {{ c }}
                                            </option>
                                      </select>
                                </div>
                                <p id="equal-sign"> = </p>
                                <div class="input-container">
                                      <input v-model="toValue"/> 
                                      <select v-model="toCurrency" @change="onChangeInput">
                                            <option v-for="c in Object.keys(currencies)" :key="c" :value="c">
                                           {{ c }}
                                            </option>
                                      </select>
                                </div>
                    </div>
        </div>
    </div>
</template>

<script>
export default {
    name:'ExchangeComponent',
    props:
    {
        currencies: {}
    },
    methods:{
        onChangeInput()
        {
            console.log("The script has run!")
            let input = this.toNumberFormat(this.fromValue)
            console.log(input)
            let rateFrom = this.currencies[this.fromCurrency]
            let rateTo = this.currencies[this.toCurrency]   
            let result = this.calculate(input,rateFrom,rateTo)
            this.toValue = this.toCurrencyFormat(result)
            this.fromValue = this.toCurrencyFormat(input)

        },
        
        calculate(amount, rateFrom, rateTo)
        {
            let result = (amount / rateFrom)*rateTo
            return result.toFixed(2)
        },

        toCurrencyFormat(value)
        {
            return new Intl.NumberFormat('hu-HU').format(value);

        
        },
        toNumberFormat(value)
        {
            let result = String(value).replace(/\s/g, '')
            result = result.replace(',','.')
            return result
        }
    },
    data() {
        return{
            fromValue: "",
            fromCurrency:"HUF",
            toCurrency:"DKK",
            toValue:null

        }
    },




    
}
</script>



<style scoped>
.main-container{
    position: absolute;
    height: 91%;
    width: 100%;
    background-image: url("./../assets/background.jpg");
    background: whitesmoke;
    color: white;
    text-shadow: 1px 1px 3px rgba(0, 0, 0, 0.466);
}

.exchange-container
{
    position: relative;
    top:10%;
    margin:auto;
    width: 55%;
    height: 45%;
    background-color: rgba(0, 0, 0, 0.336);
    
}



.flex-container
{
    margin: auto;
    display: flex;
    justify-content: space-between;
    width: 85%;

}

.input-container{
    display:flex;
    justify-content: space-between;
    width:45%;
    background: white;
    padding: 5px;
    border-radius: 15px;
    
}

h2{
    padding: 25px;
    font-size: 25px;
    margin: 0;
    color: white;
}

#equal-sign
{
font-size: 30px;
color: black;
}

p
{
    margin: 0;
    text-align: center;
    padding: 6px 0;
}
#result-display
{
    font-size: 30px;
    color: black;
    text-shadow: none;
    width: 70%;
}

input
{
    width: 70%;
    border-radius: 25px;
    text-align: right;
    font-size: 30px;
    border: none;
    font-weight: 550;
}
input:focus
{
outline: none;
}
select
{
    width: 23%;
    border: none;
    font-size: 22px;
    color:#2F7ED8;
    font-weight: 700;
    text-align: right;
}

</style>