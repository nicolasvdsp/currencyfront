<script setup>
    import { ref, onMounted } from 'vue'
    import moment from 'moment';
    import 'moment/dist/locale/en-ie';
    import Methods from '../../src/Methods'

    let m = new Methods();

    moment.locale('en-ie');

    const title = "allTransactions";
    let transactions = ref([1,2]);

    const currentUser = ref(['Nicolas']);

    onMounted(() => {
        fetch('http://localhost:3001/transactions/' + document.cookie )
            .then(res => res.json())
            .then(data => {
                transactions.value = data.data.transactions;
            });
    
        // let user = m.getUserByToken(document.cookie);

        fetch('http://localhost:3001/users/getUserByToken', 
            {
                method: "POST",
                headers: {
                    "Content-type": "application/json"
                },
                body: JSON.stringify({
                    token: document.cookie
                })
            })
            .then(res => res.json())
            .then(data => {
                // console.log(data.data.user.username);
                currentUser.value = data.data.user.username;
            });

    });


</script>

<template>
    <h2>{{ title }}</h2>
    <a href="#/new-transaction" class="btn--sticky">+</a>

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