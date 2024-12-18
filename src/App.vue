<template>
  <div id="app">
    <header class="header">
      <h1>Financial Dashboard</h1>
    </header>
    <main class="main-content">
      <section class="statistics">
        <div class="card">
          <h2>Total Sales</h2>
          <p>{{ formatCurrency(totalSales) }}</p>
        </div>
        <div class="card">
          <h2>Total Expenses</h2>
          <p>{{ formatCurrency(totalExpenses) }}</p>
        </div>
        <div class="card">
          <h2>Net Profit</h2>
          <p :class="{ positive: netProfit >= 0, negative: netProfit < 0 }">
            {{ formatCurrency(netProfit) }}
          </p>
        </div>
      </section>
      <section class="chart">
        <RevenueChart :sales-data="salesData" :expense-data="expenseData" />
      </section>
    </main>
  </div>
</template>

<script>
import { transactions } from "./data/data";
import RevenueChart from "./components/RevenueChart";

export default {
  components: {
    RevenueChart,
  },
  data() {
    return {
      transactions,
    };
  },
  computed: {
    totalSales() {
      return this.transactions
        .filter((t) => t.type === "sale")
        .reduce((sum, t) => sum + t.amount, 0);
    },
    totalExpenses() {
      return this.transactions
        .filter((t) => t.type === "expense")
        .reduce((sum, t) => sum + t.amount, 0);
    },
    netProfit() {
      return this.totalSales - this.totalExpenses;
    },
    salesData() {
      return this.transactions
        .filter((t) => t.type === "sale")
        .map((t) => t.amount);
    },
    expenseData() {
      return this.transactions
        .filter((t) => t.type === "expense")
        .map((t) => t.amount);
    },
  },
  methods: {
    formatCurrency(value) {
      return new Intl.NumberFormat("id-ID", {
        style: "currency",
        currency: "IDR",
      }).format(value);
    },
  },
};
</script>

<style>
/* General Styles */
body {
  margin: 0;
  font-family: "Arial", sans-serif;
  background-color: #332b2b;
  color: #333;
}

#app {
  text-align: center;
}

/* Header */
.header {
  background-color: #007bff;
  color: white;
  padding: 20px;
  box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1);
}

.header h1 {
  margin: 0;
  font-size: 2rem;
}

/* Main Content */
.main-content {
  max-width: 1200px;
  margin: 20px auto;
  padding: 20px;
}

/* Statistics Section */
.statistics {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  justify-content: space-around;
  margin-bottom: 40px;
}

.card {
  background: rgb(121, 108, 108);
  box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  padding: 20px;
  width: 300px;
  text-align: center;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.card:hover {
  transform: translateY(-5px);
  box-shadow: 0px 8px 12px rgba(0, 0, 0, 0.2);
}

.card h2 {
  font-size: 1.5rem;
  margin-bottom: 10px;
  color: rgb(255, 253, 253);
}

.card p {
  font-size: 2rem;
  font-weight: bold;
  margin: 0;
}

.card p.positive {
  color: #28a745; /* Green for positive profit */
}

.card p.negative {
  color: #dc3545; /* Red for negative profit */
}

/* Chart Section */
.chart {
  background: rgb(24, 1, 1);
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1);
}

.chart canvas {
  max-width: 100%;
  height: auto;
}

/* Responsive Styles */
@media (max-width: 768px) {
  .statistics {
    flex-direction: column;
    align-items: center;
  }

  .card {
    width: 90%;
  }
}
</style>
