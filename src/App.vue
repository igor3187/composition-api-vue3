<script setup>
import Header from "@/components/Header.vue";
import Balance from "@/components/Balance.vue";
import IncomeExpanses from "@/components/IncomeExpanses.vue";
import TransactionList from "@/components/TransactionList.vue";
import AddTransaction from "@/components/AddTransaction.vue";

import {useToast} from "vue-toastification";

import {ref, computed, onMounted} from "vue";

onMounted(() => {
    const savedTransactions = JSON.parse(localStorage.getItem("transactions"));

    if (savedTransactions) {
        transactions.value = savedTransactions;
    }
});

const toast = useToast();

const transactions = ref([
    // { id: 1, text: 'Flower', amount: '-19.99'},
    // { id: 2, text: 'Salary', amount: '299.97'},
    // { id: 3, text: 'Book', amount: '-10'},
    // { id: 4, text: 'Camera', amount: '150'},
]);
// Get total
const total = computed(() => transactions.value.reduce((acc, transaction) => acc + Number(transaction.amount), 0));
//Get income
const income = computed(() => transactions.value.filter((transaction) => transaction.amount > 0).reduce((acc, transaction) => acc + Number(transaction.amount), 0).toFixed(2));
//Get expenses
const expenses = computed(() => transactions.value.filter((transaction) => transaction.amount < 0).reduce((acc, transaction) => acc + Number(transaction.amount), 0).toFixed(2));
//addTransaction
const handleTransactionSubmitted = (transactionData) => {
    try {
        transactions.value.push({
            id: generateUniqueId(),
            text: transactionData.text,
            amount: transactionData.amount,
        });

        addTransactionToLocalStorage();
        toast.success('Transaction Submitted successfully.');
    } catch(error) {
        toast.error('Something went wrong', console.log(error));
    }
};
// Generate unique ID
const generateUniqueId = () => {
    return Math.floor(Math.random() * 1000000);
};
// Add Transaction
const addTransactionToLocalStorage = () => localStorage.setItem("transactions", JSON.stringify(transactions.value));
// Delete Transaction
const handleTransactionDelete = (id) => {
    try {
        transactions.value = transactions.value.filter((transaction) => transaction.id !== id);
        addTransactionToLocalStorage();
        toast.success('Transaction deleted.');
    } catch(error) {
        toast.error('Something went wrong', console.log(error));
    }
};
</script>

<template>
    <Header />
    <div class="container">
        <Balance :total="total" />
        <IncomeExpanses :income="parseFloat(income)" :expenses="parseFloat(expenses)" />
        <TransactionList :transactions="transactions" @deleteTransaction="handleTransactionDelete"/>
        <AddTransaction @transaction-submitted="handleTransactionSubmitted" />
    </div>
</template>

<style scoped>

</style>
