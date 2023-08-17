<script>
export default {
    data() {
        return {
            tableData: [
                { monthYear: 'Jan 2023', principalAmount: 1000, interestAmount: 50, totalAmount: 1050 },
                { monthYear: 'Feb 2023', principalAmount: 950, interestAmount: 47.5, totalAmount: 997.5 },
            ]
        }
    }
}

</script>

<template>
    <div class="table-container">
        <table class="custom-table">
            <thead>
                <tr>
                    <th>Months/Years</th>
                    <th>Principal Amount</th>
                    <th>Interest Amount</th>
                    <th>Total Amount</th>
                </tr>
            </thead>
            <tbody>
                <!-- Use v-for to loop through your data and populate the table rows -->
                <tr v-for="(row, index) in tableData" :key="index">
                    <td>{{ row.monthYear }}</td>
                    <td>{{ row.principalAmount }}</td>
                    <td>{{ row.interestAmount }}</td>
                    <td>{{ row.totalAmount }}</td>
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
