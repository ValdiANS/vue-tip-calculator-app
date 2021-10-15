<template>
  <div class="tip-calculator-container">
    <div class="calculator">
      <form id="calculator">
        <div class="input-tag">
          <label for="bill">Bill</label>
          <span v-if="isInputError(bill)">Can't be zero</span>
          <div class="input-elemen">
            <span>$</span>
            <input
              type="number"
              inputmode="numeric"
              id="bill"
              min="0"
              placeholder="0"
              v-model="bill"
              @input="billHandler"
              :class="{ error: isInputError(bill)}"
            >
          </div>
        </div>

        <section class="select-tip">
          <label for="customTip">Select Tip %</label>

          <div class="tip-container">
            <input type="radio" name="tip-option-list" value="5" id="option1">
            <div class="option">
              <label for="option1" @click="setTip(5)">5%</label>
            </div>

            <input type="radio" name="tip-option-list" value="10" id="option2">
            <div class="option">
              <label for="option2" @click="setTip(10)">10%</label>
            </div>

            <input type="radio" name="tip-option-list" value="15" id="option3">
            <div class="option">
              <label for="option3" @click="setTip(15)">15%</label>
            </div>

            <input type="radio" name="tip-option-list" value="25" id="option4">
            <div class="option">
              <label for="option4" @click="setTip(25)">25%</label>
            </div>

            <input type="radio" name="tip-option-list" value="50" id="option5">
            <div class="option">
              <label for="option5" @click="setTip(50)">50%</label>
            </div>

            <input type="radio" name="tip-option-list" :value="customTip" id="option6">
            <div class="option">
              <label for="option6">
                <input
                  type="number"
                  inputmode="numeric"
                  id="customTip"
                  min="0"
                  placeholder="custom"
                  @focus="customTipFocus"
                  v-model="customTip"
                  @input="setTip(customTip)"
                  @click="setTip(customTip)"
                >
              </label>

            </div>
          </div>
        </section>

        <div class="input-tag">
          <label for="peopleNumber">Number of People</label>
          <span v-if="isInputError(numberOfPeople)">Can't be zero</span>
          <div class="input-elemen">
            <span>
              <i class="fas fa-user"></i>
            </span>
            <input
              type="number"
              inputmode="numeric"
              id="peopleNumber"
              min="0"
              placeholder="0"
              v-model="numberOfPeople"
              :class="{ error: isInputError(numberOfPeople)}"
            >
          </div>
        </div>
      </form>
    </div>

    <div class="result">
      <div class="information-container">
        <div class="information tip-amount">
          <div class="caption">
            <p>Tip Amount</p>
            <span>/ person</span>
          </div>

          <div class="number">
            <h2>${{ tipAmount.toFixed(2) }}</h2>
          </div>
        </div>

        <div class="information total">
          <div class="caption">
            <p>Total</p>
            <span>/ person</span>
          </div>

          <div class="number">
            <h2>${{ total.toFixed(2) }}</h2>
          </div>
        </div>
      </div>

      <button
        type="reset"
        form="calculator"
        :disabled="disableReset"
        @click="reset"
        class="reset"
      >
        Reset
      </button>
    </div>
  </div>
</template>

<script>
import { ref, computed } from 'vue';

export default {
  name: 'TipCalculatorApp',
  setup() {
    const bill = ref();
    const numberOfPeople = ref();
    const customTip = ref();
    const tip = ref(0);
    const tipAmount = computed(() => {
      let tipAmountResult = (bill.value * (tip.value / 100)) / numberOfPeople.value;

      if (Number.isNaN(tipAmountResult) || tipAmountResult === Infinity) {
        tipAmountResult = 0;
      }

      return tipAmountResult;
    });

    const total = computed(() => {
      let totalResult = (bill.value / numberOfPeople.value + tipAmount.value);

      if (Number.isNaN(totalResult) || totalResult === Infinity) {
        totalResult = 0;
      }

      return totalResult;
    });

    const customTipFocus = () => {
      document.getElementById('option6').checked = true;
    };

    const setTip = (amount) => {
      if (amount > 100) {
        customTip.value = 100;
        tip.value = 100;
        return;
      }

      tip.value = amount;
    };

    const reset = () => {
      bill.value = null;
      numberOfPeople.value = null;
      customTip.value = null;
      setTip(0);
      document.querySelector('form#calculator').reset();
    };

    const disableReset = computed(
      () => !(bill.value || numberOfPeople.value || customTip.value || tip.value),
    );

    const isInputError = (value) => value === 0;

    const log = (test) => console.log(test);
    return {
      customTipFocus,
      customTip,
      tipAmount,
      total,
      bill,
      numberOfPeople,
      tip,
      setTip,
      reset,
      disableReset,
      log,
      isInputError,
    };
  },
};
</script>

<style scoped lang="scss">
// Variables
$primary: hsl(172, 67%, 45%);
$veryDarkCyan: hsl(183, 100%, 15%);
$darkGrayishCyan: hsl(186, 14%, 43%);
$darkGrayishCyan2: hsl(184, 14%, 56%);
$lightGrayishCyan: hsl(185, 41%, 84%);
$lightGrayishCyan2: hsl(189, 41%, 97%);
$white: hsl(0, 0%, 100%);

// Mixins
@mixin label_style {
  color: grey;
  font-weight: 700;
  font-size: 0.8em;
}

@mixin primary_outline($width: 3px) {
  outline: $primary solid $width;
}

@mixin error_outline($width: 3px) {
  outline: hsl(0, 83%, 65%) solid $width;
}

// Styles

// Tip Calculator Container
.tip-calculator-container {
  width: 100%;
  max-width: 800px;
  display: flex;
  flex-direction: column;
  background-color: #ffffff;
  border-radius: 20px 20px 0 0;
  padding: 30px;
  font-size: 1em;
}

// Calculator
.calculator {
  width: 100%;
  padding: 20px 0;

  .input-tag {
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-template-rows: auto auto;
    row-gap: 5px;

    label {
      @include label_style();
    }

    & > span {
      @include label_style();
      color: hsl(0, 84%, 56%);
      text-align: right;
    }

    .input-elemen {
      position: relative;
      grid-column-start: 1;
      grid-column-end: 3;

      span {
        color: $lightGrayishCyan;
        position: absolute;
        font-size: 1em;
        left: 10px;
        bottom: 0;
        height: 100%;
        display: grid;
        place-items: center;

        i {
          font-size: 0.8em;
        }
      }

      input {
        border: none;
        border-radius: 5px;
        width: 100%;
        padding: 5px 10px;
        background-color: $lightGrayishCyan2;
        color: $veryDarkCyan;
        font-family: inherit;
        font-weight: 700;
        font-size: 1.1em;

        &::placeholder {
          color: $lightGrayishCyan;
        }

        &:focus {
          @include primary_outline(2px);
        }

        &.error {
          @include error_outline(2px);
        }
      }
    }
  }

  .select-tip {
    margin: 25px 0;

    & > label {
      @include label_style();
    }

    .tip-container {
      display: grid;
      grid-template-columns: 1fr 1fr;
      grid-template-rows: 1fr 1fr 1fr;
      gap: 10px 10px;
      margin: 10px 0;

      & > input {
        display: none;

        &:checked + .option {
          background-color: $primary;

          label {
            color: $veryDarkCyan;
          }
        }

        &#option6:checked + .option {
          @include primary_outline(2px);

          transition: none;

          input {
            &:focus {
              outline: none;
            }
          }
        }
      }

      .option {
        background-color: $veryDarkCyan;
        border-radius: 5px;
        text-align: center;
        padding: 5px 30px;

        transition: .2s ease-in;

        &:hover {
          background-color: $lightGrayishCyan;

          label {
            color: $veryDarkCyan;
          }
        }

        &:last-child {
          position: relative;
        }

        label {
          color: $white;
          font-weight: 700;
          font-size: 1.1em;
          cursor: pointer;

          input[type=number] {
            font-family: inherit;
            width: 100%;
            height: 100%;
            position: absolute;
            top: 0;
            left: 0;
            padding: 5px 10px;
            border: none;
            border-radius: 5px;
            background-color: $lightGrayishCyan2;
            color: $veryDarkCyan;
            font-weight: 700;
            font-size: 1em;
            cursor: pointer;

            &::placeholder {
              text-align: center;
              text-transform: capitalize;
              font-size: 1.1em;
            }
          }
        }
      }
    }
  }

  input[type=number] {
    text-align: right;
    -moz-appearance: textfield;

    &::-webkit-outer-spin-button,
    &::-webkit-inner-spin-button {
      -webkit-appearance: none;
      margin: 0;
    }
  }
}

// Result
.result {
  width: 100%;
  padding: 20px;
  background-color: $veryDarkCyan;
  color: $white;
  border-radius: 10px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;

  .information {
    display: flex;
    justify-content: space-between;
    column-gap: 30px;
    margin-bottom: 20px;

    .caption {
      font-size: 0.8em;
      margin: auto 0;

      span {
        font-size: 0.9em;
        color: $lightGrayishCyan;
        opacity: 0.7;
        width: 100%;
      }
    }

    .number {
      font-size: 1.5em;
      color: $primary;
    }
  }

  .reset {
    width: 100%;
    background-color: $primary;
    color: $veryDarkCyan;
    border: none;
    border-radius: 5px;
    font-family: inherit;
    text-transform: uppercase;
    font-weight: 700;
    font-size: 1em;
    padding: 5px;
    cursor: pointer;

    transition: .2s ease-in;

    &:disabled {
      opacity: 0.5;
    }

    &:hover {
      background-color: $lightGrayishCyan;
    }
  }
}

@media screen and (min-width: 430px) {
  // Tip Calculator Container
  .tip-calculator-container {
    border-radius: 20px;
  }

  // Calculator
  .calculator {

    .select-tip {

      .tip-container {
        grid-template-columns: 1fr 1fr 1fr;
        grid-template-rows: 1fr 1fr;
      }
    }
  }
}

@media screen and (min-width: 768px) {
  // Tip Calculator Container
  .tip-calculator-container {
    flex-direction: row;
  }

  // Calculator
  .calculator {
    padding: 10px 20px 10px 0;
  }

  .result {
    padding: 30px;
  }
}

</style>
