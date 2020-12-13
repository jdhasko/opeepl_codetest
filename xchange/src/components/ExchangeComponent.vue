<template>
    <div class="main-container">
        <div class="exchange-container">
                    <h2>Fiat and crypto currency exchange</h2>
                    <div class="flex-container">
                                <div class="input-container">
                                    <input v-model="leftValue"/> 

                                       <select>
                                            <option v-for="c in currencies" :key="c.key" :value="c.value">
                                                {{ c }}
                                            </option>
                                      </select>
                                </div>
                                <p> = </p>
                                <div class="input-container">
                                      <input/> 
                                      <select>
                                            <option v-for="c in currencies" :key="c.key" :value="c.value">
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
        currencies: Number
    },
    methods:{

                    


    },
    data() {
        return{
            leftValue: 1,
            rightCurrency:String,
            rightValue:Number

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
    height: 20%;
    background-color: rgba(0, 0, 0, 0.336);
    
}

.flex-container
{
    margin: auto;
    display: flex;
    justify-content: space-between;
    width: 70%;
}

h2{
    padding: 25px;
    font-size: 25px;
    margin: 0;
    color: white;
}

p
{
    margin: 0;
}

input
{
    width: 200px;
    border-radius: 25px;
    text-align: right;
    font-size: 30px;
}
select
{
    width: 60px;
    height: 23px;
}

</style>