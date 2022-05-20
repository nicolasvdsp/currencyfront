<template>
<span class="error-message-login"></span>
    <form @submit.prevent="submit">
        <h1>Sign up</h1>
        <input v-model="data.username" placeholder="Username" required>
        <input v-model="data.email" type="email" placeholder="Email" required>
        <input v-model="data.password" type="password" placeholder="password" required>
        <input type="submit" placeholder="Submit">
    </form>
</template>

<script>
import {reactive} from 'vue';

    export default{
        name: "signup",
        setup(){
            const data = reactive({
                username: "",
                email: "",
                password: ""
            });
         
            const submit = async () => {
                
                const response = await fetch('http://localhost:3001/users/signup', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                  
                    body: JSON.stringify(data)
                });
                const json = await response.json();
              
                //if json status is success then redirect to login page
                if(json.status == "success") {
                   window.location.href = '#/transactions';
                }
                //else log errors
                else {
                    //get query selector
                    const errorMessage = document.querySelector('.error-message-login');
                    errorMessage.innerHTML = json.message;
                }
             
                
             
            };
           
            return {
                data,
                submit
            }
    }

    }
</script>

<style scoped>

</style>