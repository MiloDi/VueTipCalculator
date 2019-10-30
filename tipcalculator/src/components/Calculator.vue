<template>
  <div class="calculator">

<div class = "container"> 
    <cleave
        placeholder="Total Bill"
        v-model="totalBill" 
        :options="options.money" 
        class="form-control" 
        >
    </cleave>
    </div>

<div class = "container"> 
    <vue-dropdown
    :config="config"
    @setSelectedOption="setTipAmount($event);">
    </vue-dropdown>
</div>

<div class = "container"> 
    <cleave
        placeholder="Party Size"
        v-model="partySize" 
        :options="options.quantity" 
        class="form-control" >
    </cleave>
</div>
    <div>
    <button
    class = "calculateButton"
    v-on:click="calculateTip">
        Calculate
    </button>
    </div>


<div class = "text-container">
    <h1>Tip: {{"$"+tipTotal.toFixed(2)}}</h1>
</div>
<div class = "text-container">
    <h1>Total: {{"$"+total.toFixed(2)}}</h1>
</div>


</div>

</template>

<script>
import Cleave from 'vue-cleave-component';
import VueDropdown from 'vue-dynamic-dropdown';
export default {
  name: 'HelloWorld',  
    data () {
        return {
            totalBill: null, 
            partySize: null,
            tipPercentage: null,
            tipTotal: null,
            total: null,
            tipMessage: null,
            options: {
                money: {
                    prefix: '$ ',
                    numeral: true,
                    numeralPositiveOnly: true,
                    noImmediatePrefix: true,
                    rawValueTrimPrefix: true,
                    numeralIntegerScale: 9,
                    numeralDecimalScale: 2
                },
                quantity: {
                    numeral: true,
                    numeralPositiveOnly: true,
                    numeralIntegerScale: 9,
                    numeralDecimalScale: 0
                }
            },            
            config: {
                options: [
                    {
                        value: "25%"
                    },
                    {
                        value: "20%"
                    },
                    {
                        value: "15%"
                    },
                    {
                        value: "10%"
                    },
                ],
                backgroundColor: "white",
                borderRadius: "1.5px",
                placeholder:"Percentage",
                textColor: "grey",
                width:100               
            }
        } 
    }, 
    
    methods:{
        setTipAmount(selectedOption) {
            this.config.placeholder = selectedOption.value;
            this.tipPercentage = parseInt(selectedOption.value)/100;
            // switch(selectedOption){
            //     case "25%":
            //         //change style
            //         this.tipMessage = "Excellent Service"
            //         break;
            //     case "20%":
            //         this.tipMessage = "Great Service"
            //         break;
            //     case "15%":
            //         this.tipMessage = "Subpar Service"
            //         break;
            //     case "10%":
            //         this.tipMessage = "Poor Service"
            //         break;              
            // }
        },
        calculateTip(){
            this.tipTotal = this.totalBill * this.tipPercentage;
            this.total = Number(this.totalBill) + Number(this.tipTotal)
        },
    },
    components: {
        VueDropdown,
        Cleave 
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.calculator{
    display: flexbox;
    width: 20%;
    background-color: lightgrey;
    margin: auto;
    padding: 20px;
    border-radius: 10px;
}
.container{
    display: flexbox;
    width: 80%;
    background-color: lightgrey;
    margin: auto;
    padding: 20px;
    border-radius: 10px;
}
.form-control{
    border:1px;
    border-color:darkgray;
    padding-top: 10px;
    padding-bottom: 5px;
    padding-left: 5px;
    padding-right: 5px;
    font-size: 20px;
    height: 100%;
    width: 100%;    
}
.calculateButton{
    font-size:20px;
    float: right;
}
.text-container{
    font-size: 15px;
    display: flex;
    align-content: flex-start;
}

</style>
