<template>

   <form @submit.prevent="submit" class="form-login">
         <h1>Login</h1>
         <span class="error-message-login"></span>
            <input class="form__input__text" v-model="data.username" placeholder="Username" required>
            <input class="form__input__text" v-model="data.password" type="password" placeholder="Password" required>
            <input class="form__input__button" type="submit" value="Login">
            <span class="form__switch">No account yet? <a href="#/signup">Signup</a></span>
   </form>

   
</template>

<script>
  import {reactive} from 'vue';
    export default{
        name: "login",
        setup(){
            const data = reactive({
                username: "",
                password: ""
            });
            const submit = async () => {
                 const response = await fetch('http://localhost:3001/users/login', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    
                    body: JSON.stringify(data)
                });
                const json = await response.json();
           
           
                if(json.status == "success") {
            

                    // document.cookie = `token=${json.data.token};expires=${json.data.expires}`;
                    var expires = (new Date(Date.now()+ 86400*1000)).toUTCString();
                    document.cookie = `token=${json.data.token}; expires=` + expires + ";path=/;"
                   
                    window.location.href = '/';
                }
                //console log errors
                else {
                    //get query selector
                    const errorMessage = document.querySelector('.error-message-login');
                    errorMessage.style.display = "block";
                    errorMessage.innerHTML = json.message;
                
                }
               
            }
            return {
                data,
                submit
            }
        },
    }
</script>