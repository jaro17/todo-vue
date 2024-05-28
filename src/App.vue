<script setup>
import {ref, onMounted} from "vue";

const taskText = ref("");
const tasks = ref([]);

onMounted(() => {
  tasks.value = JSON.parse(localStorage.getItem('tasks'));
})


function addTask() {
  const text = taskText.value.trim();
  if (text !== "") {
    tasks.value.push({text, completed: false});
  }
  updateTasks();
  taskText.value = "";
}

function updateTasks() {
  localStorage.setItem("tasks", JSON.stringify(tasks.value));
}

function toggleTask(index) {
 tasks.value[index].completed = !tasks.value[index].completed;
  updateTasks();
}

function deleteTask(index) {
      tasks.value.splice(index, 1);
      updateTasks();
    }


</script>

<template>
    <h1 class="is-size-4 has-text-centered mt-4">To Do List</h1>

<div class="container ">
    <div class="box">
        <form>
            <input class="input" v-model="taskText" id="taskInput" @keyup.enter="addTask" type="text">
            <button class="button is-light mt-2" @click.prevent="addTask()">Add</button>
            <ul id="taskList">
              <li v-for="(task, index) in tasks" :key="index" class="mt-3">
                <span :class="{ completed: task.completed }">{{ task.text }}</span>
                <button @click="toggleTask(index)" class="button btn">
                  Mark {{ task.completed ? 'Incomplete' : 'Complete' }}
                </button>
                <button @click="deleteTask(index)" class="button">Delete</button>
              </li>
            </ul>
        </form>
    </div>
</div>

</template>

<style scoped>
 form li {
            width: 100%;
            display: flex;
            align-items: center;
            justify-content: space-between;
            margin-bottom: 1rem;
        }

        .completed {
            text-decoration: line-through;
            color: #999;
        }

        .box {
            max-width: 40rem;
            margin: 3rem auto;
        }

        .btn {
            position: absolute;
            left: 50%;
        }

</style>
