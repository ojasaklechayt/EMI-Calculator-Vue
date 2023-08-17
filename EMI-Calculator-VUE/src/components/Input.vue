<script>
export default {
    data() {
        return {
            form: {
                amount: null,
                interest_rate: null,
                months: null,
                display: 'monthly'
            }
        }
    },
    methods: {
        async submit() {
            console.log(this.form.amount);
            console.log(this.form.interest_rate);
            console.log(this.form.months);
            console.log(this.form.display);
        }
    },
    watch: {

    }
}
</script>

<template>
    <div class="main">
        <div>
            <form class="Form" @submit.prevent="submit">
                <input id="amount" v-model="form.amount" type="number" class="input-field" placeholder="Loan Amount">
                <input id="rate" v-model="form.interest_rate" type="number" class="input-field" placeholder="Interest Rate (%)">
                <input id="term" v-model="form.months" type="number" class="input-field" placeholder="Loan Term (Months)">
                <p>Choose Timeline</p>
                <select v-model="form.display" class="choose">
                    <option value="display-monthly">Display Monthly</option>
                    <option value="display-annually">Display Annually</option>
                </select>
                <p class="info-text">*1 Year = 12 Months</p>
                <button type="submit" class="calculate-button">Calculate</button>
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
</style>