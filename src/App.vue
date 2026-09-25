<template>
  <div class="container">
    <h1>To Do List</h1>

    <form @submit.prevent="addTask" class="add-form">
      <input 
        v-model="newTaskTitle" 
        type="text" 
        placeholder="Enter new task..." 
        class="input-title"
      />
      
      <select v-model="newTaskPriority" class="select-priority">
        <option value="High">High</option>
        <option value="Medium">Medium</option>
        <option value="Low">Low</option>
      </select>

      <button type="submit" class="btn-add">Add task</button>
    </form>

    <div class="filter-section">
      <label>Filter tasks: </label>
      <button 
        v-for="filterOption in ['All', 'Active', 'Completed']" 
        :key="filterOption"
        @click="filter = filterOption"
        :class="{ active: filter === filterOption }"
        class="btn-filter"
      >
        {{ filterOption }}
      </button>
    </div>

    <div class="task-list">
      <p v-if="filteredTasks.length === 0" class="no-tasks">
        No tasks available.
      </p>

      <TaskItem 
        v-for="task in filteredTasks" 
        :key="task.id" 
        :task="task" 
        @toggle-complete="toggleTaskComplete"
        @delete-task="deleteTask"
      />
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import TaskItem from './components/TaskItem.vue'

const tasks = ref([
  { id: 1, title: "Learn Vue basics", completed: true, priority: "High" },
  { id: 2, title: "Practice Vue directives", completed: false, priority: "Medium" },
  { id: 3, title: "Create To Do App", completed: false, priority: "Low" }
])

const newTaskTitle = ref('')
const newTaskPriority = ref('Medium')
const filter = ref('All')

const addTask = () => {
  if (!newTaskTitle.value.trim()) return

  const newTask = {
    id: Date.now(),
    title: newTaskTitle.value.trim(),
    completed: false,
    priority: newTaskPriority.value
  }

  tasks.value.push(newTask)
  newTaskTitle.value = ''
  newTaskPriority.value = 'Medium'
}

const toggleTaskComplete = (taskId) => {
  const task = tasks.value.find(t => t.id === taskId)
  if (task) {
    task.completed = !task.completed
  }
}

const deleteTask = (taskId) => {
  tasks.value = tasks.value.filter(t => t.id !== taskId)
}

const filteredTasks = computed(() => {
  if (filter.value === 'Active') {
    return tasks.value.filter(t => !t.completed)
  }
  if (filter.value === 'Completed') {
    return tasks.value.filter(t => t.completed)
  }
  return tasks.value
})
</script>

<style>
body {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background-color: #f4f7f6;
  margin: 0;
  padding: 40px 20px;
}

.container {
  max-width: 600px;
  margin: 0 auto;
  background: white;
  padding: 24px;
  border-radius: 8px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

h1 {
  text-align: center;
  color: #333;
  margin-top: 0;
}

.add-form {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.input-title {
  flex: 1;
  padding: 8px 12px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 1rem;
}

.select-priority {
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
  background-color: white;
}

.btn-add {
  padding: 8px 16px;
  background-color: #2196f3;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-weight: bold;
}

.btn-add:hover { background-color: #1e88e5; }

.filter-section {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-bottom: 20px;
  font-size: 0.9rem;
}

.btn-filter {
  padding: 4px 8px;
  border: 1px solid #ddd;
  background-color: #fff;
  border-radius: 4px;
  cursor: pointer;
}

.btn-filter.active {
  background-color: #333;
  color: #fff;
  border-color: #333;
}

.no-tasks {
  text-align: center;
  color: #888;
  font-style: italic;
  padding: 20px 0;
}
</style>