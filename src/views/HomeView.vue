<template>
  <main>
    <DynamicForm title="List Task(s)"/>
  </main>
</template>

<script setup lang="ts">
import DynamicForm from '../components/DynamicForm.vue'
</script>

<style scoped>
table {
border-collapse:collapse;
width:100%;
max-width:700px;
min-width:400px;
text-align:center;
}

caption {
caption-side:bottom;
font-weight:bold;
font-style:italic;
margin:4px;
}

table,th, td {
border: 1px solid gray;
}

th, td {
height: 24px;
padding:4px;
vertical-align:middle;
}

th {
background-image:url(table-shaded.png);
}

.rowtitle {
background: #9CF;
font-weight:bold;
}
</style>
