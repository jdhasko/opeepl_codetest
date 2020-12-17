<template>
        <div class="exchange-container">
                    <h2>Fiat and crypto currency exchange</h2>
                    <div class="exchange-header">
                    <p>Convert from</p>
                    <p>Convert to</p>
                    </div>
                    <div class="flex-container">
                                <div class="input-container">
                                    <input v-model="fromValue" @change="onChangeInput" @keypress=" finterNonNumbericInput($event)" placeholder="0"/> 
                                       <select v-model="fromCurrency" @change="onChangeInput" >
                                            <option v-for="c in Object.keys(currencies)" :key="c" :value="c">
                                                {{ c }}
                                            </option>
                                      </select>
                                </div>
                                <div>
                                <button @click="switchCurrencies" id="switch-button">&#xE7FD;</button>
                                <p id="equal-sign"> = </p>
                                    
                                </div>
                                <div class="input-container">
                                      <input v-model="toValue" placeholder="0" disabled/> 
                                      <select v-model="toCurrency" @change="onChangeInput">
                                            <option v-for="c in Object.keys(currencies)" :key="c" :value="c">
                                           {{ c }}
                                            </option>
                                      </select>
                                </div>
                    </div>
                    <div class="flex-container">
                    <p>1 {{ fromCurrency }} = {{valueOfOne}} {{toCurrency}}</p>
                    <p>1 {{ toCurrency }} = {{valueOfOneReverse}} {{fromCurrency}}</p>
                    </div>

                    <div class="flex-container info-container">
                    <div class="description">
                        <p>Fiat data is provided by: <a href="https://exchangeratesapi.io/.">www.exchangeratesapi.io</a></p>
                        <p>Crypto data is provided by: <a href="https://api.binance.com/api/v3/ticker/price"> www.api.binance.com</a></p>



                    </div>
                    <div class="buttons-container">
                    <button  @click="loadCryptoData" id="crypto-button" :class="crypto ? 'inactive' : 'active'" >  
                        <div class="flex-container space-between">
                         <img src="../assets/Bitcoin.png" alt="bitcoin icon" height="30px" width="30px">
                         <label v-if="crypto === false" >Add crypto</label>
                         <label v-if="crypto === true">Remove crypto</label>
                        </div>
                    </button>
                    </div>
                    </div>
        </div>
</template>

<script>
import moment from 'moment'

export default {
    name:'ExchangeComponent',
    props:
    {
        currencies: {},
    },
    data() {
        return{
            fromValue: "",
            fromCurrency:"HUF",
            toCurrency:"DKK",
            toValue:null,
            crypto:false
        }
    },
    methods:{

        /**
         * A method that's called every time the input changes or the currency has been changed. It takes no paramaters.
         * 
         */
        onChangeInput(){

            this.updateProperties(this.fromValue,this.fromCurrency,this.toCurrency)
            if(this.fromValue != 0){this.addDataToHistory(this.fromValue,this.fromCurrency,this.toValue,this.toCurrency)}
        },

        /**
         * Method that updates the properties, calls @method calculate() to exchange the entered value
         * and calls @method toCurrencyFormat() on the displayed amout of money.
         * Takes 3 parameters:
         * @param valueBefore is the amount of money that is to be exchanged.
         * @param currencyFrom is the currency we would like to exchange from.
         * @param currencyTo is the currency we would like to convert to.
         */
        updateProperties(valueBefore,currencyFrom,currencyTo)
        {
            let input = this.toNumberFormat(valueBefore)
            let rateFrom = this.currencies[currencyFrom]
            let rateTo = this.currencies[currencyTo]   
            let result = this.calculate(input,rateFrom,rateTo)
            this.toValue = this.toCurrencyFormat(result)
            this.fromValue = this.toCurrencyFormat(input)
        },


        /**
         * This method prevents user from using non-numeric input.
         */
        finterNonNumbericInput: function(evt) {
            evt = (evt) ? evt : window.event;
            var charCode = (evt.which) ? evt.which : evt.keyCode;
            if ((charCode > 31 && (charCode < 48 || charCode > 57)) && charCode !== 46) {
                evt.preventDefault()}
             else return true;
        
        },

        /**
         * This method adds a new item to the history list. Takes 4 parameters:
         * @param fromValue is the amount of money that got converted.
         * @param fromCurrency is the currency the money got converted from.
         * @param toValue is the amount of money after it got exchanged to the desired currency.
         * @param toCurrency is the currency the money got converted into.
         */
        addDataToHistory(fromValue,fromCurrency,toValue,toCurrency)
        {
            this.$parent.history.push({
                    "fromValue":fromValue,
                    "fromCurrency":fromCurrency,
                    "toValue":toValue,
                    "toCurrency":toCurrency,
                    "time": moment(Date()).format('LTS'),
                })
        },
        
        /**
         * This method is responsible for converting an amount of money to another currency.
         * Takes 3 parameters:
         * @param amount is the amount of money that is to be exchanged.
         * @param rateFrom is the exchange rate of the base currency
         * @param rateTo is the exhange rate of the currency we would like to exchange into.
         * Important! The from and to rates have to be based for the same currency in order to get correct result.
         */
        calculate(amount, rateFrom, rateTo)
        {
            let result = (amount / rateFrom) * rateTo
            return result.toFixed(2)
        },


        /**
         * This method converts numbers into currency format so they are nicely displayed.
         * Takes one parameter:
         * @param value is the number that is to be displayed as currency.
         */
        toCurrencyFormat(value)
        {
            //(adds decimal comma)
            //(could be changed to local prefference)
            return new Intl.NumberFormat('hu-HU').format(value);
        },

        /**
         * This method converts back from the currency format to normal number format
         * Takes one parameter:
         * @param value the number in currency format that is to be converted back to simple number format
         */
        toNumberFormat(value)
        {
            let result = String(value).replace(/\s/g, '')
            result = result.replace(',','.')
            return result
        },


        /**
         * This method is used to switch the currency we convert from with the currency we convert to.
         * It calles @method onChangeInput()
         */
        switchCurrencies()
        {
            let container = this.fromCurrency
            this.fromCurrency = this.toCurrency
            this.toCurrency = container
            this.onChangeInput()
        },

        /**
         * This method calls the @methodloadCrytoCurrencies method in the parent class.
         * It can active or deactive the status of crypto currencies.
         */
        loadCryptoData()
        {
            this.crypto = !this.crypto;
                if(this.crypto){ this.$parent.loadCryptoCurrencies();}
                
                //it's not the best solution because the API is every time to override the excisting data 
                //instead of using the already chased data, but I had issues with that approach, so I settled with this for now.
                else 
                { this.$parent.loadFiatCurrencies();
                      if(this.fromCurrency.length > 3 || this.fromCurrency.length < 3){this.fromCurrency = "EUR"}
                    if(this.toCurrency.length > 3 || this.toCurrency.length < 3){this.toCurrency = "DKK"}
                }
                       
        }
    },


    computed:
    {
        /**
         * Computed property that displays the value of 1 unit of the chosen currency.
         */
        valueOfOne: function()  
        {
            return this.calculate(1,this.currencies[this.fromCurrency],this.currencies[this.toCurrency])
        },

        /**
         * Computed property that displays the value of 1 unit of the chosen currency.
         */
        valueOfOneReverse: function()
        {
              return this.calculate(1,this.currencies[this.toCurrency],this.currencies[this.fromCurrency])
        }
    }




    
}
</script>



<style scoped>

.exchange-container
{
    position: relative;
    margin:auto;
    margin-top: 50px;
    width: 55%;
    height: auto;
    top: 10%;
    padding: 25px;
    background-color: rgba(0, 0, 0, 0.74);
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
    margin: 10px;
    
}

h2{
    padding: 5px;
    font-size: 25px;
    margin:0;
    margin-bottom: 15px;
    color: white;
}

#equal-sign
{
font-size: 25px;
color: white;
}

p
{
    margin: 0;
    text-align: center;
    padding: 0px 0;
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
    width: 74%;
    text-align: right;
    font-size: 28px;
    border: none;
    font-weight: 550;
}
input:disabled
{
    color: black;
}

input::placeholder
{
    color:gray;
    font-weight: 500;
    font-size: 25px;
    margin-bottom:3px;
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
select:hover
{
    width: 23%;
    border: none;
    font-size: 22px;
    color:#2a5e99;
    font-weight: 700;
    text-align: right;
}
select:focus
{
    outline: none;
}

.exchange-header p
{
    width: 95px;
    display: inline;
}

.exchange-header
{
    width: 90%;
    margin-left: 4%;
    display: flex;
    justify-content:space-around;
}

#switch-button
{
    height: 35px;
    width: 35px;
    background: none;
    font-family:'Segoe MDL2 Assets';
    color: white;
    border: none;
    font-size: 20px;
    text-align: center;

    transition: color 1s, font-size 0.4s;
}

#switch-button:focus{
    outline: none;
    border: none;
}

#switch-button:hover
{
    font-size: 23px;
    color: #2F7ED8;
    font-weight: 800;
}

.buttons-container
{
    height: auto;
    text-align: right;
}


.active , .inactive{
    height: 40px;
    width: 180px;
    justify-content: space-between;
    font-size: 13px;
    background: #ff9416;
    color:white;
    border: 0;
    border-radius: 15px;
    text-shadow: 1px 1px 3px rgb(0, 0, 0);
    transition: color 1s, width 0.4s, height 0.4s, font-size 0.4s;
    text-align: center ;
    margin-top: 35px;
    position: relative;
    margin-bottom:5px;
}
.inactive
{
    background: #3f3d3c;

}

.active:hover, .inactive:hover{
    background: #e78009;
    height: 44px;
    width: 198px;
    font-size: 16px;
}

.inactive:hover{
    background: #636260;
}
#crypto-button:active{
    background: #bd6b0d;

}
#crypto-button:focus{
    outline:none;
}

#crypto-button label{
    padding: 8px;
    width: 70%;
}
#crypto-button img{
    height: 32px;
    width: 32px;
        display: inline-block;
}
.space-between
{
    padding: 3px 0;
    justify-content: space-between;
}
.info-container{
    margin-top: 20px;
    height:80px;
}
.description
{
    margin-top: 35px;

}
.description p
{   
    text-align: left;
}


</style>