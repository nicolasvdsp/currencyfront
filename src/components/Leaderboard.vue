<script setup>
    import { ref, onMounted } from 'vue'

    const title = "leaderBoard";
    let allUsers = ref([1,2]);

    onMounted(() => {
        fetch('https://imdcurrencyapp.herokuapp.com/users/getallbybalance')
            .then(res => res.json())
            .then(data => {
                allUsers.value = data.data.users;
            });
    });

</script>

<template>
    <h2>{{ title }}</h2>

    <div class="leaderboard">
        <div class="leaderboard__item">
            <p class="leaderboard__item__name">
                <span class="leaderboard__item__ranking">#</span>
                userName
            </p>
            <span class="leaderboard__item__balance">balance</span>
        </div>
        <hr style="margin-top: -0.5rem">

        <div v-for="(u, index) in allUsers" :key="index" class="leaderboard__item">
            <p class="leaderboard__item__name">
                <span class="leaderboard__item__ranking">{{ index + 1 }}</span>
                {{ u.username }}
            </p>
            <span class="leaderboard__item__balance">{{ u.balance }}</span>
        </div>

    </div>

</template>

<style scoped>

</style>