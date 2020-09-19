<template>
  <div id="app">
    <formBlock v-on:addMoney="addMoney" v-on:setBudget="setBudget" v-on:addPayment="addPayment"></formBlock>
    <div class="wrapper">
      <h1>Бюджет {{budget}}</h1>
      <h2>Остаток {{excess}}</h2>
      <h1 v-if="excess<=0">Израсходован весь бюджет!</h1>
      <ul>
        <li v-for="item in allPayments" :key="item.id" :class="item.type">
          <span>{{ item.name }}</span>
          <span>{{ item.sum }}</span>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import formBlock from "@/components/form";

export default {
  name: "App",
  data(){
    return{
      allPayments:[],
      budget:0,
      excess:0,
      paymentTemplate:{type:'',name:'',sum:''}
    }
  },
  components: {
    formBlock
  },
  methods:{
    addMoney(data){
      if(data.operationName && data.operationSum){
        const item = JSON.parse(JSON.stringify(this.paymentTemplate))
        item.type = 'add';
        item.name = data.operationName;
        item.sum = data.operationSum;
        this.allPayments.push(item);
        console.log(this.allPayments)
        this.budget += +data.operationSum;
        this.excess += +data.operationSum
      }
    },
    setBudget(operationSum){
      if (operationSum){
        this.budget = +operationSum;
        this.excess = this.budget;
        this.allPayments=[]
      }
    },
    addPayment(data){
      if(data.operationName && data.operationSum){
        const item = JSON.parse(JSON.stringify(this.paymentTemplate))
        item.type = 'pay';
        item.name = data.operationName;
        item.sum = data.operationSum;
        this.allPayments.push(item);
        this.excess = this.budget - data.operationSum;

      }
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}
.wrapper{
  width: 70%;
}
ul{
  width: 50%;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: space-between;
}
ul li{
  width: 200px;
  height: 100px;
  padding: 20px 0;
  margin-bottom: 10px;
  list-style: none;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
  background-color: #fafafa;
  font-size: 18px;
  color: #000;
  border-radius: 5px;
  border: 1px solid black;
}
.add{
  background-color: #c3fca0;
}
.pay{
  background-color: #ff9d9d;
}

</style>
