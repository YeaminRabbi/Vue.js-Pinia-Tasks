<template>
  <main>

      <!-- heading -->
      <header>
        <img src="./assets/pinia-logo.svg" alt="pinia-logo">
        <h1>{{ taskStore.name }}</h1>
      </header>



      <!-- nav task -->
      <div class="new-task-form">
        <TaskForm></TaskForm>
      </div>

      <!-- filter  -->
      <nav class="filter">
        <button @click="filter = 'all'">All Tasks</button>
        <button @click="filter = 'favs'">Fav Tasks</button>

      </nav>


      <!-- loading -->
      <div class="loading" v-if="taskStore.loading">
        Loading tasks...
      </div>

      <!-- task -->
      <div class="task-list" v-if="filter === 'all'">
        <p>You have {{ taskStore.totalCount }} tasks left to do</p>
        <div v-for="task in taskStore.tasks" :key="task.id">
            <TaskDetails :task="task"></TaskDetails>
        </div>
      </div>

       <!-- Fav task -->
       <div class="task-list" v-if="filter === 'favs'">
        <p>You have {{ taskStore.favCount }} tasks left to do</p>
        <div v-for="task in taskStore.favs" :key="task.id">
            <TaskDetails :task="task"></TaskDetails>
        </div>
      </div>

      <!-- <button @click="taskStore.$reset">Reset</button> -->

  </main>
</template>

<script>
import { ref } from 'vue'
import TaskDetails from './components/TaskDetails.vue'
import TaskForm from './components/TaskForm.vue'
import { useTaskStore } from './stores/TaskStore.js'
import { storeToRefs } from 'pinia'

export default {
  components : { TaskDetails, TaskForm },
  setup () {
    const taskStore = useTaskStore()

    // const { task, loading, favs, totalCount, favCount } = storeToRefs(taskStore)

    //fetch tasks data from data.json
    taskStore.getTasks()

    const filter = ref('all')
    return { taskStore , filter}
  }
}
</script>

