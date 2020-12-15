<template>
    <div class="main-container">
        <div class="exchange-container">
                    <h2>Fiat and crypto currency exchange</h2>
                    <div class="exchange-header">
                    <p>Convert from</p>
                    <p>Convert to</p>
                    </div>
                    <div class="flex-container">
                                <div class="input-container">
                                    <input v-model="fromValue" @change="onChangeInput" placeholder="0"/> 
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
                    <button  @click="loadCryptoData" id="crypto-button" >  
                        <div class="flex-container space-between">
                         <img src="../assets/Bitcoin.png" alt="bitcoin icon" height="30px" width="30px">
                         <label v-if="crypto === false" >Add crypto</label>
                         <label v-if="crypto === true">Remove crypto</label>
                        </div>
                    </button>
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
        onChangeInput()
        {
            let input = this.toNumberFormat(this.fromValue)
            let rateFrom = this.currencies[this.fromCurrency]
            let rateTo = this.currencies[this.toCurrency]   
            let result = this.calculate(input,rateFrom,rateTo)
            this.toValue = this.toCurrencyFormat(result)
            this.fromValue = this.toCurrencyFormat(input)

        },
        
        calculate(amount, rateFrom, rateTo)
        {
            let result = (amount / rateFrom) * rateTo
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
        },
        switchCurrencies()
        {
            let container = this.fromCurrency
            this.fromCurrency = this.toCurrency
            this.toCurrency = container
            this.onChangeInput()
        },
        loadCryptoData()
        {

            this.crypto = !this.crypto;
            if(this.crypto){
           this.$parent.loadCryptoCurrencies();
            }
            else
            {
            this.$parent.loadFiatCurrencies();
            }

        }
    },
    computed:
    {
        valueOfOne: function()  
        {
            return this.calculate(1,this.currencies[this.fromCurrency],this.currencies[this.toCurrency])
        },
        valueOfOneReverse: function()
        {
              return this.calculate(1,this.currencies[this.toCurrency],this.currencies[this.fromCurrency])
        }
    }




    
}
</script>



<style scoped>
.main-container{
    position: absolute;
    height: 91%;
    width: 100%;
    background-image: url("./../assets/background.jpg");
    color: white;
    text-shadow: 1px 1px 3px rgba(0, 0, 0, 0.466);

}

.exchange-container
{
    position: relative;
    top:10%;
    margin:auto;
    width: 55%;
    height: auto;
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
    border-radius: 25px;
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

#crypto-button{
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

}

#crypto-button:hover{
    background: #e78009;
    height: 44px;
    width: 198px;
    font-size: 16px;
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