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
    function check_cookie_name(name) 
    {
      var match = document.cookie.match(new RegExp('(^| )' + name + '=([^;]+)'));
      if (match) {
        return match[2];
      }
      else{
           console.log('something went wrong');
      }
    }
    
    onMounted(() => {
        let cookie = "token="+check_cookie_name('token');
        fetch('https://imdcurrencyapp.herokuapp.com/transactions/getAll',
            {
                method: "POST",
                headers: {
                    "Content-type": "application/json"
                },
                body: JSON.stringify({
                    token: cookie
                })
            })
            .then(res => res.json())
            .then(data => {
                transactions.value = data.data.transactions;
            });
    

        fetch('https://imdcurrencyapp.herokuapp.com/users/getUserByToken', 
            {
                method: "POST",
                headers: {
                    "Content-type": "application/json"
                },
                body: JSON.stringify({
                    token: cookie
                })
            })
            .then(res => res.json())
            .then(data => {
                // console.log(data.data);
                currentUser.value = data.data.user;
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