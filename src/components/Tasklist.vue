<template>
  <div class="container">
    <h1>Task Manager</h1>
    <div v-if="tasks.length === 0" class="no-tasks">No tasks available</div>
    <ul class="task-list">
      <li v-for="task in tasks" :key="task.id" :style="taskStyles(task)" class="task-item">
        <span>{{ task.name }}</span>
        <button @click="toggleTaskCompletion(task)" class="complete-btn">Complete</button>
        <button @click="deleteTask(task.id)" class="delete-btn">Delete</button>
      </li>
    </ul>
    <button @click="showForm = !showForm" class="toggle-form-btn">Add Task</button>
    <div v-if="showForm" class="form-container">
      <input v-model="newTaskName" @keyup.enter="addTask" placeholder="Enter task name">
      <button @click="addTask" class="add-task-btn">Add Task</button>
    </div>
  </div>
</template>

<script>
import { reactive, toRefs } from 'vue';

export default {
  setup() {
    const state = reactive({
      tasks: JSON.parse(localStorage.getItem('tasks') || '[]'),
      newTaskName: '',
      showForm: false,
    });

    const addTask = () => {
      if (state.newTaskName.length >= 3) {
        const newTask = { id: Date.now(), name: state.newTaskName, completed: false };
        state.tasks.push(newTask);
        localStorage.setItem('tasks', JSON.stringify(state.tasks));
        state.newTaskName = '';
      }
    };

    const toggleTaskCompletion = (task) => {
      task.completed = !task.completed;
      localStorage.setItem('tasks', JSON.stringify(state.tasks));
    };

    const deleteTask = (id) => {
      state.tasks = state.tasks.filter(task => task.id !== id);
      localStorage.setItem('tasks', JSON.stringify(state.tasks));
    };

    const taskStyles = (task) => ({
      backgroundColor: task.completed ? 'lightgreen' : '',
      borderColor: task.completed ? 'red' : '',
    });

    return {
      ...toRefs(state),
      addTask,
      toggleTaskCompletion,
      deleteTask,
      taskStyles,
    };
  }
};
</script>

<style scoped>
.container {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  background-color: #f9f9f9;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  font-family: Arial, sans-serif;
}

h1 {
  text-align: center;
  color: #333;
}

.no-tasks {
  text-align: center;
  color: #999;
}

.task-list {
  list-style: none;
  padding: 0;
}

.task-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border: 1px solid #ddd;
  padding: 10px;
  margin: 5px 0;
  border-radius: 5px;
  transition: background-color 0.3s ease, border-color 0.3s ease;
}

.complete-btn,
.delete-btn,
.toggle-form-btn,
.add-task-btn {
  background-color: #007bff;
  align-items: center;
  color: #fff;
  border: none;
  border-radius: 5px;
  padding: 5px 10px;
  cursor: pointer;
  transition: background-color 0.3s ease;

}

.complete-btn {
  background-color: #28a745;
}

.delete-btn {
  background-color: #dc3545;
}

.complete-btn:hover,
.delete-btn:hover,
.toggle-form-btn:hover,
.add-task-btn:hover {
  background-color: #0056b3;
}

.form-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 10px;
}

form-container input {
  padding: 5px;
  margin-bottom: 5px;
  width: 80%;
}

form-container input {
  margin-top: 10px;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
  width: 100%;
}

form-container button {
  width: 100%;
}
</style>
