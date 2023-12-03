<template>
  <h3> {{ title }} </h3>
  <div>
    <input v-model="taskField" placeholder="Task" type="text">
    <input v-model="descriptionField" placeholder="description" type="text">
    <select v-model="selectedUser">
      <option v-for="u in items" :key="u.id" :value="u.id">{{u.name}}</option>
    </select>

    <!--<input v-model="priceField" placeholder="Price" @keyup.enter="save()">-->
    <button type="button" @click="save()">Save</button>
  </div>
  <div>
    <table>
      <thead>
      <tr>
        <th>Name</th>
        <th>E-mail</th>
        <th>Password</th>
      </tr>
      </thead>
      <tbody>
      <tr v-if="items.length === 0">
        <td colspan="2">No users yet</td>
      </tr>
      <tr v-for="item in items" :key="item.iduser">
        <td>{{item.name}}</td>
        <td>{{item.email}}</td>
        <td>{{item.password}}</td>
      </tr>
      <tr>
        <td>{{ nameField }}</td>
        <td>{{ emailField }}</td>
        <td>{{ passwordField }}</td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup lang="ts">
import {ref, onMounted} from 'vue'
import type {Ref} from 'vue'

defineProps<{
  title: string
}>()

type user = { iduser?: number, name: string, email: string, password: string }

const items: Ref<User[]> = ref([])
const nameField = ref('')
const emailField = ref('')
const passwordField = ref('')

function loadUsers () {
  const baseUrl = import.meta.env.VITE_BACKEND_BASE_URL // 'http://localhost:8080' in dev mode
  const endpoint = baseUrl + '/users'
  const requestOptions: RequestInit = {
    method: 'GET',
    redirect: 'follow',
  }
  fetch(endpoint, requestOptions)
      .then(response => response.json())
      .then(result => result.forEach((user: User) => {
        items.value.push(user)
      }))
      .catch(error => console.log('error', error))
}

function save () {
  const baseUrl = import.meta.env.VITE_BACKEND_BASE_URL // 'http://localhost:8080' in dev mode
  const endpoint = baseUrl + '/users'
  const data: User = {
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
        console.log('Success:', data)
      })
      .catch(error => console.log('error', error))
}

// Lifecycle hooks
onMounted(() => {
  loadUsers()
})
</script>

<style scoped>
h3 {
  text-align: center;
}
table {
  margin-left: auto;
  margin-right: auto;
}
button {
  color: blue;
}
</style>
