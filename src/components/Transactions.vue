<script setup>
    import { ref, onMounted } from 'vue'
    import moment from 'moment';
    import 'moment/dist/locale/en-ie';

    moment.locale('en-ie');

    const title = "allTransactions";
    let transactions = ref([1,2]);

    const currentUser = 'Nicolas';

    onMounted(() => {
        fetch('http://localhost:3001/transactions')
            .then(res => res.json())
            .then(data => {
                transactions.value = data.data.transactions;
            });
    });

</script>

<template>
    <h2>{{ title }}</h2>

    <div class="transactions">
      <div class="transactions__item" v-for="(t, index) in transactions" :key="index">
            <p v-if="t.sender === currentUser" class="transactions__item__name">{{ t.receiver }}</p>
            <p v-else class="transactions__item__name">{{ t.sender }}</p>

            <span class="transactions__item__date">{{ moment(t.date).format('lll') }}</span>
            
            <span v-if="t.sender === currentUser" class="transactions__item__amount transactions__item__amount--out">{{ t.amount }}<span> cC</span></span>
            <span v-else class="transactions__item__amount transactions__item__amount--in">{{ t.amount }}<span> cC</span></span>
      </div>
  </div>
</template>

<style scoped>

</style>