User
<template>
  <div class="task-grid">

    <div class="task-column">
      <h2 class="section-heading">Open Tasks</h2>
      <ul class="task-list">
        <li v-for="task in openTasks" :key="task.id" class="task-item">
          <span class="task-title">{{ task.title }}</span>
          <span class="task-details">Created on {{ formatDate(task.createdAt) }}</span>
          <button @click="completeTask(task.id)" class="complete-button">Complete</button>
        </li>
      </ul>
    </div>

    <div class="task-column">
      <h2 class="section-heading">Completed Tasks</h2>
      <ul class="task-list">
        <li v-for="task in completedTasks" :key="task.id" class="task-item completed">
          <span class="task-title">{{ task.title }}</span>
          <span class="task-details">Completed </span>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  props: ['tasks'],
  methods: {
    formatDate(date) {
      return new Date(date).toLocaleString();
    },
    completeTask(id) {
      this.$emit('completeTask', id);
    }
  },
  computed: {
    openTasks() {
      return this.tasks.filter(task => !task.completed);
    },
    completedTasks() {
      return this.tasks.filter(task => task.completed);
    }
  }
}
</script>

<style scoped>
.section-heading {
  font-size: 1.5em;
  color: #333;
  margin-bottom: 10px;
}

.task-column {
  padding-right: 30px;
}

.task-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 20px;
}

.task-list {
  list-style-type: none;
  padding: 0;
}

.task-item {
  background-color: #ffffff;
  border: 1px solid #e0e0e0;
  border-radius: 8px;
  padding: 15px;
  transition: transform 0.2s;
}

.task-item:hover {
  transform: translateY(-3px);
}

.task-item.completed {
  background-color: #f0f0f0;
}

.task-title {
  font-weight: bold;
}

.task-details {
  color: #777;
}

.complete-button {
  background-color: #4caf50;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.complete-button:hover {
  background-color: #45a049;
}
</style>