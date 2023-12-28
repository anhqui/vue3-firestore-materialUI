<template>
    <div class="container">
         <div class="row">
            <div class="col s12 m8 offset-m2">
                <div class="login card-panel green white-text center">
                    <h3>Login</h3>
                    <form @submit.prevent="login">
                    <div class="input-field">
                        <i class="material-icons prefix">email</i>
                        <input type="email" v-model="email" id="email" class="white-text" required>
                        <label class="white-text" for="email">Email</label>
                    </div>
                    <div class="input-field">
                        <i class="material-icons prefix">lock</i>
                        <input type="password" v-model="password" id="password" class="white-text" required>
                        <label class="white-text" for="password">Password</label>
                    </div>
                     <button type="submit" class="btn btn-large grey lighten-4 black-text">Login</button>
               </form>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import {ref} from 'vue'
import { useRouter} from "vue-router"
import { getAuth, signInWithEmailAndPassword  } from "firebase/auth";

const email = ref('')
const password = ref('')
const router = useRouter();
const auth = getAuth();
const login = () => {
    signInWithEmailAndPassword (auth, email.value, password.value)
    .then(userCredential=>{
        alert(`You are logged in as ${userCredential.user.email}`)
        // router.push('/')
        router.go({path: router.path})
    })
    .catch(error=>{
        // alert(error.message)
        alert("Login is invalid")
        })
}
</script>