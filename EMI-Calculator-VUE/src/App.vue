<script>
import Input from './components/Input.vue'
import Displaytable from './components/Displaytable.vue'

export default {
  name: "App",
  data() {
    return {
      table: {
        opening: [],
        emi: [],
        interest_payment: [],
        principal_amount: [],
        closing_balance: [],
        display: "Null"
      }
    }
  },
  components: {
    Input, Displaytable,
  },
  methods: {
    GetData(data) {
      this.table = data;
    }
  },
}

</script>

<template>
  <header>
    <h1>EMI Calculator</h1>
    <div class="wrapper">
      <Input class="input" @values-calculated="GetData($event)" />
    </div>
  </header>

  <main>
    <Displaytable :tableData="table"/>
  </main>
</template>

<style scoped>
header {
  flex: flex;
  place-items: center;
  padding-right: calc(var(--section-gap) / 2);
  flex-direction: column;
  justify-content: center;
  gap: 30px;
  margin-top: 40px;
}

header h1 {
  color: white;
  font-size: 2rem;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

.input {
  background-color: white;
  padding: 10%;
  border-radius: 15px;
  width: 350px;
}

main {
  flex: flex;
  align-items: center;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
