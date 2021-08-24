<template>
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
</template>

<script>
export default {
  data() {
    return {
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
$background-color: #c5e4e7;

.main {
  background-color: $background-color;
}

:root {
  --Strong-cyan: hsl(172, 67%, 45%);
  --Very-dark-cyan: hsl(183, 100%, 15%);
  --Dark-grayish-cyan1: hsl(186, 14%, 43%);
  --Dark-grayish-cyan2: hsl(184, 14%, 56%);
  --Light-grayish-cyan1: hsl(185, 41%, 84%);
  --Light-grayish-cyan2: hsl(189, 41%, 97%);
  --White: hsl(0, 0%, 100%);
}
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: "Space Mono", monospace;
  font-size: 1.5rem;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #c5e4e7 !important;
}

.tip-button.tip {
  padding: 0.2em 3em 0.2em 1.3em;
  border: 0.16em solid rgba(255, 255, 255, 0);
  text-decoration: none;
  font-size: 1.3rem;
  color: #ffffff;
  text-shadow: 0 0.04em 0.04em rgba(0, 0, 0, 0.35);
  text-align: center;
  justify-content: center;
  transition: all 0.2s;
}

.logo-container {
  display: flex;
  justify-content: center;
  margin-bottom: 5rem;
}
.main-container {
  background-color: var(--White);
  width: 57.5rem;
  display: flex;
  border-radius: 25px;
}
.bill-price {
  flex-basis: 50%;
  margin: 3rem;
}
.billing {
  position: relative;
}
.bill {
  color: var(--Dark-grayish-cyan1);
  font-weight: 700;
  font-size: 1rem;
  margin-bottom: 0.5rem;
}
.bill-input {
  padding: 0.25rem 1rem 0.25rem 1rem;
  border: 2px solid transparent;
  background-color: var(--Light-grayish-cyan2);
  width: 100%;
  text-align: right;
  font-family: "Space Mono", monospace;
  font-weight: 700;
  font-size: 1.5rem;
  outline: none;
  border-radius: 5px;
  color: var(--Very-dark-cyan);
  cursor: pointer;
}
::placeholder {
  color: var(--Dark-grayish-cyan2);
}
.dollar-sign {
  position: absolute;
  left: 1.5rem;
  top: 3rem;
}
.tips-container {
  margin-top: 2.5rem;
}
.tip-text {
  color: var(--Dark-grayish-cyan1);
  font-weight: 700;
  font-size: 1rem;
  margin-bottom: 1rem;
}
.tip-button-loop-contain {
  display: flex;
  justify-content: space-around;
  flex-wrap: wrap;
  text-align: center;
  height: auto;
}
.tip-button {
  width: 30%;
  padding: 0.25rem;
  font-weight: 700;
  background-color: var(--Very-dark-cyan);
  color: var(--White);
  border-radius: 5px;
  margin-bottom: 1rem;
  cursor: pointer;
}
.tip-button:hover {
  color: var(--Very-dark-cyan);
  background-color: #9fe8df;
  transition: 0.4s;
}
.custom {
  padding: 0 0.5rem 0 0;
  height: 3rem;
  border: none;
  background-color: var(--Light-grayish-cyan2);
  width: 30%;
  text-align: right;
  font-family: "Space Mono", monospace;
  font-weight: 700;
  font-size: 1.5rem;
  outline: none;
  border-radius: 5px;
  color: var(--Very-dark-cyan);
}
.people-count-container {
  position: relative;
  margin-top: 2rem;
}
.people-count {
  color: var(--Dark-grayish-cyan1);
  font-weight: 700;
  font-size: 1rem;
  margin-bottom: 0.5rem;
}
.error-msg {
  color: rgba(255, 0, 0, 0.507);
  font-size: 1rem;
  position: absolute;
  left: 15rem;
  top: 0;
  font-weight: 700;
  visibility: hidden;
}
.show-error-msg {
  visibility: visible;
}
.people-count-sign {
  position: absolute;
  left: 1.5rem;
  top: 3rem;
}
.result-container {
  flex-basis: 60%;
  margin: 3rem 3rem 3rem 0;
  padding: 3rem;
  background-color: var(--Very-dark-cyan);
  border-radius: 25px;
}
.tip-amount {
  margin-bottom: 3rem;
  position: relative;
}
.total-amount {
  position: relative;
  margin-bottom: 7rem;
}
.result-paragraph {
  color: var(--White);
  font-size: 1rem;
  font-weight: 700;
}
.desc {
  font-size: 0.8rem;
  color: var(--White);
  opacity: 0.5;
}
.tip-amount-value {
  position: absolute;
  top: -1rem;
  left: 10rem;
  font-size: 3rem;
  font-weight: 700;
  color: var(--Strong-cyan);
}
.resetting-button {
  width: 100%;
  padding: 0.5rem;
  text-transform: uppercase;
  background-color: var(--Strong-cyan);
  font-weight: 700;
  font-size: 1.2em;
  text-align: center;
  color: var(--Very-dark-cyan);
  border-radius: 5px;
  cursor: pointer;
}
.resetting-button:hover {
  background-color: #9fe8df;
  transition: 0.4s;
}
.btn-active {
  background-color: var(--Strong-cyan);
  color: var(--Very-dark-cyan);
  transition: 0.4s;
}
.bill-input:focus {
  border: 2px solid rgb(34, 190, 170);
  transition: 0.4s;
}
.bill-input-error {
  border: 2px solid rgb(190, 34, 34);
  transition: 0.4s;
}

@media screen and (max-width: 920px) {
  .main {
    margin: 5rem 0 5rem 0;
  }
  .main-container {
    width: 23.4rem;
    flex-direction: column;
  }
  .bill-price {
    margin: 2rem;
  }
  .tip-button {
    width: 45%;
  }
  .tip-button.tip {
    width: 35%;
  }
  .custom {
    padding: 0 1rem 0 0;
    width: 45%;
  }
  .result-container {
    margin: 2rem;
    padding: 2rem;
  }
  .total-amount {
    margin-bottom: 2rem;
  }
  .tip-amount-value {
    font-size: 2rem;
    left: 9rem;
  }
}
</style>