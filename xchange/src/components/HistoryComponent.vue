<template>
    <div class="history-container">
        <div class="title-container">
        <button @click="logOpen = !logOpen" id="history-button">&#xE81C;</button>
        <h3>History</h3>
        </div>
                    
            <table v-if="logOpen === true && history.length >0">
                <tr>
                    <th>Converted from</th>
                    <th>Converted to</th>
                    <th>Date</th>
                </tr>   

                 <tr v-for="(item, index) in history" :key="index" >
                    <td>{{history[index].fromValue}} {{history[index].fromCurrency}}</td>
                    <td>{{history[index].toValue}} {{history[index].toCurrency}}</td>
                    <td>{{history[index].time}} </td>
                </tr>     
                
                </table>

                <template v-if="history.length <1 && logOpen===true">
                    <div class="information-container">
                    <p>Your exchange history will be displayed here.</p>
                    <p>This panel can be hidden by clicking on the icon. </p>
                    </div>
                </template>
     </div>
    
</template>

<script>

export default {
    name:"HistoryComponent",

    props:
    {
        history:[]
    },

    data()
    {   return{
            logOpen:true
        }
    }


}

</script>

<style scoped>
#history-button
{
    font-family: "Segoe MDL2 Assets";
}
.title-container{
    display: inline-flex;
}

table
{
    width: 85%;
    margin: auto;
    text-align: center;
    margin-top:20px;
    border: 1px solid #2F7ED8;
    border-collapse:collapse;

}





table th
{
        background: #2F7ED8;
        padding: 10px;
}
table td
{
    background: white;
    color:black;
    padding: 3px 0px;
    border: 1px solid #2F7ED8;
}
p
{
    margin: 0;
    padding-left: 8px;
    padding-bottom: 4px;
    }

.information-container
{
    margin-top:8px
}

.history-container{
    position: relative;
    margin:auto;
    width: 55%;
    margin-top: 25px;
    color: white;
    text-shadow: 1px 1px 3px rgba(0, 0, 0, 0.466);
    padding: 25px;
    background-color: rgba(0, 0, 0, 0.74);
    text-align: left;
    margin-bottom: 45px;
    transition: width 2s, height 2s;
}

h3
{
    position: relative;
    display: inline-block;
    margin: 0;
    font-size: 35px;
    letter-spacing: 1px;
    padding:4px;
    font-weight: 500;

    
}

#history-button 
{
    font-size: 30px;
    padding: 8px;
    color: white;
    background: none;
    border: none;
    transition: color 1s, width 0.4s, height 0.4s, font-size 0.4s;

}
#history-button:hover{
    color:#2F7ED8
}
#history-button:focus{
    outline: none;
}



</style>