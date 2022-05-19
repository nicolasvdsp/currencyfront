<script setup>
    import { ref, onMounted } from 'vue'

    const title = "newTransaction";

    let token = ref(document.cookie);
    let sender = 'Nicolas' //comes from token
    let receiver = ref('ijsbeer');
    let amount = ref(Math.floor(Math.random()*10));

    onMounted(() => {
        
    })
    
    function onSubmit(e) {
        fetch('http://localhost:3001/transactions', {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify({
                sender: sender,
                receiver: receiver.value,
                amount: amount.value,
                token: token.value
            })
        })

        console.log(`${receiver.value} received ${amount.value} camelCoins`);
        receiver.value = '';
        amount.value = '';
    }
</script>

<template>
    <h2>{{ title }}</h2>

    <form @submit.prevent="onSubmit" class="add-form">
        <div class="form-control">
            <label>receiver</label>
            <input type="text" v-model="receiver" name="receiver" placeholder="username" />
        </div>
        <div class="form-control">
            <label>amount</label>
            <input type="text" v-model="amount" name="amount" placeholder="how many camerlCoins?"/>
        </div>
    <input type="submit" value="Save Information" class="btn btn-block" />
  </form>


</template>

<style scoped>
    template {
        height: 100px;
    }
</style>