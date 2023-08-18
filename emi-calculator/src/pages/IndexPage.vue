<template>
  <q-page class="flex flex-center">
    <div>
      <q-input filled v-model="form.amount" placeholder="Loan Amount" input-class="text-left" />
      <q-input filled v-model="form.interest_rate" placeholder="Interest Rate (%)" input-class="text-left" />
      <q-select v-model="form.display" :options="displayOptions" label="Display"></q-select>
      <q-input filled v-model="form.terms" input-class="text-left" :placeholder="loanTermsPlaceholder" />
      <q-btn type="submit" class="calculate-button" label="Calculate" @click="calculate" />
    </div>
  </q-page>
</template>

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
      table: {
        opening: [],
        emi: [],
        interest_payment: [],
        principal_amount: [],
        closing_balance: [],
      },
      displayOptions: ['Monthly', 'Annually'],
    };
  },
  computed: {
    loanTermsPlaceholder() {
      return this.form.display === 'Monthly' ? 'Loan Terms (No. of Months)' : 'Loan Terms (No. of Years)';
    },
  },
  methods: {
    calculate() {
      this.table.opening = [];
      this.table.emi = [];
      this.table.interest_payment = [];
      this.table.principal_amount = [];
      this.table.closing_balance = [];
      let principal = parseFloat(this.form.amount);
      let interest_rate = parseFloat(this.form.interest_rate);
      let terms = parseInt(this.form.terms);
      let monthly_interest;

      if (this.form.display === 'Monthly') {
        monthly_interest = interest_rate / 12 / 100;
      } else {
        monthly_interest = interest_rate / 100;
      }

      for (let i = 0; i < terms; i++) {
        this.table.opening.push(principal);
        let calc_emi = (principal * monthly_interest * Math.pow(1 + monthly_interest, terms)) /
          (Math.pow(1 + monthly_interest, terms) - 1);
        this.table.emi.push(calc_emi);
        let calc_interest_pay = principal * monthly_interest;
        this.table.interest_payment.push(calc_interest_pay);
        let calc_principal_amount = calc_emi - calc_interest_pay;
        this.table.principal_amount.push(calc_principal_amount);
        let calc_closing_balance = principal - calc_principal_amount;
        this.table.closing_balance.push(calc_closing_balance);
        principal = calc_closing_balance;
      }
      this.$emit('values-calculated', {
        opening: this.table.opening,
        emi: this.table.emi,
        interest_payment: this.table.interest_payment,
        principal_amount: this.table.principal_amount,
        closing_balance: this.table.closing_balance,
        display: this.form.display,
      });
    },
  },
});
</script>
