<template>
  <div 
    class="task-item" 
    :class="{ 
      'completed': task.completed,
      'high-priority': task.priority === 'High'
    }"
  >
    <div class="task-info">
      <span class="task-title">{{ task.title }}</span>
      
      <span class="badge" :class="task.priority.toLowerCase()">
        {{ task.priority }}
      </span>
      
      <span class="status">
        ({{ task.completed ? 'Completed' : 'Pending' }})
      </span>
    </div>

    <div class="task-actions">
      <button 
        @click="$emit('toggle-complete', task.id)" 
        class="btn btn-complete"
      >
        {{ task.completed ? 'Undo' : 'Complete' }}
      </button>

      <button 
        @click="$emit('delete-task', task.id)" 
        class="btn btn-delete"
      >
        Delete
      </button>
    </div>
  </div>
</template>

<script setup>
defineProps({
  task: {
    type: Object,
    required: true
  }
})

defineEmits(['toggle-complete', 'delete-task'])
</script>

<style scoped>
.task-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 12px 16px;
  margin-bottom: 10px;
  background-color: #f9f9f9;
  border-radius: 6px;
  border-left: 5px solid #ccc;
  transition: all 0.3s ease;
}

.task-item.completed {
  background-color: #e8f5e9;
  text-decoration: line-through;
  opacity: 0.7;
}

.task-item.high-priority {
  border-left-color: #e53935;
}

.task-info {
  display: flex;
  align-items: center;
  gap: 10px;
}

.task-title {
  font-weight: 500;
}

.badge {
  padding: 2px 8px;
  border-radius: 12px;
  font-size: 0.8rem;
  font-weight: bold;
  color: white;
}

.badge.high { background-color: #e53935; }
.badge.medium { background-color: #fb8c00; }
.badge.low { background-color: #43a047; }

.status {
  font-size: 0.85rem;
  color: #666;
}

.task-actions {
  display: flex;
  gap: 8px;
}

.btn {
  padding: 6px 12px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-weight: bold;
}

.btn:hover { opacity: 0.8; }
.btn-complete { background-color: #4caf50; color: white; }
.btn-delete { background-color: #f44336; color: white; }
</style>