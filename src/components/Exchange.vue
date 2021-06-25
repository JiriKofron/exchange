<template>
  <main class="exchange">
    <h1>Exchange</h1>
    <div class="saved">
      Zapamatovaná hodnota konverze: {{ this.savedAmount }}
    </div>
    <section class="exchange__from">
      <input type="number" v-model="amount" @focus="getExchangeRate()" />

      <select v-model="originalCurrency" @click="getExchangeRate()">
        <option
          v-for="currency in currencies"
          :key="currency.code"
          v-bind:value="currency.rate"
          >{{ currency.code }}</option
        >
      </select>
    </section>
    <section class="exchange__to">
      <div class="exchange__to__results">
        <span> {{ getExchangeResult }}</span>
      </div>
      <select v-model="exchangeCurrency" @click="getExchangeRate()">
        <option
          v-for="currency in currencies"
          :key="currency.code"
          v-bind:value="currency.rate"
        >
          {{ currency.code }}
        </option>
      </select>
    </section>
    <button
      @click="
        showModal();
        saveExchange();
        alwaysUSD();
      "
    >
      Zapamatovat
    </button>
    <Modal
      v-show="isModalVisible"
      @close="closeModal"
      :savedAmount="this.savedAmount"
    />
  </main>
</template>

<style scoped>
.exchange {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  font-size: 2rem;
}

.exchange__from {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 1rem;
  background: rgba(200, 100, 50, 0.3);
}

.exchange__to {
  background: rgba(50, 100, 200, 0.3);
  display: flex;
  align-items: center;
  /*border: 1px solid green;*/
  padding: 1rem;
  justify-content: space-between;
}

.exchange__from,
.exchange__to {
  width: 15rem;
  height: 3rem;
}

.exchange__to__results {
  display: flex;
  flex-direction: column;
}

.saved {
  width: 70%;
  margin: 2rem;
  padding: 1rem;
}
</style>

<script>
import Modal from './Modal.vue';
export default {
  name: 'Exchange',
  components: {
    Modal,
  },
  data() {
    return {
      amount: '',
      savedAmount: '',
      originalCurrency: 1,
      exchangeCurrency: 23,
      exchangeRate: '',
      exchangeToUSD: 0,
      exchangeResult: this.result,
      isModalVisible: false,
      currencies: [
        { code: 'CZK', rate: 1 },
        {
          code: 'EUR',
          rate: 26,
        },
        { code: 'USD', rate: 23 },
      ],
    };
  },
  methods: {
    // function that get exchange rates from select boxes
    getExchangeRate: function() {
      this.exchangeRate = this.originalCurrency / this.exchangeCurrency;
      return this.exchangeRate;
    },
    // function for saving exchange after button click
    saveExchange: function() {
      // key is code for the currency - USD, EUR, etc...
      let key = '';
      // looping currencies array to find out which currency is selected, so it can write proper currency code after amount
      for (const item of this.currencies) {
        if (item.rate === this.exchangeCurrency) {
          key = item.code;
        } else {
          console.log(' ');
        }
      }
      // saving the final exchange amount to local storage
      localStorage.getExchangeResult = this.getExchangeResult;
      // saved amount is variable for showing amount and currency code
      this.savedAmount = this.getExchangeResult + ' ' + key;
      // save savedAmount variable to local storage
      localStorage.savedAmount = this.savedAmount;
      return this.savedAmount;
    },
    alwaysUSD: function() {
      let usdRate = this.originalCurrency / 23;
      this.exchangeToUSD = Number(this.amount * usdRate).toFixed(2);
      localStorage.exchangeToUSD = this.exchangeToUSD;
      return this.exchangeToUSD;
    },
    // functions for open and close modal window
    showModal() {
      console.log('showModal');
      this.isModalVisible = true;
    },
    closeModal() {
      console.log('hideModal');
      this.isModalVisible = false;
    },
  },
  // computed function for showing exchange while user is writing input with amount for exchange
  computed: {
    getExchangeResult: function() {
      const result = Number((this.amount * this.exchangeRate).toFixed(2));
      return result;
    },
  },
  //mounted function to know if the saved amount variable is saved to local storage
  mounted() {
    if (localStorage.savedAmount) this.savedAmount = localStorage.savedAmount;
  },
};
</script>
