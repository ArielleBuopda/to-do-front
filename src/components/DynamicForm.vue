<template>
  <h3> {{ title }} </h3>


  
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
        <span v-if="item.idtodo"><center><button type="button" @click="del(item.idtodo)">Delete</button></center></span>
        </td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup lang="ts">
import {ref, onMounted} from 'vue'
import type {Ref} from 'vue'
import { useRoute, useRouter } from 'vue-router';

defineProps<{
  title: string
}>()


const route = useRoute();
const router = useRouter();
type todo = { idtodo?: number, task: string, description:string }

const item1s: Ref<Todo[]> = ref([])


function loadTodos () {
  const baseUrl = import.meta.env.VITE_BACKEND_BASE_URL // 'http://localhost:8080' in dev mode
  const endpoint = baseUrl + '/todos'
  const requestOptions: RequestInit = {
    method: 'GET',
    redirect: 'follow',
    headers: {
      'Authorization': sessionStorage.getItem("token")
    },
  }
  fetch(endpoint, requestOptions)
      .then(response => response.json())
      .then(result => result.forEach((todo: Todo) => {
        item1s.value.push(todo)
      }))
      .catch(error => console.log('error', error))
}

function del (test){

  if (confirm("confirm your choice ?")) {
    
  const baseUrl = import.meta.env.VITE_BACKEND_BASE_URL // 'http://localhost:8080' in dev mode
  const endpoint = baseUrl + '/todo/delete'
  const data: Todo = {
    idtodo: test
  }
  const requestOptions: RequestInit = {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': sessionStorage.getItem("token")
    },
    body: JSON.stringify(data)
  }
  fetch(endpoint, requestOptions)
      
      .then(response => response.json())
      .then(result => result.forEach((todo: Todo) => {
        item1s.value.push(todo)
      }))
      .catch(error => console.log('error', error))

  }

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


function checkPage(){
const token = sessionStorage.getItem("token");

if (token === undefined || token === null  || token === 'null' || token === "") {
   router.push('/');
} 
}
// Lifecycle hooks
onMounted(() => {
  loadTodos()
  checkPage()
})
</script>

<style scoped>

</style>
