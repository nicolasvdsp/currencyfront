<script setup>
  // This starter template is using Vue 3 <script setup> SFCs
  // Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
  import Home from './components/Home.vue'
  import Transactions from './components/Transactions.vue'
  import Balance from './components/Balance.vue'
  import Leaderboard from './components/Leaderboard.vue'

  // Check out https://vuejs.org/api/composition-api-setup.html
  import { ref, computed, onMounted } from 'vue'
  import Navigation from './components/Navigation.vue';

  // We create just an object
  const routes = {
    '/leaderboard': Leaderboard,
    '/': Balance,
    '/transactions': Transactions
  };

  // get the hash from the url, e.g. /#/todo 
  let currentPath = ref(window.location.hash);
  console.log(currentPath + " is the current path");

  // computed properties are derived from other variables, this will return the name of a component 
  let currentView = computed(() => routes[currentPath.value.slice(1) || '/'] || Balance);
  console.log(currentView);

  // https://vuejs.org/api/composition-api-lifecycle.html#onmounted
  onMounted( () => {
      window.addEventListener('hashchange', () => {
        currentPath.value = window.location.hash;
        console.log(currentPath.value + " is the current path");
        console.log(currentView);
        
      })
    });

</script>





<template>
  <div class="notch"></div>
  <component :is="currentView" />
  <Navigation/>
</template>

<style>

</style>
