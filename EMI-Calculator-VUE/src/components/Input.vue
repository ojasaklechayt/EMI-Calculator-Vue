<template>
    <div class="main">
        <div>
            <form class="Form" @submit.prevent="calculate">
                <input v-model="form.amount" class="input-field" placeholder="Loan Amount">
                <input v-model="form.interest_rate" class="input-field" placeholder="Interest Rate (%)">
                <select v-model="form.display" class="choose">
                    <option value="monthly">Monthly Installment</option>
                    <option value="annually">Annual Installment</option>
                </select>
                <input v-model="form.months" class="input-field" :placeholder="loantermsplaceholder">
                <button type="submit" class="calculate-button">Calculate</button>
            </form>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            form: {
                amount: null,
                interest_rate: null,
                months: null,
                display: 'monthly'
            },
            table: {
                opening: [],
                emi: [],
                interest_payment: [],
                principal_amount: [],
                closing_balance: []
            }
        }
    },
    computed: {
        loantermsplaceholder() {
            return this.form.display === 'monthly' ? 'Loan Terms (No. of Months)' : 'Loan Terms (No. of Years)';
        }
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
            let months;
            let monthly_interest;
            months = parseInt(this.form.months);
            if(this.form.display === "monthly") {
                monthly_interest = interest_rate / 12 / 100;
            } else {
                monthly_interest = interest_rate / 100;
            }

            for (let i = 0; i < months; i++) {
                this.table.opening.push(principal);
                let calc_emi = (principal * monthly_interest * Math.pow(1 + monthly_interest, months)) /
                    (Math.pow(1 + monthly_interest, months) - 1);
                this.table.emi.push(calc_emi);
                let calc_interest_pay = principal * monthly_interest;
                this.table.interest_payment.push(calc_interest_pay);
                let calc_principal_amount = calc_emi - calc_interest_pay;
                this.table.principal_amount.push(calc_principal_amount);
                let calc_closing_balance = principal - calc_principal_amount;
                this.table.closing_balance.push(calc_closing_balance);
                principal = calc_closing_balance;
            }
            this.$emit('values-calculated',{
                opening: this.table.opening,
                emi: this.table.emi,
                interest_payment: this.table.interest_payment,
                principal_amount: this.table.principal_amount,
                closing_balance: this.table.closing_balance,
                display: this.form.display
            });
        }
    }
}
</script>


<style>
.Form {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 20px;
}

.choose {
    display: flex;
    flex-direction: row;
    gap: 5px;
    border: 2px solid black;
    border-radius: 5px;
    height:30px;
    width: 300px;
}

.info-text {
    font: white;
}

.input-field {
    border: 2px solid black;
    border-radius: 5px;
    height: 30px;
    width: 300px;
    padding: 5px;
}

.calculate-button {
    border: 2px solid black;
    border-radius: 5px;
    padding: 3px;
    background-color: #323232;
    color: aliceblue;
}

.calculate-button:hover {
    background-color: cadetblue;
}

.table-container {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 40px;
    padding-right: 70px;
}

.custom-table {
    width: 80%;
    border-collapse: collapse;
    border: 1px solid #ccc;
    background-color: #f0f0f0;
}

.custom-table th,
.custom-table td {
    padding: 10px;
    text-align: center;
    border: 1px solid #ccc;
}

.custom-table th {
    background-color: #333;
    color: white;
}

.custom-table tr:nth-child(even) {
    background-color: #e0e0e0;
}

.custom-table tr:hover {
    background-color: #d0d0d0;
}
</style>