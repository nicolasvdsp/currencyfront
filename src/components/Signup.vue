<template>

    <form @submit.prevent="submit" class="form-login">
        <h1>Sign up</h1>
        <span class="error-message-login"></span>
        <input class="form__input__text" v-model="data.username" placeholder="Username" required>
        <input class="form__input__text" v-model="data.email" type="email" placeholder="Email" required>
        <input class="form__input__text" v-model="data.password" type="password" placeholder="Password" required>
        <input class="form__input__button" type="submit" value="Sign up">
        <span class="form__switch">Already an user? <a href="#/login">Login</a></span>
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