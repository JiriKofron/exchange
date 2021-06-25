<template>
  <main>
    <h1>USD wallet</h1>
    <span> Zapamatovaná hodnota konverze: {{ this.savedAmount }} </span>
    <div class="dollarwallet">
      Celková konverze v dolarech: {{ this.totalUSD }} USD
    </div>
  </main>
</template>
<style scoped></style>
<script>
export default {
  name: 'Usdwallet',
  data() {
    return {
      getExchangeResult: localStorage.getExchangeResult,
      exchangeToUSD: localStorage.exchangeToUSD,
      savedAmount: localStorage.savedAmount,
      addUsd: 0,
      totalUSD: 0,
    };
  },
  mounted() {
    console.log(`Exchange result: ${this.getExchangeResult}`);
    console.log(`Local storage addUsd: ${localStorage.addUsd}`);
    //condition to find out, if it is first conversion or not, so the value for total dollar exchange is properly counted
    if (localStorage.addUsd === undefined) {
      this.addUsd = this.exchangeToUSD;
      localStorage.addUsd = this.addUsd;
      this.totalUSD = this.exchangeToUSD;
      localStorage.totalUSD = this.totalUSD;
      console.log('první sčítání');
      //condition to prevent addition to total USD conversion, if no new amount is given to input
    } else if (this.exchangeToUSD === localStorage.addUsd) {
      console.log('jen překliknutí bez konverze');
      this.totalUSD = localStorage.totalUSD;
    }
    // else statement, for first counting so there won't be wrong addition
    else {
      //total amount of USD counted from result from exchange and the helping variable for counting
      let n =
        parseFloat(this.exchangeToUSD) + parseFloat(localStorage.totalUSD);
      this.totalUSD = n.toFixed(2);
      console.log(this.totalUSD);
      //helping variable to safe the value of previous conversion, so in case that two following conversions are the same, the total usd count won't be rised
      localStorage.addUsd = localStorage.exchangeToUSD;
      localStorage.totalUSD = this.totalUSD;
    }
  },
};
</script>
