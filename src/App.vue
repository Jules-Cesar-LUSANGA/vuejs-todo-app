<script setup>
import { ref, defineEmits, onMounted } from "vue";

import Task from './components/Task.vue';

const tasks = ref([])

const addTask = ref('')

function saveTask() {

  // Check if task is not empty
  if (addTask.value == '') {
    alert('Your task is empty')
  } else {
    // Create new task
    const newTask = {
      content : addTask.value
    }
    // Add it to our tasks
    tasks.value.push(newTask)

    // Clear input field
    addTask.value = ''

    // Update local storage
    refreshSavedOnLocalStorage()
  }
}

function deletedTask(task) {
  // Get index of this task
  const indexOfTask = tasks.value.indexOf(task)

  // Delete it
  tasks.value.splice(indexOfTask, 1)

  refreshSavedOnLocalStorage()
}

function completedOrNot(task) {
  // Get index of this task
  const indexOfTask = tasks.value.indexOf(task)

  // Change completed state
  tasks.value[indexOfTask].completed = !task.completed

  refreshSavedOnLocalStorage()
}

function deleteAllTasks()
{
  tasks.value = []

  refreshSavedOnLocalStorage()
}

// Update local storage data
function refreshSavedOnLocalStorage() {
  localStorage.setItem('tasks', JSON.stringify(tasks.value))
}

// Load all tasks from local storage if exist
onMounted(() => {
  if (localStorage.getItem('tasks') != null) {
    tasks.value = JSON.parse(localStorage.getItem('tasks'))
  }
})

</script>

<template>
  <div class="flex items-center justify-center h-screen font-mono">
    <div id="app-container">
      <div class="bg-teal-300 p-3 rounded-lg shadow-lg shadow-teal-400">
        
        <h1 class="text-teal-900 font-bold text-2xl text-center mb-3">ToDo-List</h1>
        
        <form @submit.prevent="saveTask()" class="mb-3">
          <div class="flex items-center justify-center">
            <input type="text" v-model="addTask" name="addTask" id="addTask" class="border p-1 px-2 rounded-l-2xl">
            <button type="button" @click="saveTask()" class="bg-cyan-600 hover:bg-cyan-500 py-1 px-3 rounded-r-2xl">Add Task</button>
          </div>
        </form>

        <div id="tasks-container" v-if="tasks.length > 0">
          <ul>
            <Task v-for="task in tasks" :key="task.content" :task="task" @deleted="deletedTask(task)" @completed-or-not="completedOrNot(task)"></Task>
          </ul>
        </div>

        <button type="button" class="bg-red-500 hover:bg-red-400 mt-3 font-bold text-white hover:text-black py-1 px-2 rounded" @click="deleteAllTasks()" v-if="tasks.length > 0">Delete All Tasks</button>
      
      </div>

      <p class="text-center mt-3">
        Made with <span class="text-red-500">&hearts;</span> By <a href="https://ljcesar.vercel.app" class="font-bold hover:text-blue-500 underline hover:no-underline">Jules-César LUSANGA</a>
      </p>
    </div>
    
  </div>
  
</template>
