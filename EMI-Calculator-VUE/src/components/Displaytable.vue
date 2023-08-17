<script>
export default {
    data() {
        return {
            tableData: [
                { monthYear: 'Jan 2023', openingAmount: 1000, emivalue:20, interestAmount: 50, principleamount:500, totalAmount: 1050 },
                { monthYear: 'Feb 2023', openingAmount: 950, emivalue:20, interestAmount: 47.5, principleamount:500, totalAmount: 997.5 },
            ]
        }
    }
}

</script>

<template>
    <div class="results" v-if="table.opening.length > 0">
            <table>
                <thead>
                    <tr>
                        <th>Month</th>
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
</template>

<style scoped>
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
