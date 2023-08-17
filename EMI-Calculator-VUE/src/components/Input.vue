<template>
    <div class="main">
        <div>
            <form class="Form" @submit.prevent="calculate">
                <input v-model="form.amount" type="number" class="input-field" placeholder="Loan Amount">
                <input v-model="form.interest_rate" type="number" class="input-field" placeholder="Interest Rate (%)">
                <input v-model="form.months" type="number" class="input-field" placeholder="Loan Term (Months)">
                <p>Choose Timeline</p>
                <select v-model="form.display" class="choose">
                    <option value="monthly">Display Monthly</option>
                    <option value="annually">Display Annually</option>
                </select>
                <p class="info-text">*1 Year = 12 Months</p>
                <button type="submit" class="calculate-button">Calculate</button>
            </form>
        </div>
        <div class="results" v-if="table.opening.length > 0">
            <table>
                <thead>
                    <tr>
                        <th>{{form.display === "monthly" ? "Month" : "Year"}}</th>
                        <th>Opening Balance</th>
                        <th>EMI</th>
                        <th>Interest Payment</th>
                        <th>Principal Amount</th>
                        <th>Closing Balance</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(month, index) in table.opening.length" :key="index">
                        <td>{{ index + 1 }}</td>
                        <td>{{ table.opening[index].toFixed(2) }}</td>
                        <td>{{ table.emi[index].toFixed(2) }}</td>
                        <td>{{ table.interest_payment[index].toFixed(2) }}</td>
                        <td>{{ table.principal_amount[index].toFixed(2) }}</td>
                        <td>{{ table.closing_balance[index].toFixed(2) }}</td>
                    </tr>
                </tbody>
            </table>
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
    methods: {
        calculate() {
            this.table.opening = [];
            this.table.emi = [];
            this.table.interest_payment = [];
            this.table.principal_amount = [];
            this.table.closing_balance = [];

            let principal = parseFloat(this.form.amount);
            let interest_rate = parseFloat(this.form.interest_rate);
            let months = parseFloat(this.form.months);
            let monthly_interest = interest_rate / 12 / 100;

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