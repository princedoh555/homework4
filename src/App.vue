<script setup>
import Header from './components/Header.vue';
import Balance from './components/Header.vue';
import IncomeExpense from './components/IncomeExpense.vue';
import AddTranscation from './components/AddTranscation.vue';
import TransactionList from './components/TransactionList.vue';
import {ref, computed, onMounted} from 'vue'

const transactions = ref([])

const sum = computed(()=>{
  return transactions.value.reduce((acc, x)=>{
    return acc+x.amount
  },0)
})

const moneyIn = computed(()=>{
  return transactions.value
  .filter((x)=>x.amount>0)
  .reduce((acc, x)=>{
    return acc+x.amount
  },0)
})


const moneyOut = computed(()=>{
  return transactions.value
  .filter((x)=>x.amount<0)
  .reduce((acc, x)=>{
    return acc+x.amount
  },0)
})

const handleTransaction =(transactions) => {
  transactions.value.push({
    text: transactionData.text,
    amount: transactionData.amount,
  })
  saveToLocalStorage()
}

const generateID = () =>{
  return Math.floor(Math.random()*10000000)
}

const handleDeleted = (id) => {
  transactions.value = transactions.value.filter((x) => x.id !== id)
  saveToLocalStorage()
}

const saveToLocalStorage =() => {
  localStorage.setItem('transaction', JSON.stringify(transactions.value))
}

onMounted( () => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions')) 

  if(savedTransactions){
    transactions.value = savedTransactions
  }
})


</script>

<template>
<Header></Header>
<div class="container">
  <Balance :total="sum"></Balance>
  <IncomeExpense :income="moneyIn" :expense="moneyOut"></IncomeExpense>
  <AddTranscation @transactionSubmitted="handleTransaction"></AddTranscation>
  <TransactionList> :transactions="transactions" @transactionDeleted="handleDeleted"</TransactionList>
</div>

</template>