<template>
  <q-page class="emi-calculator">
    <div class="calculator-container">
      <h2 class="title">Loan Calculator</h2>
      <q-input class="input-ok" filled v-model="form.amount" placeholder="Loan Amount" input-class="text-left" />
      <br>
      <q-input class="input-ok" filled v-model="form.interest_rate" placeholder="Interest Rate (%)"
        input-class="text-left" />
      <br>
      <q-select class="input-select" v-model="form.display" :options="displayOptions" label="Display"
        use-input></q-select>
      <br>
      <q-input class="input-ok" filled v-model="form.terms" input-class="text-left" :placeholder="loanTermsPlaceholder" />
      <br>
      <q-btn type="submit" class="calculate-button" label="Calculate" @click="calculate" />
    </div>
    <div class="table-chart-container">
      <div class="table-container" v-if="showTable">
        <q-table class="custom-table" :rows="tableData.map((row, index) => ({
          index: index + 1,
          opening: row.opening.toFixed(2),
          emi: row.emi.toFixed(2),
          interest_payment: row.interest_payment.toFixed(2),
          principal_amount: row.principal_amount.toFixed(2),
          closing_balance: row.closing_balance.toFixed(2),
        }))" :columns="[
  {
    label: displayLabel,
    field: 'index',
    name: 'index'
  },
  {
    label: 'Opening Balance',
    field: 'opening'
  },
  {
    label: 'EMI',
    field: 'emi'
  },
  {
    label: 'Interest Payment',
    field: 'interest_payment'
  },
  {
    label: 'Principal Amount',
    field: 'principal_amount'
  },
  {
    label: 'Closing Balance',
    field: 'closing_balance'
  }
]" />
      </div>
      <Bar id="my-chart-id" :options="chartOptions" :data="chartData" v-if="showTable" />
    </div>
  </q-page>
</template>

<style>
.emi-calculator {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
}

.calculator-container {
  width: 500px;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 10px;
  background-color: #fff;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
  text-align: center;
  /* Align content to the center */
}

.text-left {
  text-align: left;
}

.input-select {
  background-color: rgb(238, 238, 238);
  padding-left: 2%;
}

.calculate-button {
  margin-top: 20px;
  width: 100%;
  background-color: rgb(243, 243, 243);
}

.title {
  font-size: 24px;
  margin-bottom: 10px;
}

.table-chart-container {
  display: flex;
  flex-direction: column;
  align-items: start;
  margin-top: 20px;
  margin-right: 100px;
}

.table-container {
  margin-top: 20px;
  margin-bottom: 20px;
}

.custom-table {
  width: 100%;
  border-collapse: collapse;
}

.custom-table th,
.custom-table td {
  padding: 8px;
  border: 1px solid #ccc;
  text-align: center;
}

.custom-table th {
  background-color: #f2f2f2;
}

.custom-table tbody tr:nth-child(even) {
  background-color: #f2f2f2;
}
</style>

<script>
import { defineComponent } from 'vue';
import { Bar } from 'vue-chartjs'
import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js'

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale)

export default defineComponent({
  name: 'IndexPage',
  components: {
    Bar,
  },
  data() {
    return {
      form: {
        amount: null,
        interest_rate: null,
        display: 'Monthly',
        terms: null,
      },
      tableData: [],
      displayOptions: ['Monthly', 'Annually'],
      showTable: false,
      chartData: {
        labels: [],
        datasets: [
          {
            label: 'Opening Balance',
            backgroundColor: 'rgba(75,192,192,0.2)',
            borderColor: 'rgba(75,192,192,1)',
            borderWidth: 1,
            hoverBackgroundColor: 'rgba(75,192,192,0.4)',
            hoverBorderColor: 'rgba(75,192,192,1)',
            data: [],
          },
          {
            label: 'EMI',
            backgroundColor: 'rgba(255,0,0,0.2)',
            borderColor: 'rgba(255,0,0,1)',
            borderWidth: 1,
            hoverBackgroundColor: 'rgba(255,0,0,0.4)',
            hoverBorderColor: 'rgba(255,0,0,1)',
            data: [],
          },
          {
            label: 'Interest Payment',
            backgroundColor: 'rgba(0,255,0,0.2)',
            borderColor: 'rgba(0,255,0,1)',
            borderWidth: 1,
            hoverBackgroundColor: 'rgba(0,255,0,0.4)',
            hoverBorderColor: 'rgba(0,255,0,1)',
            data: [],
          },
          {
            label: 'Principal Amount',
            backgroundColor: 'rgba(255,255,0,0.2)',
            borderColor: 'rgba(255,255,0,1)',
            borderWidth: 1,
            hoverBackgroundColor: 'rgba(255,255,0,0.4)',
            hoverBorderColor: 'rgba(255,255,0,1)',
            data: [],
          },
          {
            label: 'Closing Balance',
            backgroundColor: 'rgba(0,0,255,0.2)',
            borderColor: 'rgba(0,0,255,1)',
            borderWidth: 1,
            hoverBackgroundColor: 'rgba(0,0,255,0.4)',
            hoverBorderColor: 'rgba(0,0,255,1)',
            data: [],
          },
        ],
      },
      chartOptions: {
        responsive: true,
        scales: {
          x: {
            stacked: true,
          },
          y: {
            stacked: true,
          },
        },
      },
    };
  },
  computed: {
    displayLabel() {
      return this.form.display === 'Monthly' ? 'Months' : 'Years';
    },
    loanTermsPlaceholder() {
      return this.form.display === 'Monthly' ? 'Loan Terms (No. of Months)' : 'Loan Terms (No. of Years)';
    },
  },
  methods: {
    calculate() {
      this.tableData = [];

      let principal = parseFloat(this.form.amount);
      const interest_rate = parseFloat(this.form.interest_rate);
      const terms = parseInt(this.form.terms);
      const monthly_interest = this.form.display === 'Monthly' ? interest_rate / 12 / 100 : interest_rate / 100;

      for (let i = 0; i < terms; i++) {
        const opening = principal;
        const calc_emi = (principal * monthly_interest * Math.pow(1 + monthly_interest, terms)) /
          (Math.pow(1 + monthly_interest, terms) - 1);
        const calc_interest_pay = principal * monthly_interest;
        const calc_principal_amount = calc_emi - calc_interest_pay;
        const calc_closing_balance = principal - calc_principal_amount;

        this.tableData.push({
          opening,
          emi: calc_emi,
          interest_payment: calc_interest_pay,
          principal_amount: calc_principal_amount,
          closing_balance: calc_closing_balance,
        });

        principal = calc_closing_balance;
        this.chartData.labels.push(this.displayLabel === 'Months' ? `Month ${i + 1}` : `Year ${i + 1}`);
        this.chartData.datasets[0].data.push(opening);
        this.chartData.datasets[1].data.push(calc_emi);
        this.chartData.datasets[2].data.push(calc_interest_pay);
        this.chartData.datasets[3].data.push(calc_principal_amount);
        this.chartData.datasets[4].data.push(calc_closing_balance);
      }

      this.showTable = true;
    },

    formatValue(value) {
      return value ? value.toFixed(2) : '';
    },
    displayValue(index) {
      return index + 1;
    },
  },
});
</script>
