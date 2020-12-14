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
        },
        switchCurrencies()
        {
            let container = this.fromCurrency
            this.fromCurrency = this.toCurrency
            this.toCurrency = container
            this.onChangeInput()
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
    height: 45%;
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
    padding: 25px;
    font-size: 25px;
    margin: 0;
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
</style>