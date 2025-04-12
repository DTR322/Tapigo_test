<script setup>
import { ref, onMounted } from 'vue'

const tasks = ref([])

async function fetchTasks(){
  try {
    const savedTasks = localStorage.getItem('tasks');

    if (savedTasks) {
      tasks.value = JSON.parse(savedTasks)
    } else {
      const response = await fetch('/tasks.json');
      if (!response.ok)
        throw new Error(`HTTP error: ${response.status}`)

      tasks.value = await response.json()


    }
  }
    catch (error){
    console.error('Ошибка загрузки задач:', error);
    }


}

const saveTasks = () => {
  localStorage.setItem('tasks', JSON.stringify(tasks.value))
}


onMounted(() => {
  fetchTasks()
    }
)


</script>

<template>
  <header>
    <h1>
      Список задач
    </h1>
  </header>
  <p/>
  <ul>
    <li v-for="(task) in tasks" :key="task.id">
      <input
          type="checkbox"
          v-model="task.done"
          @change="saveTasks"
      />
      {{ task.title }}
    </li>
  </ul>

</template>

<style scoped>

header{
  text-align: center;
}

ul{
  list-style-type: none;
  margin-left: 20vw;
}

@media (min-width: 1200px){
  input{
  width: 80px;
  height: 80px;
}
li{
  font-size: 60px;
  margin-top: 20px;
}
h1{
  font-size: 80px
}
}


@media (min-width: 900px){
  input{
  width: 60px;
  height: 60px;
}
li{
  font-size: 45px;
  margin-top: 15px;
}
h1{
  font-size: 60px
}
}

@media (min-width: 600px){
  input{
  width: 40px;
  height: 40px;
}
li{
  font-size: 30px;
  margin-top: 10px;
}
h1{
  font-size: 40px
}
}


</style>
