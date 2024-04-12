<script setup>
import HeaderComp from "./components/HeaderComp.vue";
import BalanceComp from "./components/BalanceComp.vue";
import IncomeExpenseve from "./components/IncomeExpenseve.vue";
import TransactionList from "./components/TranactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";
import { ref, computed, onMounted } from "vue";
import { useToast } from "vue-toastification";

onMounted(() => {
  const savedTransaction = JSON.parse(localStorage.getItem("transactions"));
  if (savedTransaction) {
    transactions.value = savedTransaction;
  }
});

const toast = useToast();
const transactions = ref([]);

const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
});

const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0);
});

const expense = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0);
});

//push the transaction
const handleTransaction = (transactionData) => {
  transactions.value.push({
    id: generateId(),
    text: transactionData.text,
    amount: transactionData.amount,
  });
  saveTransactionToLocalstorage()
  toast.success("transaction added");
};

//generate the unique id
const generateId = () => {
  return Math.floor(Math.random() * 1000000);
};

//delete transaction
const handleDelateTrans = (idtrans) => {
  transactions.value = transactions.value.filter(
    (trans) => trans.id !== idtrans
  );
  saveTransactionToLocalstorage();  
  toast.success("transaction deleted successfully");
};

//save to local storage
const saveTransactionToLocalstorage = () => {
  localStorage.setItem("transactions", JSON.stringify(transactions.value));
};
</script>

<template>
  <HeaderComp />
  <div class="container">
    <BalanceComp :total="total" />
    <IncomeExpenseve :income="+income" :expense="+expense" />
    <TransactionList
      @transactionDeleted="handleDelateTrans"
      :transactions="transactions"
    />
    <AddTransaction @transactionSubmited="handleTransaction" />
  </div>
</template>
