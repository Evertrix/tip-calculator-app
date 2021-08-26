<template>
<div :class="scssStyles">
  <main class="main">
    <div class="logo-container">
      <img src="../assets/images/logo.svg" alt="splitter-logo" class="logo" />
    </div>
    <div class="main-container">
      <div class="bill-price">
        <div class="billing">
          <p class="bill">Bill</p>
          <input
            class="bill-input"
            id="billing-id"
            type="number"
            v-model="currentBill"
          />
          <img
            src="../assets/images/icon-dollar.svg"
            alt="icon-dollar"
            class="dollar-sign"
          />
        </div>
        <div class="tips-container">
          <p class="tip-text">Select Tip %</p>
          <div class="container">
            <div class="row">
              <div
                class="col-md-4 tip-button-loop-contain tip"
                v-for="(tip, index) in selectedTips"
                :key="tip"
                @change="currentBill"
              >
                <button class="tip-button tip" @click="setTipPercentage(index)">
                  {{ tip }}%
                </button>
              </div>
            </div>
          </div>
          <input
            class="bill-input custom"
            type="text"
            name="tip-input"
            id="tip-input"
            placeholder="Custom"
            @input="setTipPercentage(index)"
            v-model="customTip"
          />
        </div>
        <div class="people-count-container">
          <p class="people-count">Number of people</p>
          <div class="error-msg">Can't be zero</div>
          <input
            class="bill-input"
            id="people-input"
            type="text"
            placeholder="0"
            v-model="personResult"
          />
          <img
            src="../assets/images/icon-person.svg"
            alt="icon-person"
            class="people-count-sign"
          />
        </div>
      </div>
      <div class="result-container">
        <div class="tip-amount">
          <p class="result-paragraph">Tip Amount</p>
          <p class="desc">/ person</p>
          <div class="tip-amount-value">
            <p class="tip-tip-amount-value" v-if="this.personResult !== ''">
              ${{ this.resultTipAmount().toFixed(2) }}
            </p>
            <p class="tip-tip-amount-value" v-else>${{ 0 }}</p>
          </div>
        </div>
        <div class="total-amount">
          <p class="result-paragraph">Total</p>
          <p class="desc">/ person</p>
          <div class="tip-amount-value">
            <p class="total-tip-amount-value" v-if="this.personResult !== ''">
              ${{ this.totalAmount().toFixed(2) }}
            </p>
            <p class="total-tip-amount-value" v-else>${{ setBilling || 0 }}</p>
          </div>
        </div>
        <div class="resetting-button reset" @click="clearAll">Reset</div>
      </div>
    </div>
  </main>
  </div>
</template>

<script>
import scssStyles from '../assets/styles/style.scss'
export default {
  data() {
    return {
      scssStyles,
      currentBill: "",
      billingResult: "",
      selectedTips: [5, 10, 15, 25, 50],
      chosenTip: "",
      customTip: "",
      currentPerson: "",
      personResult: "",
      totalBill: "",
      totalAmounting: "",
    };
  },
  computed: {
    setBilling() {
      this.billingResult = this.currentBill;
      return this.billingResult;
    },
  },
  methods: {
    setTipPercentage(index) {
      if (this.selectedTips[index]) {
        this.customTip = "";
        this.chosenTip = this.selectedTips[index];
      } else if (this.customTip) {
        this.selectedTips[index] = "";
        this.chosenTip = this.customTip;
      }
    },

    setTotalPerson() {
      this.personResult = this.currentPerson;
      return this.personResult;
    },

    resultTipAmount(event) {
      if (
        this.chosenTip === 0 ||
        this.currentBill === 0 ||
        this.currentBill === ""
      ) {
        this.totalBill = null;
        return;
      }
      this.totalBill =
        (this.currentBill * (this.chosenTip / 100)) / this.personResult;
      return this.totalBill;
    },

    totalAmount() {
      this.totalAmounting =
        this.billingResult / this.personResult + this.totalBill;
      return this.totalAmounting;
    },

    clearAll() {
      Object.assign(this.$data, this.$options.data());
    },
  },
};
</script>

<style lang="scss">
</style>