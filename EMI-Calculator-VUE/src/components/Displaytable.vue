<script>
export default {
    props: {
        tableData: {
            type: Array,
            default: () => ({
                opening: [],
                emi: [],
                interest_payment: [],
                principal_amount: [],
                closing_balance: [],
                display: "Null"
            })
        },
    },
};

</script>

<template>
    <div class="table-container" v-if="tableData.opening.length > 0">
        <table class="custom-table">
            <thead>
                <tr>
                    <th>{{ tableData.display === "monthly" ? "Months" : "Years" }}</th>
                    <th>Opening Balance</th>
                    <th>EMI</th>
                    <th>Interest Payment</th>
                    <th>Principal Amount</th>
                    <th>Closing Balance</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(item, index) in tableData.opening" :key="index">
                    <td>{{ index + 1 }}</td>
                    <td>{{ item ? item.toFixed(2) : '' }}</td>
                    <td>{{ tableData.emi[index] ? tableData.emi[index].toFixed(2) : '' }}</td>
                    <td>{{ tableData.interest_payment[index] ? tableData.interest_payment[index].toFixed(2) : '' }}</td>
                    <td>{{ tableData.principal_amount[index] ? tableData.principal_amount[index].toFixed(2) : '' }}</td>
                    <td>{{ tableData.closing_balance[index] ? tableData.closing_balance[index].toFixed(2) : '' }}</td>
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
    margin: 40px 0;
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
