<script setup>
    import { ref, onMounted } from 'vue'

    const title = "newTransaction";

    const currentUser = ref([""]);

    let token = ref(document.cookie);
    let receiver = ref('');
    let amount = ref();

    onMounted(() => {
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
    })
    
    function onSubmit(e) {
        fetch('http://localhost:3001/transactions', {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify({
                sender: currentUser,
                receiver: receiver.value,
                amount: amount.value,
                token: token.value
            })
        })
        .then( res => res.json())
        // .then(res => res.json())
        .then(data => {
            console.log(data);
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