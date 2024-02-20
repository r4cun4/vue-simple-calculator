<script setup>

import { onMounted, ref } from 'vue'

const calculatorValue = ref('')
const calculatorElements = ref(['C','','','/',7,8,9,'*',4,5,6,'-',1,2,'3','+',0, '','.','='])
const operator = ref(null)
const previousCalculatorValue = ref('')


// show numbers
const displayValue = (n) => {

  if( !isNaN(n) || n === '.' ) {
    calculatorValue.value += n + ''
  }

}

// reset
const clearValue = (n) => {

  if( n === 'C') {
    calculatorValue.value = ''
    operator.value = null
  }

}

// we check if the operators exists with n argument
// when the user does click we storage the operator selected in n
// then we storage the operator in the reactive variable
// then we storage the first value in the reactive variable previousCalculatorValue
// finally we reset the variable 
const handleOPerator = (n) => {

  if(['/', '*', ',', '-', '+'].includes(n)) {
    operator.value = n
    previousCalculatorValue.value = calculatorValue.value
    calculatorValue.value = ''
  }

}

// calculate
const calculateResult = (n, e) => {

  if( n === '=' ) {
    calculatorValue.value = eval(
      previousCalculatorValue.value + operator.value + calculatorValue.value
    )

    previousCalculatorValue.value = ''
    operator.value = null
  }

}

// actions
const action = (n) => {

  displayValue(n)
  clearValue(n)
  handleOPerator(n)
  calculateResult(n)

}

// keyboard events
const handleEnterKey = () => {

  calculatorValue.value = eval(previousCalculatorValue.value + operator.value + calculatorValue.value);
  previousCalculatorValue.value = '';
  operator.value = null;

};

const handleEscapeKey = () => {

  calculatorValue.value = '';
  operator.value = null;

};

const handleBackspaceKey = () => {

  calculatorValue.value = calculatorValue.value.slice(0, -1);

};

const keyEvt = (e) => {
  action(e.key);

  switch (e.key) {
    case 'Enter':
      handleEnterKey();
      break;
    case 'Escape':
      handleEscapeKey();
      break;
    case 'Backspace':
      handleBackspaceKey();
      break;
  }
};

// Lifecycle call because listeners are not able on divs elements
onMounted(() => {
  document.addEventListener('keydown', keyEvt)
})

</script>

<template>
  <div class="p-3" style="max-width: 400px; margin: 50px auto; background: #234">
    
    <div class="rounded m-1 p-3 text-right lead font-weight-bold text-white bg-vue-dark">
      {{ calculatorValue || 0 }}
    </div>

    <div class="grid grid-cols-4 grid-rows-5">
      <div v-for="n in calculatorElements" :key="n" class="text-white text-center m-1 py-3  rounded bg-vue-dark hover:bg-[#3D5875]"
        :class="{'bg-vue-green': ['C', '+', '/', '*', '-', '+', '='].includes(n) }"
        @click="action(n)"
      >
          {{ n }}
      </div>
    </div>

  </div>
</template>

<style>
  body {
    background: #31475e !important;
  }

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

  div :nth-child(2),
  div :nth-child(3),
  div :nth-child(18) {
    background: rgb(34, 51, 68);
  }
</style>
