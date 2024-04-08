<script setup>
const emit = defineEmits(['deleteTransaction']);

defineProps({
    transactions: {
        type: Array,
        required: true,
    },
});

const deleteTransactionEmit = (id) => {
    emit('deleteTransaction', id)
};

</script>

<template>
    <h3>History</h3>
    <ul id="list" class="list">
        <li v-for="transaction in transactions"
            :key="transaction.id"
            :class="transaction.amount < 0 ? 'minus' : 'plus'"
        >
            {{transaction.text}} <span>${{transaction.amount}}</span>
            <button class="delete-btn" @click="deleteTransactionEmit(transaction.id)">x</button>
        </li>
    </ul>
</template>

<style lang="scss" scoped>

</style>