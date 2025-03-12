<script setup>
import { ref } from 'vue'
import { evaluate } from 'mathjs'
const inputDisplay = ref('')
const outDisplay = ref('')

const clickToDisplay = (value) => {
  inputDisplay.value += value
}

const clickToClearAll = () => {
  inputDisplay.value = ''
  outDisplay.value = ''
}
// const calculate = () => {
//   console.log(inputDisplay.value)
//   outDisplay.value = evaluate(inputDisplay.value)
// }

const calculate = () => {
  try {
    console.log(inputDisplay.value)
    let processedExpression = inputDisplay.value
    processedExpression = processedExpression.replace(/(\d|\))(?=(sin|cos|tan|log)\()/g, '$1*')
    const functionCount = (processedExpression.match(/(sin\(|cos\(|tan\(|log\()/g) || []).length
    const closingCount = (processedExpression.match(/\)/g) || []).length
    const missingBrackets = functionCount - closingCount
    if (missingBrackets > 0) {
      processedExpression += ')'.repeat(missingBrackets)
    }
    if (inputDisplay.value === '!') {
      processedExpression = '0!'
    }
    //do for raise to power also
    outDisplay.value = evaluate(processedExpression)
  } catch {
    outDisplay.value = 'ERR'
  }
}

const deleteLastDigit = () => {
  inputDisplay.value = inputDisplay.value.slice(0, -1)
}
</script>

<template>
  <div class="container">
    <input v-model="inputDisplay" id="input" type="text" placeholder="" readonly />
    <input v-model="outDisplay" id="outputDisplay" type="text" placeholder="" readonly />
    <div class="the-keys relative">
      <button class="operator" @click="clickToDisplay('sin(')">sin</button>
      <button class="operator" @click="clickToDisplay('cos(')">cos</button>
      <button class="operator" @click="clickToDisplay('tan(')">tan</button>
      <button class="operator" @click="clickToDisplay('sqrt(')">√</button>
      <button @click="clickToDisplay('pi')">π</button>
      <button @click="clickToDisplay('!')">x!</button>
      <button @click="clickToDisplay('exp')">e</button>
      <button class="operator" @click="clickToDisplay('log(')">log</button>
      <button @click="clickToDisplay('(')">(</button>
      <button @click="clickToDisplay(')')">)</button>
      <button @click="deleteLastDigit()">⌫</button>

      <button class="operator" @click="clickToClearAll()">CE</button>
      <button @click="clickToDisplay('%')">%</button>
      <!-- <button class="operator" @click="clickToDisplay('**')">²</button> -->
      <button @click="clickToDisplay(',')">,</button>
      <button @click="clickToDisplay('/')">÷</button>
      <button class="operator" @click="clickToDisplay('pow(')">xʸ</button>
      <button @click="clickToDisplay('7')">7</button>
      <button @click="clickToDisplay('8')">8</button>
      <button @click="clickToDisplay('9')">9</button>
      <button class="operator" @click="clickToDisplay('*')">×</button>
      <button @click="clickToDisplay('4')">4</button>
      <button @click="clickToDisplay('5')">5</button>
      <button @click="clickToDisplay('6')">6</button>
      <button class="operator" @click="clickToDisplay('-')">-</button>
      <button @click="clickToDisplay('1')">1</button>
      <button @click="clickToDisplay('2')">2</button>
      <button @click="clickToDisplay('3')">3</button>
      <button class="operator" @click="clickToDisplay('+')">+</button>
      <button @click="clickToDisplay('deg')">Deg</button>
      <!-- <button @click="clickToDisplay('00')">00</button> -->
      <button @click="clickToDisplay('0')">0</button>
      <button @click="clickToDisplay('.')">.</button>
      <button class="equals" @click="calculate()">=</button>
    </div>
  </div>
</template>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.container {
  margin: auto;
  /* height: 498px; */
  min-width: 318px;
  max-width: 318px;
  background-color: #26313e;
}
.input-in,
.input-out {
  border: none;
  /* display: flex;
  justify-content: end; */
  /* text-align: right; */
  direction: rtl;
  outline: none;
  padding: 2px 15px;
  min-height: 45px;
  width: 100%;
  font-size: 35px;
  color: white;
  background-color: #78909c;
  overflow-x: scroll;
  scrollbar-width: none;
}
input::placeholder {
  color: white;
}
#input,
#outputDisplay {
  /* border: 1px solid black; */
  border: none;
  text-align: right;
  outline: none;
  padding: 5px 15px;
  /* height: 193px; */
  width: 100%;
  font-size: 35px;
  color: white;
  background-color: #78909c;
  overflow: visible;
}

.advanced-operators {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  /* gap: 1rem; */
  background-color: #26313e;
}

.the-keys {
  display: grid;
  /* gap: .8rem; */
  grid-template-columns: repeat(4, 1fr);
  background-color: #26313e;
}

button {
  height: 58px;
  border: 1px solid #e5e9ec;
  cursor: pointer;
}

.fill {
  grid-row-start: 4;
  grid-row-end: 6;
  grid-column-start: 4;
  height: 100%;
  border-radius: 40% 0;
}
.operator {
  border: 1px solid white;
  background-color: #e4e9ec;
}
.equals {
  background-color: #ff6567;
}
@media (min-width: 300px) and (max-width: 550px) {
  .container {
    width: 100%;
    height: 100%;
  }
}
</style>

<!-- <template>
  <div>
    <input v-model="expression" readonly />
    <button @click="appendToExpression('sin(')">sin(</button>
    <button @click="appendToExpression('cos(')">cos(</button>
    <button @click="appendToExpression('tan(')">tan(</button>
    <button @click="appendToExpression('log(')">log(</button>
    <button v-for="num in digits" :key="num" @click="appendToExpression(num)">{{ num }}</button>
    <button v-for="op in operators" :key="op" @click="appendToExpression(op)">{{ op }}</button>
    <button @click="processExpression">=</button>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const expression = ref('')
const digits = ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9']
const operators = ['+', '-', '*', '/', ')']

const appendToExpression = (input) => {
  expression.value += input
}


const processExpression = () => {
  let processedExpression = expression.value

  // Ensure multiplication (*) is added before functions if necessary
  processedExpression = processedExpression.replace(/(\d|\))(?=(sin|cos|tan|log)\()/g, '$1*')

  // Count occurrences of sin(, cos(, tan(, log(
  const functionCount = (processedExpression.match(/(sin\(|cos\(|tan\(|log\()/g) || []).length
  // Count existing closing brackets
  const closingCount = (processedExpression.match(/\)/g) || []).length
  // Add missing closing brackets
  const missingBrackets = functionCount - closingCount

  if (missingBrackets > 0) {
    processedExpression += ')'.repeat(missingBrackets)
  }

  // Update the expression with the corrected format
  expression.value = processedExpression
}
</script> -->
