<script setup>
import { ref } from 'vue'

defineProps({
  msg: String
})

const count = ref(0)
</script>

<template>
  <h1>hi there</h1>
</template>

<style scoped>
a {
  color: #9342b9;
}
</style>
