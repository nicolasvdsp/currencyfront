<template>
   <form @submit.prevent="submit">
         <h1>Login</h1>
            <input v-model="data.username" placeholder="Username" required>
            <input v-model="data.password" type="password" placeholder="password" required>
            <input type="submit" placeholder="Login">
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
               
            }
            return {
                data,
                submit
            }
        },
    }
</script>