<script setup>
    import { ref, onMounted } from 'vue';
    import moment from 'moment';

    const title = "camelCoin";
    const balance = ref();
    let transactions = ref([]);

    const currentUser = ref({
        username: "",
        balance: 0
    });

    onMounted(() => {
        fetch('http://localhost:3001/transactions/getAll',
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
                transactions.value = data.data.transactions;
            });

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
                console.log(data.data);
                currentUser.value = data.data.user;
                balance.value = data.data.balance
            });
    })
    function logout(e) {
        e.preventDefault();
        window.location.href = '/';
        document.cookie = "token=; expires=Thu, 01 Jan 1970 00:00:00 UTC; path=/;";
    }
    
</script>

<template>
  <h2>{{ title }}</h2>
  <a href="#" v-on:click="logout">logout</a>

  <div class="balance">
        <span>{{ currentUser }}</span>
        <h3>currentBalance</h3>
        <p class="balance__amount">{{ balance }}<span class="balance__amount__symbol">cC</span></p>
  </div>

  <div class="transactions">
      <h3 class="transactions__title">recentTransactions</h3>
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
