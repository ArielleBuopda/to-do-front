<template>
  <h3> {{ title }} </h3>
  <div>
    <input v-model="taskField" placeholder="Task" type="text">
    <input v-model="descriptionField" placeholder="description" type="text">
    <center><button type="button" @click="save()">Save</button></center>
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
const taskField = ref('')
const descriptionField = ref('')
const selectedUser = ref('')

function save () {
  const baseUrl = import.meta.env.VITE_BACKEND_BASE_URL // 'http://localhost:8080' in dev mode
  const endpoint = baseUrl + '/todo'
  const data: Todo = {
    task: taskField.value,
    description: descriptionField.value
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
      .then(response => response.text())
      .then(data => {
        if (data=="Success"){
          alert("Successful registration")
        }else{
          alert("Registration failed");
        }
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
  checkPage()
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
