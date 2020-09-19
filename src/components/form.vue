<template>
  <div>
    <input type="text" placeholder="Название операции" v-model="operationName">
    <input type="number" placeholder="Сумма операции" v-model="operationSum">
    <button @click="setBudget()">Установить бюджет</button>
    <button @click="addMoney()">Добавить доход</button>
    <button @click="addPayment()">Добавить расход</button>
  </div>
</template>

<script>
export default {
  name: "formBlock",
  data(){
    return {
      operationName: '',
      operationSum: null
    }
  },
  methods: {
    addMoney: function () {
      this.$emit('addMoney', {operationName:this.operationName,operationSum: this.operationSum})
      this.operationName=''
      this.operationSum = 0
    },
    setBudget() {
      this.$emit('setBudget', this.operationSum)
      this.operationName=''
      this.operationSum = 0
    },
    addPayment() {
      this.$emit('addPayment', {operationName:this.operationName,operationSum: this.operationSum})
      this.operationName=''
      this.operationSum = 0
    }
  }
}
</script>

<style scoped>
  div{
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
  }
  input, button{
    margin: 5px 0;
    padding: 10px 20px;
    border: 1px solid blue;
  }
</style>
