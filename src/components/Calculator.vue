<template>
  <div class="p-3 rounded" style="max-width: 400px; margin: 100px auto; background: #234">

    <!--Calculator Result-->
    <div class="w-full rounded m-1 p-3 text-end lead font-weight-bold text-white bg-vue-dark">
      {{calculatorValue || 0}}
    </div>

    <!--Calculator Buttons-->
    <div class="row no-gutters">
      <div class="col-3" v-for="numbers in calculatorElements" :key="numbers">
        <div class="lead text-white text-center m-1 py-2 bg-vue-dark rounded hover-class" 
        :class="{'bg-vue-green': ['C', '*', '/', '-', '+', '%', '+/-', '='].includes(numbers)}"
        @click="action(numbers)">
          {{numbers}}
        </div>
      </div>
      <div class="col" v-for="extraButton in extraElements" :key="extraButton">
        <div class="lead text-white text-center m-1 py-2 bg-vue-dark rounded hover-class" 
        :class="{'bg-vue-green': extraButton === '='}"
        @click="action(extraButton)">
          {{extraButton}}
        </div>
      </div>
    </div>

  </div>
</template>

<script>

export default {
  name: 'CalculatorApp',
  props: {
    msg: String
  },

  data() {
    return {
      calculatorValue: '',
      calculatorElements: ['C', '*', '/', '-', 7, 8, 9, '+', 4, 5, 6, '%', 1, 2, 3, '+/-', 0, '.'],
      extraElements: ['='],
      operator: null,
      previousCalculatorValue: ''
    }
  },

  methods: {
    action(numbers) {
      if (!isNaN(numbers) || numbers === '.') {
        this.appendValue(numbers);
      } else if (numbers === 'C') {
        this.clearValue();
      } else if (numbers === '%') {
        this.calculatePercentage();
      } else if (['*', '/', '-', '+'].includes(numbers)) {
        this.setOperator(numbers);
      } else if (numbers === '=') {
        this.calculateResult();
      } else if (numbers === '+/-') {
        this.toggleSign();
      }
    },

    appendValue(numbers) {
      if (this.calculatorValue === '' && numbers === 0) return;
      if (this.calculatorValue === '0' && !isNaN(numbers)) {
        this.calculatorValue = numbers + '';
      } else {
        this.calculatorValue += numbers + '';
      }
    },

    clearValue() {
      this.calculatorValue = '';
      this.previousCalculatorValue = '';
      this.operator = null;
    },

    calculatePercentage() {
      this.calculatorValue = this.calculatorValue / 100 + '';
    },

    setOperator(operator) {
      if (this.calculatorValue !== '') {
        this.operator = operator;
        this.previousCalculatorValue = this.calculatorValue;
        this.calculatorValue = '';
      }
    },

    calculateResult() {
      if (this.previousCalculatorValue !== '' && this.operator !== null && this.calculatorValue !== '') {
        try {
          this.calculatorValue = eval(
            this.previousCalculatorValue + this.operator + this.calculatorValue
          ).toString();
          if (isNaN(this.calculatorValue)) {
            this.calculatorValue = 'NaN';
          }
        } catch (e) {
          this.calculatorValue = 'Error!';
        }
        this.previousCalculatorValue = '';
        this.operator = null;
      }
    },

    toggleSign() {
      if (this.calculatorValue !== '') {
        this.calculatorValue = (parseFloat(this.calculatorValue) * -1).toString();
      }
    }
  }
}
</script>

<style scoped>
.bg-vue-dark {
  background: #31475e;
}
.hover-class:hover {
  cursor: pointer;
  background: #3D5875;
}
.bg-vue-green {
  background: #3fb984;
}
</style>
