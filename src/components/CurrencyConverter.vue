<template>
  <div>
    <h1>Currency Converter</h1>
    <form>
      <div class="formGroup">
        <label for="amount">Amount:</label>
        <input
          name="amount"
          type="number"
          v-model="toConvert"
          @change="convert"
        />
      </div>

      <div class="formGroup">
        <label for="from">From</label>
        <select name="from" v-model="fromExchangeRate" @change="convert">
          <option
            v-for="(fromExchangeRate, fromCurrencyCode) of exchangeRates"
            v-bind:key="fromCurrencyCode"
          >
            {{ fromCurrencyCode }}
          </option>
        </select>
      </div>

      <div class="formGroup">
        <label for="to">To</label>
        <select name="to" v-model="toExchangeRate" @change="convert">
          <option
            v-for="(toExchangeRate, toCurrencyCode) of exchangeRates"
            v-bind:key="toCurrencyCode"
          >
            {{ toCurrencyCode }}
          </option>
        </select>
      </div>

      <div class="formGroup">
        <label for="converted">Converted</label>
        <input type="number" disabled v-model="converted" />
      </div>
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "CurrencyConverter",

  data() {
    return {
      toConvert: "",
      converted: "",
      exchangeRates: undefined,
      fromExchangeRate: undefined,
      toExchangeRate: undefined
    };
  },

  mounted() {
    axios
      .get("https://open.exchangerate-api.com/v6/latest")
      .then(res => (this.exchangeRates = res.data.rates));
  },

  methods: {
    convert() {
      // Check if both exchange rates are set
      if (this.fromExchangeRate != undefined && this.toExchangeRate) {
        // Convert the given amount to USD, since the base currency of the api is USD
        let toUSD =
          this.toConvert /
          parseFloat(this.exchangeRates[this.fromExchangeRate]);

        // Convert the amount to the chosen currency
        this.converted =
          toUSD * parseFloat(this.exchangeRates[this.toExchangeRate]);

        // Round the number down to two decimals
        this.converted = Number(Math.round(this.converted + "e2") + "e-2");
      }
    }
  }
};
</script>

<style scoped>
h1 {
  text-align: center;
}
form {
  margin: auto;
}
.formGroup {
  display: inline-block;
  padding: 0.5em;
  margin: auto;
}
label {
  display: block;
}
input,
select {
  width: 100%;
  font-size: 1.4em;
  border: none;
  padding: 10px;
  background-color: #252626;
  color: white;
}
</style>
