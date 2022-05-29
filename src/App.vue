<script setup>
    // This starter template is using Vue 3 <script setup> SFCs
    // Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
    import Home from './components/Home.vue'
    import PageTransactions from './components/PageTransactions.vue'
    import PageBalance from './components/PageBalance.vue'
    import PageLeaderboard from './components/PageLeaderboard.vue'
    import TransactionNew from './components/TransactionNew.vue'
    import Login from './components/Login.vue'
    import Signup from './components/Signup.vue'
    import NotFound from './components/NotFound.vue'
    import Methods from './Methods.js'


    // Check out https://vuejs.org/api/composition-api-setup.html
    import { ref, computed, onMounted } from 'vue'
    import Navigation from './components/Navigation.vue';

    let m = new Methods();
    // We create just an object
    const routes = {
        '/leaderboard': PageLeaderboard,
        '/': PageBalance,
        '/transactions': PageTransactions,
        '/new-transaction': TransactionNew,
        '/login': Login,
        '/signup': Signup
    };

    // get the hash from the url, e.g. /#/todo 
    let currentPath = ref(window.location.hash);
    console.log(currentPath + " is the current path");

    // computed properties are derived from other variables, this will return the name of a component 
    let currentView = computed(() => routes[currentPath.value.slice(1) || '/'] || NotFound);
    console.log(currentView);

    let cookie = ref(document.cookie);
        // console.log(cookie);

    // https://vuejs.org/api/composition-api-lifecycle.html#onmounted
    onMounted( () => {
        window.addEventListener('hashchange', () => {
            currentPath.value = window.location.hash;
            // console.log(currentPath.value + " is the current path");
            // console.log(currentView);
            m.redirectOnLoginStatus(cookie, currentPath.value);
        })

        m.redirectOnLoginStatus(cookie, currentPath.value);

    });


</script>





<template>
  <div class="views">
      <div class="notch"></div>
    <component :is="currentView" />
  </div>

  <Navigation v-if="cookie" />
</template>

<style>

</style>
