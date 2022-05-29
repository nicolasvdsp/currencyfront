<script setup>
    import { ref, onMounted } from 'vue'
    import moment from 'moment';
    import 'moment/dist/locale/en-ie';
    import Methods from '../../src/Methods'

    const props = defineProps({
        sender: String,
        receiver: String,
        date: String,
        amount: Number,
        currentUser: String
    });



</script>

<template>

    <!-- <div>{{ sender }} en {{ receiver }}</div> -->

    <div class="transactions__item">
        <p v-if="sender === currentUser" class="transactions__item__name">{{ receiver }}</p>
        <p v-else class="transactions__item__name">{{ sender }}</p>

        <span class="transactions__item__date">{{ moment(date).format('lll') }}</span>
        
        <span v-if="sender === currentUser" class="transactions__item__amount transactions__item__amount--out">{{ amount }}<span> cC</span></span>
        <span v-else class="transactions__item__amount transactions__item__amount--in">{{ amount }}<span> cC</span></span>
    </div>


</template>

<style scoped>

</style>