

<template>
  <div class="text-black">
    <h1 class="text-2xl font-bold p-8 bg-slate-300">Loan EMI Calculator</h1>
    <div class="container mx-auto bg-gray-300">
      <div>
        <div class="p-4">
          <label class="text-lg px-8" for="loanAmount">Loan Amount:</label>
          <input class="text-lg pl-4" v-model="sliderValue" type="number" />
        </div>
        <input
          v-model="sliderValue"
          type="range"
          min="0"
          setup="100000"
          max="2000000"
          class="slider w-full"
        />
      </div>
      <div>
        <div class="p-4">
          <label class="text-lg px-8" for="interestRate">Interest Rate:</label>
          <input class="text-lg pl-4" v-model="intRate" type="number" />
        </div>
        <input v-model="intRate" type="range" min="0" max="20" class="slider w-full" />
      </div>
      <div>
        <div class="p-4">
          <label class="text-lg px-8" for="interestRate">Loan Tenure:</label>
          <input class="text-lg pl-4" v-model="LTenure" type="number" />
        </div>
        <input v-model="LTenure" type="range" min="0" max="30" class="slider w-full" />
      </div>
      <div class="flex flex-row">
        <div class="w-1/2">
          <div class="p-4 flex flex-col text-center">
            <label class="text-lg px-8" for="emiResult">EMI:</label>
            <span class="text-lg">{{ calculateEMI.toFixed(0) }}</span>
          </div>
          <div class="p-4 flex flex-col text-center">
            <label class="text-lg px-8" for="totalInterest">Total Interest Payable:</label>
            <span class="text-lg">{{ calculateTotalInterest.toFixed(0) }}</span>
          </div>
          <div class="p-4 flex flex-col text-center">
            <label class="text-lg px-8" for="totalAmount">Total Amount Payable:</label>
            <span class="text-lg">{{ calculateTotalAmount }}</span>
          </div>
        </div>
        <div class="w-1/2">
            <ChartComponent 
              :loanAmount="sliderValue"
              :totalInterestPayable="calculateTotalInterest"
             />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import './assets/base.css'
import ChartComponent from './components/ChartComponent.vue'

import { ref, computed, watch } from 'vue'
const sliderValue = ref(0)
const intRate = ref(0)
const LTenure = ref(0)

const calculateEMI = computed(() => {
  const principal = sliderValue.value
  const rateOfInterest = intRate.value / 100 / 12
  const tenureInMonths = LTenure.value * 12

  // EMI calculation formula
  const emi =
    (principal * rateOfInterest * Math.pow(1 + rateOfInterest, tenureInMonths)) /
    (Math.pow(1 + rateOfInterest, tenureInMonths) - 1)

  return emi
})

const calculateTotalInterest = computed(() => {
  const principal = sliderValue.value
  const emi = calculateEMI.value
  const tenureInMonths = LTenure.value * 12

  // Total Interest Payable calculation formula
  const totalInterest = emi * tenureInMonths - principal

  return totalInterest
})

const calculateTotalAmount = computed(() => {
  const principal = sliderValue.value
  const totalInterest = Math.round(calculateTotalInterest.value)

  // Total Amount Payable calculation
  const totalAmount = Number(principal) + Number(totalInterest)
  return totalAmount
})
</script>

<style scoped></style>
