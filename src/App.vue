<!-- <script setup>
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
    // processedExpression = processedExpression.replace(/(tan|cos|sin)\((\d+)/g, '$&deg')
    processedExpression = processedExpression.replace(/(sin|cos|tan)\((\d+)\)/g, '$1($2deg)')
    console.log(processedExpression)
    outDisplay.value = evaluate(processedExpression)
  } catch {
    outDisplay.value = 'error'
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
      <button @click="clickToDisplay(',')">,</button>
      <button @click="clickToDisplay('/')">÷</button>
      <button class="operator" @click="clickToDisplay('^')">xʸ</button>
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
  min-width: 318px;
  max-width: 318px;
  background-color: #26313e;
}
.input-in,
.input-out {
  border: none;
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
  border: none;
  text-align: right;
  outline: none;
  padding: 5px 15px;
  width: 100%;
  font-size: 35px;
  color: white;
  background-color: #78909c;
  overflow: visible;
}

.advanced-operators {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  background-color: #26313e;
}

.the-keys {
  display: grid;
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
</style> -->

<script setup>
import { ref, nextTick } from 'vue'

const display = ref('') // Store display value
const displayRef = ref(null) // Reference to the display container

const addDigit = (digit) => {
  display.value += digit

  // Wait for Vue to update the DOM, then scroll
  nextTick(() => {
    if (displayRef.value) {
      displayRef.value.scrollLeft = displayRef.value.scrollWidth
    }
  })
}
</script>

<template>
  <div class="calculator">
    <div ref="displayRef" class="display">{{ display }}</div>
    <div class="buttons">
      <button v-for="n in 10" :key="n" @click="addDigit(n - 1)">
        {{ n - 1 }}
      </button>
    </div>
  </div>
</template>

<style scoped>
.calculator {
  width: 200px;
  margin: auto;
  text-align: center;
}

.display {
  width: 100%;
  height: 40px;
  overflow-x: auto;
  white-space: nowrap;
  text-align: right;
  border: 1px solid #000;
  padding: 5px;
  scrollbar-width: none; /* For Firefox */
}
.display::-webkit-scrollbar {
  display: none; /* For Chrome, Safari, Edge */
}

.buttons {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 5px;
  margin-top: 10px;
}

button {
  width: 50px;
  height: 50px;
  font-size: 20px;
}
</style>
