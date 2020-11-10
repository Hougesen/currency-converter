<template>
  <div>
    <h1>Currency Converter</h1>
    <label for="amount">Amount:</label>
    <input name="amount" type="number" v-model="toConvert" @change="convert" />

    <label for="from">From</label>
    <select name="from" v-model="fromExchangeRate" @change="convert">
      <option
        v-for="(fromExchangeRate, fromCurrencyCode) of exchangeRates"
        v-bind:key="fromCurrencyCode"
      >
        {{ fromCurrencyCode }}
      </option>
    </select>

    <label for="to">To</label>
    <select name="to" v-model="toExchangeRate" @change="convert">
      <option
        v-for="(toExchangeRate, toCurrencyCode) of exchangeRates"
        v-bind:key="toCurrencyCode"
      >
        {{ toCurrencyCode }}
      </option>
    </select>

    <p v-if="fromExchangeRate != undefined && toExchangeRate != undefined">
      {{ toConvert }}
      {{ fromExchangeRate }}
      is equal to
      {{ converted }}
      {{ toExchangeRate }}
    </p>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "CurrencyConverter",

  data() {
    return {
      toConvert: "",
      converted: 0,
      exchangeRates: 1,
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
      console.log(this.exchangeRates[this.fromExchangeRate]);
      this.converted =
        (this.toConvert /
          parseFloat(this.exchangeRates[this.fromExchangeRate])) *
        parseFloat(this.exchangeRates[this.toExchangeRate]);
    }
  }
};
</script>

<style scoped></style>
