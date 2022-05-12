<script setup>
    import { ref, onMounted } from 'vue'
    const title = "All transactions";
    let transactions = ref([1,2]);

    onMounted(() => {
        fetch('http://localhost:3001/transactions')
            .then(res => res.json())
            .then(data => {
                console.log(data);
                transactions.value = data.data.transactions;
                console.log(transactions);
            });
    });

</script>

<template>
    <h1>{{ title }}</h1>

    <div class="transactions">
        <ul>
            <li v-for="(t, index) in transactions" :key="index">{{ t.sender }} transfered {{ t.amount }} to {{ t.receiver }}</li>
        </ul>
    </div>
</template>

<style scoped>
ul {
    max-width: 600px;
    margin: 0 auto;
    text-align: left;
}
</style>