<script setup>
import { ref, onMounted } from "vue"

const name = ref("John Doe")
const status = ref("active")
const tasks = ref(["One", "Two", "Three"])
const newTask = ref("")

const toggleStatus = () => {
  if (status.value === 'inactive') {
    status.value = 'pending'
  } else if (status.value === 'pending') {
    status.value = 'active'
  } else {
    status.value = 'inactive'
  }
}

const addTask = () => {
  if (newTask.value.trim() !== "") {
    tasks.value.push(newTask)
    newTask.value = ""
  }
}

const deleteTask = (index) => {
  tasks.value.splice(index, 1)
}

onMounted(async () => {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/todos")
    const data = await response.json()
    tasks.value = data.map(task => task.title)
  } catch (error) {
    console.error("Error fetching tasks.")
  }
}) 

</script>

<template>
  <h1>{{ name }}</h1>
  <p v-if="status === 'active'">User is active</p>
  <p v-else-if="status === 'pending'">User is pending</p>
  <p v-else>User is inactive</p>
  <form @submit.prevent="addTask">
    <label for="newTask">Add task</label>
    <input type="text" id="newTask" name="newTask" v-model="newTask" />
    <button type="submit">Submit</button>
  </form>
  <h3>Tasks</h3>
  <ul>
    <li v-for="(task, index) in tasks" :key="task">
      <span>{{ task }}</span>
      <button @click="deleteTask(index)">x</button>
    </li>
  </ul>
  <a v-bind:href="link">google</a>
  <button @click="toggleStatus">Change status</button>
</template>
