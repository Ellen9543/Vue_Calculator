<script setup>
import { ref } from "vue";


let showVal = ref('0')
let calcArr = [0]
let isComplete = false

const calc = (e) => {
  const ele = e.target
  if (ele.classList.contains('display')) {
    return
  }

  let len
  const val = String(ele.innerHTML)
  
  switch(val) {
    case 'AC':
      showVal.value = '0'
      calcArr = [0]
      break
      
    case '.':
      showVal.value = (showVal.value.includes(val)) ? showVal.value : showVal.value + val
      break
    
    case '=':
      if (calcArr.length == 2) {
        calcArr[2] = calcArr[0]
      }

      if (calcArr.length > 2) {
        calcArr = calculator(calcArr)
        isComplete = true
      }
      break

    case '+/-':
      len = calcArr.length-1
      if (!isFunKey(calcArr[len])) {
        calcArr[len] = (-Number(calcArr[len])).toString()
        showVal.value = calcArr[len]
      }
      
      break

    case '%':
      len = calcArr.length-1
      if (!isFunKey(calcArr[len])) {
        calcArr[len] = (Number(calcArr[len])/100).toString()
        showVal.value = calcArr[len]
      }
      break


    default:
      if (isFunKey(val)) {
        if (calcArr.length == 1) {
          calcArr.push(val)
        }
        else if (calcArr.length == 2) {
          isComplete = false
          calcArr[1] = val
        }
        else {
          calcArr = calculator(calcArr)
          calcArr.push(val)
        }
        
      }
      else {
        
        if (calcArr.length == 2) {
          showVal.value = '0'
        }

        showVal.value = (showVal.value === '0' || isComplete) ? val : showVal.value + val
        isComplete = false
        if (calcArr.length == 1) {
          calcArr[0] = showVal.value
        }
        else {
          calcArr[2] = showVal.value
        }

      }
      
      //console.log(showVal.value, calcArr)
      break
  }
  
}

const isFunKey = (key) => {
  const keyarr = ['+/-', '+', '–', '=', '×', '÷', '%']
  return keyarr.includes(key)
}

const calculator = calcArr => {
  let operator, sum = 0;

  switch(calcArr[1]) {
    case '×':
      operator = '*'
      break
    case '÷':
      operator = '/'
      break
    case '–':
      operator = '-'
      break
    default:
      operator = calcArr[1] 
      break

  }

  sum = eval(`${calcArr[0]} ${operator} ${calcArr[2]}`).toString()
  calcArr = [sum]
  showVal.value = sum

  return calcArr
}
</script>

<template>
  <main>
    <div class="calc" @click="calc">
      <div class="display display1">{{ showVal }}</div>

      <div class="row">
        <button class="special-key">AC</button>
        <button class="special-key">+/-</button>
        <button class="special-key">&#37;</button>
        <button class="operator-key">&#247;</button>
      </div>

      <div class="row">
        <button class="number-key">7</button>
        <button class="number-key">8</button>
        <button class="number-key">9</button>
        <button class="operator-key">&#215;</button>
      </div>

      <div class="row">
        <button class="number-key">4</button>
        <button class="number-key">5</button>
        <button class="number-key">6</button>
        <button class="operator-key">–</button>
      </div>

      <div class="row">
        <button class="number-key">1</button>
        <button class="number-key">2</button>
        <button class="number-key">3</button>
        <button class="operator-key">+</button>
      </div>

      <div class="row">
        <button class="number-key double-size">0</button>
        <button class="number-key">.</button>
        <button class="operator-key">=</button>
      </div>
    </div>
  </main>
</template>

<style scoped>

</style>
