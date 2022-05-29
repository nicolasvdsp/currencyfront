<script setup>
    import { ref, onMounted } from 'vue'

    const title = "newTransaction";

    const currentUser = ref([""]);

    let token = ref(document.cookie);
    let receiver = ref('');
    let amount = ref();
   function check_cookie_name(name) 
    {
      var match = document.cookie.match(new RegExp('(^| )' + name + '=([^;]+)'));
      if (match) {
        return match[2];
      }
      else{
           console.log('something went wrong');
      }
    }
    onMounted(() => {
          let cookie = "token="+check_cookie_name('token');
        fetch('https://imdcurrencyapp.herokuapp.com/users/getUserByToken', 
            {
                method: "POST",
                headers: {
                    "Content-type": "application/json"
                },
                body: JSON.stringify({
                    token: cookie
                })
            })
            .then(res => res.json())
            .then(data => {
                // console.log(data.data.user.username);
                currentUser.value = data.data.user.username;
            });
          
    })
    
    function onSubmit(e) {
        let cookie = "token="+check_cookie_name('token');
        fetch('https://imdcurrencyapp.herokuapp.com/transactions', {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify({
                sender: currentUser,
                receiver: receiver.value,
                amount: amount.value,
                token: cookie
            })
        })
        .then( res => res.json())
        // .then(res => res.json())
        .then(data => {
            console.log(data);
            if(data.status === "success") {
                window.location.href = "/transactions";
            }
            else{
                const errorMessage = document.querySelector('.error-message-login');
                errorMessage.style.display = "block";
                errorMessage.innerHTML = data.message;
            }
        })


        console.log(`${receiver.value} received ${amount.value} camelCoins`);
        receiver.value = '';
        amount.value = '';
    }
</script>

<template>
    <h2>{{ title }}</h2>

    <form @submit.prevent="onSubmit" class="add-form">
      <span class="error-message-login" style="text-align: center;"></span>
        <div class="form-control">
            <!-- <label class="form__label">receiver</label> -->
            <input  class="form__input__text" type="text" v-model="receiver" name="receiver" placeholder="Receiver" />
        </div>
        <div class="form-control">
            <!-- <label class="form__label">amount</label> -->
            <input  class="form__input__text" type="text" v-model="amount" name="amount" placeholder="Amount 0"/>
        </div>
    <input type="submit" value="Save Information" class="btn btn-block form__input__button" />
  </form>


</template>

<style scoped>
    template {
        height: 100px;
    }
</style>