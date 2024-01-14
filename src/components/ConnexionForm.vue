<template>
<body>
<div class="container" id="container">
	<div class="form-container sign-up-container">
		<form @submit.prevent="save">
			<h1>Create Account</h1>
			<input class="my-1" v-model="nameField" type="text" placeholder="Name" />
			<input class="my-1" v-model="surnameField" type="text" placeholder="Surname" />
			<input class="my-1" v-model="emailField" type="email" placeholder="Email" />
			<input class="my-1" v-model="passwordField" type="password" placeholder="Password" />
			<button  type="submit">Sign Up</button>
		</form>
	</div>
	<div class="form-container sign-in-container">
		<form @submit.prevent="connexion">
			<h1>Sign in</h1>
			<input class="my-1" v-model="emailField1" type="email" placeholder="Email" />
			<input class="my-1" v-model="passwordField1" type="password" placeholder="Password" />
			<button type="submit">Sign In</button>
		</form>
	</div>
	<div class="overlay-container">
		<div class="overlay">
			<div class="overlay-panel overlay-left">
				<h1>Welcome Back!</h1>
				<p>To keep connected with us please login with your personal info</p>
				<button class="ghost" id="signIn" @click="handleSignIn">Sign In</button>
			</div>
			<div class="overlay-panel overlay-right">
				<h1>Hello, World!</h1>
				<p>Enter your personal details and start journey with us</p>
				<button class="ghost" id="signUp" @click="handleSignUp">Sign Up</button>
			</div>
		</div>
	</div>
</div>

</body>
</template>
<script setup lang="ts">
import {ref, onMounted} from 'vue'
import type {Ref} from 'vue'
import { useRoute, useRouter } from 'vue-router';
defineProps<{
  title: string
}>()

import ConnexionForm from '../components/ConnexionForm.vue'
type user = { iduser?: number, name: string, surname: string, email: string, password: string }


const route = useRoute();
const router = useRouter();

    function handleSignUp() {
      const container = window.document.getElementById('container') as HTMLElement;
      container.className += " right-panel-active";
    }
    function handleSignIn() {
      const container = window.document.getElementById('container') as HTMLElement;
      container.className = container.className.replace(/\bright-panel-active\b/g, "").trim();
    }


const items: Ref<User[]> = ref([])
const nameField = ref('')
const surnameField = ref('')
const emailField = ref('')
const passwordField = ref('')
const emailField1 = ref('')
const passwordField1 = ref('')

function save () {
  const baseUrl = import.meta.env.VITE_BACKEND_BASE_URL // 'http://localhost:8080' in dev mode
  const endpoint = baseUrl + '/users'
  const data: User = {
    surname: surnameField.value,
    name: nameField.value,
    email: emailField.value,
    password: passwordField.value
  }
  const requestOptions: RequestInit = {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify(data)
  }
  fetch(endpoint, requestOptions)
      .then(response => response.json())
      .then(data => {
        alert('Recording completed');
      })
      .catch(error => console.log('error', error))
}

function connexion () {
  const baseUrl = import.meta.env.VITE_BACKEND_BASE_URL // 'http://localhost:8080' in dev mode
  const endpoint = baseUrl + '/authentificate'
  const data: User = {
    email: emailField1.value,
    password: passwordField1.value
  }
  const requestOptions: RequestInit = {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify(data)
  }
  fetch(endpoint, requestOptions)
      .then(response => response.text())
      .then(data => {
        if ( data == "Fail"){
            alert('Fail');
        }else{
            sessionStorage.setItem('token',data);
            router.push('/home');
        }
        
      })
      .catch(error => console.log('error', error))
}

</script>
