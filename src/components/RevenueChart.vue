<template>
  <div class="chart-container">
    <Bar :data="chartData" :options="chartOptions" />
  </div>
</template>

<script>
// import { defineComponent } from "vue";
import { Bar } from "vue-chartjs";
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  BarElement,
  CategoryScale,
  LinearScale,
} from "chart.js";

// Register chart.js components
ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale);

export default ({
  name: "RevenueChart",
  components: {Bar,
  },
  props: {
    salesData: {
      type: Array,
      required: true,
    },
    expenseData: {
      type: Array,
      required: true,
    },
  },
  computed: {
    chartData() {
      return {
        labels: ["Januari", "Februari", "Maret", "April"], // Sesuaikan label dengan data
        datasets: [
          {
            label: "Sales",
            backgroundColor: "#4caf50",
            data: this.salesData,
          },
          {
            label: "Expenses",
            backgroundColor: "#f44336",
            data: this.expenseData,
          },
        ],
      };
    },
    chartOptions() {
      return {
        responsive: true,
        plugins: {
          legend: {
            display: true,
            position: "top",
          },
        },
      };
    },
  },
});



</script>


<style scoped>
.chart-container {
  max-width: 800px;
  margin: 0 auto;
}
</style>
