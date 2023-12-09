<template>
  <h1>Break-up of Total Payment</h1>
  <div>
    <chart :data="chartData" :options="chartOptions" />
  </div>
</template>

<script setup>
import { ref, watch } from 'vue';
import { Pie } from 'vue-chartjs';

const loanAmount = ref(0);
const totalInterestPayable = ref(0);

const chartData = ref({
  labels: ['Loan Amount', 'Total Interest Payable'],
  datasets: [
    {
      data: [loanAmount.value, totalInterestPayable.value],
      backgroundColor: ['#36A2EB', '#FF6384'],
    },
  ],
});

const chartOptions = ref({
  responsive: true,
  maintainAspectRatio: false,
});

// Watch for changes in props and update the chart accordingly
watch([loanAmount, totalInterestPayable], ([newLoanAmount, newTotalInterest]) => {
  chartData.value.datasets[0].data = [newLoanAmount, newTotalInterest];
  updateChart();
});

const updateChart = () => {
  if (chartData.value._chart) {
    chartData.value._chart.update();
  }
};
</script>

<style scoped>
/* Add any custom styles here */
</style>