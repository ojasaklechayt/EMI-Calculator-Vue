<script setup>
</script>

<template>
    <div class="main">
        <div>
            <form class="Form">
                <input type="number" class="input-field" placeholder="Loan Amount">
                <input type="number" class="input-field" placeholder="Interest Rate (%)">
                <input type="number" class="input-field" placeholder="Loan Term (Months)">
                <div class="choose">
                    <input id="monthly" type="radio" class="input-field-radio" name="timeline">
                    <label for="monthly">Monthly</label>
                    <br>
                    <input id="annually" type="radio" class="input-field-radio" name="timeline">
                    <label for="annually">Annually</label>
                </div>
                <p class="info-text">*1 Year = 12 Months</p>
                <button class="calculate-button">Calculate</button>
            </form>
        </div>
    </div>
</template>


<style scoped>
.Form {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 20px;
}

.choose{
    display: flex;
    flex-direction: row;
    gap: 10px;
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
</style>