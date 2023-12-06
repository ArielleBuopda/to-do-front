<template>
  <h3> {{ title }} </h3>
  <div>
    <input v-model="taskField" placeholder="Task" type="text">
    <input v-model="descriptionField" placeholder="description" type="text">
    <select v-model="selectedUser">
      <option v-for="u in items" :key="u" :value="u">{{u.name}}</option>
    </select>

    <!--<input v-model="priceField" placeholder="Price" @keyup.enter="save()">-->
    <button type="button" @click="save()">Save</button>
  </div>
  <div>
    <table>
      <thead>
      <tr>
        <th>Task</th>
        <th>Description</th>
        <th>User</th>
      </tr>
      </thead>
      <tbody>
      <tr v-if="item1s.length === 0">
        <td colspan="2">No todos yet</td>
      </tr>
      <tr v-for="item in item1s" :key="item.idtodo">
        <td>{{item.task}}</td>
        <td>{{item.description}}</td>
        <td>
        <span v-if="item.user">{{ item.user.name }}</span>
        </td>
      </tr>
      <tr>
        <td>{{ taskField }}</td>
        <td>{{ descriptionField }}</td>
        <td>{{ selectedUser.name }}</td>
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
type todo = { idtodo?: number, task: string, description:string , user : user}

const items: Ref<User[]> = ref([])
const item1s: Ref<Todo[]> = ref([])
const taskField = ref('')
const descriptionField = ref('')
const selectedUser = ref('')

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
function loadTodos () {
  const baseUrl = import.meta.env.VITE_BACKEND_BASE_URL // 'http://localhost:8080' in dev mode
  const endpoint = baseUrl + '/todos'
  const requestOptions: RequestInit = {
    method: 'GET',
    redirect: 'follow',
  }
  fetch(endpoint, requestOptions)
      .then(response => response.json())
      .then(result => result.forEach((todo: Todo) => {
        item1s.value.push(todo)
      }))
      .catch(error => console.log('error', error))
}

function save () {
  const baseUrl = import.meta.env.VITE_BACKEND_BASE_URL // 'http://localhost:8080' in dev mode
  const endpoint = baseUrl + '/todo'
  const data: Todo = {
    task: taskField.value,
    description: descriptionField.value,
    user: selectedUser
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
  loadTodos()
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
