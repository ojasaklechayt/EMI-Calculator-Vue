<template>
  <q-page class="flex flex-center">
    <div class="calculator-container">
      <h2 class="title">Loan Calculator</h2>
      <q-input class="input-ok" filled v-model="form.amount" placeholder="Loan Amount" input-class="text-left" />
      <br>
      <q-input class="input-ok" filled v-model="form.interest_rate" placeholder="Interest Rate (%)" input-class="text-left" />
      <br>
      <q-select class="input-select" v-model="form.display" :options="displayOptions" label="Display"></q-select>
      <br>
      <q-input class="input-ok" filled v-model="form.terms" input-class="text-left" :placeholder="loanTermsPlaceholder" />
      <br>
      <q-btn type="submit" class="calculate-button" label="Calculate" @click="calculate" />
    </div>

    <div class="table-container" v-if="showTable">
      <table class="custom-table">
        <thead>
          <tr>
            <th>{{ displayLabel }}</th>
            <th>Opening Balance</th>
            <th>EMI</th>
            <th>Interest Payment</th>
            <th>Principal Amount</th>
            <th>Closing Balance</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(item, index) in tableData.opening" :key="index">
            <td>{{ displayValue(index) }}</td>
            <td>{{ formatValue(item) }}</td>
            <td>{{ formatValue(tableData.emi[index]) }}</td>
            <td>{{ formatValue(tableData.interest_payment[index]) }}</td>
            <td>{{ formatValue(tableData.principal_amount[index]) }}</td>
            <td>{{ formatValue(tableData.closing_balance[index]) }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </q-page>
</template>

<style>
.calculator-container {
  width: 500px;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 10px;
  background-color: #fff;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
}

.text-left {
  text-align: left;
}

.input-select{
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
  text-align: center;
}

.table-container {
  margin-top: 20px;
  margin-right: 100px;
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

export default defineComponent({
  name: 'IndexPage',
  data() {
    return {
      form: {
        amount: null,
        interest_rate: null,
        display: 'Monthly',
        terms: null,
      },
      tableData: {
        opening: [],
        emi: [],
        interest_payment: [],
        principal_amount: [],
        closing_balance: [],
      },
      displayOptions: ['Monthly', 'Annually'],
      showTable: false,
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
      Object.keys(this.tableData).forEach(key => this.tableData[key] = []);

      let principal = parseFloat(this.form.amount);
      const interest_rate = parseFloat(this.form.interest_rate);
      const terms = parseInt(this.form.terms);
      const monthly_interest = this.form.display === 'Monthly' ? interest_rate / 12 / 100 : interest_rate / 100;

      for (let i = 0; i < terms; i++) {
        this.tableData.opening.push(principal);
        const calc_emi = (principal * monthly_interest * Math.pow(1 + monthly_interest, terms)) /
          (Math.pow(1 + monthly_interest, terms) - 1);
        this.tableData.emi.push(calc_emi);
        const calc_interest_pay = principal * monthly_interest;
        this.tableData.interest_payment.push(calc_interest_pay);
        const calc_principal_amount = calc_emi - calc_interest_pay;
        this.tableData.principal_amount.push(calc_principal_amount);
        const calc_closing_balance = principal - calc_principal_amount;
        this.tableData.closing_balance.push(calc_closing_balance);
        principal = calc_closing_balance;
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
