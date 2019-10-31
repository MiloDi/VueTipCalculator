<template>
  <div class="calculator">
    <div class="container">
      <cleave
        placeholder="Total Bill"
        v-model="totalBill"
        :options="options.money"
        class="form-control"
      ></cleave>
    </div>

    <div class="container">
      <vue-dropdown :config="config" @setSelectedOption="setTipAmount($event);"></vue-dropdown>
      <div class="text-container-small">
        <h1 v-if="tipMessage != null">{{tipMessage}}</h1>
      </div>
    </div>

    <div class="container">
      <cleave
        placeholder="Party Size"
        v-model="partySize"
        :options="options.quantity"
        class="form-control"
      ></cleave>
    </div>
    <div>
      <button class="calculateButton" v-on:click="calculateTip">Calculate</button>
    </div>

    <div class="text-container-small" v-if="party">
      <h1>Each party pays:</h1>
    </div>
    <div class="container">
      <div class="text-container">
        <h1>Tip:</h1>
        <h1 v-if="tipTotal != null">{{"$"+tipTotal.toFixed(2)}}</h1>
      </div>
      <div class="text-container">
        <h1>Total:</h1>
        <h1 v-if="total != null">{{"$"+total.toFixed(2)}}</h1>
      </div>
    </div>
  </div>
</template>

<script>
import Cleave from "vue-cleave-component";
import VueDropdown from "vue-dynamic-dropdown";
export default {
  name: "Calculator",
  data() {
    return {
      totalBill: null,
      partySize: null,
      tipPercentage: null,
      tipTotal: null,
      tipMessage: null,
      total: null,
      party: false,

      //Options segement for Cleave objects
      options: {
        money: {
          prefix: "$ ",
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

      //Configuration for dropdown menu
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
          }
        ],
        backgroundColor: "white",
        borderRadius: "1.5px",
        placeholder: "Percentage",
        textColor: "grey",
        width: 180
      }
    };
  },

  methods: {
    //Set the tip percentage from the dropdown menu
    setTipAmount(selectedOption) {
      this.config.placeholder = selectedOption.value;
      this.tipPercentage = parseInt(selectedOption.value) / 100;
      //Configure a message to render based on the tip percentage
      switch (selectedOption.value) {
        case "25%":
          this.tipMessage = "Excellent Service";
          break;
        case "20%":
          this.tipMessage = "Good Service";
          break;
        case "15%":
          this.tipMessage = "Subpar Service";
          break;
        case "10%":
          this.tipMessage = "You should tip better";
          break;
        default:
          break;
      }
    },
    //Exceute the calculation for the total bill and tip amount
    calculateTip() {
      this.checkPartySize();
      this.tipTotal = this.totalBill * this.tipPercentage;
      this.total = Number(this.totalBill) + Number(this.tipTotal);
    },
    //If the bill is to be split, divide the bill and tip amount between the party
    checkPartySize() {
      if (!isNaN(this.partySize) && this.partySize > 1) {
        this.party = true;
        this.tipTotal = this.tipTotal / this.partySize;
        this.totalBill = this.totalBill / this.partySize;
      }
    }
  },
  components: {
    VueDropdown,
    Cleave
  }
};
</script>


<style scoped>
.calculator {
  display: flexbox;
  width: 20%;
  background-color: lightgrey;
  margin: auto;
  padding: 20px;
  border-radius: 10px;
}
.container {
  display: flexbox;
  width: 80%;
  background-color: lightgrey;
  margin: auto;
  padding: 20px;
  border-radius: 10px;
}
.form-control {
  border: 1px;
  border-color: darkgray;
  padding-top: 10px;
  padding-bottom: 5px;
  padding-left: 5px;
  padding-right: 5px;
  font-size: 20px;
  height: 100%;
  width: 100%;
}
.calculateButton {
  font-size: 20px;
  float: right;
}
.text-container {
  font-size: 15px;
  display: flex;
  align-content: flex-start;
}
.text-container-small {
  font-size: 10px;
  display: flexbox;
  margin: auto;
  padding-top: 10px
}
</style>
