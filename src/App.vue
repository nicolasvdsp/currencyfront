<script setup>
    // This starter template is using Vue 3 <script setup> SFCs
    // Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
    import Home from './components/Home.vue'
    import Transactions from './components/Transactions.vue'
    import Balance from './components/Balance.vue'
    import Leaderboard from './components/Leaderboard.vue'
    import TransactionNew from './components/TransactionNew.vue'
    import Login from './components/Login.vue'
    import Signup from './components/Signup.vue'
    import NotFound from './components/NotFound.vue'

    // Check out https://vuejs.org/api/composition-api-setup.html
    import { ref, computed, onMounted } from 'vue'
    import Navigation from './components/Navigation.vue';

    // We create just an object
    const routes = {
        '/leaderboard': Leaderboard,
        '/': Balance,
        '/transactions': Transactions,
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
            redirectOnLoginStatus(cookie);
        })

        redirectOnLoginStatus(cookie);

    });

    function redirectOnLoginStatus(cookie) {
        if(currentPath.value != "#/login" || currentPath.value != "#/signup") {
            let cookie = getCookie("token");
            if(cookie == null) {
                window.location.href = '#/login';
            }
        } 

        if(currentPath.value === "#/login" || currentPath.value === "#/signup"){
            let cookie = getCookie("token");
            if(cookie != null) {
                window.location.href = '#/transactions';
            }
        }
    }

    function getCookie(name) {
    var dc = document.cookie;
    var prefix = name + "=";
    var begin = dc.indexOf("; " + prefix);
    if (begin == -1) {
        begin = dc.indexOf(prefix);
        if (begin != 0) return null;
    }
    else
    {
        begin += 2;
        var end = document.cookie.indexOf(";", begin);
        if (end == -1) {
        end = dc.length;
        }
    }
    // because unescape has been deprecated, replaced with decodeURI
    //return unescape(dc.substring(begin + prefix.length, end));
    return decodeURI(dc.substring(begin + prefix.length, end));
} 

</script>





<template>
  <div class="notch"></div>
  <div class="views">
    <component :is="currentView" />
  </div>

  <Navigation v-if="cookie" />
</template>

<style>

</style>
