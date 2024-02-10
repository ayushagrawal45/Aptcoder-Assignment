<template>
  <div id="app">
    <div class="header">
      <h1>To-Do App</h1>
      <img class="todo-icon" src="https://media.istockphoto.com/id/1330040188/photo/to-do-list-in-spiral-notepad-isolated-on-desk.jpg?s=612x612&w=0&k=20&c=ksolS1xqL9gnTOaXS4OFGvF3S_kfV3CW0tMhaQ8wkUg=" alt="ToDo Icon" />
    </div>
    <div class="container">
      <div class="add-task">
        <h2>Add Task</h2>
        <div class="input-wrapper">
          <input v-model="newTask" placeholder="Add New task..." />
          <button @click="addTask">Add</button>
        </div>
      </div>
      <div class="task-list-wrapper">
        <task-list :tasks="tasks" @completeTask="completeTask"></task-list>
      </div>
    </div>
  </div>
</template>

<script>
import TaskList from './components/TaskList.vue';

export default {
  name: 'App',
  components: {
    TaskList,
  },
  data() {
    return {
      tasks: [],
      newTask: '',
    };
  },
  methods: {
    async addTask() {
      try {
        const newTask = { title: this.newTask, createdAt: new Date(), completed: false };
        await this.$http.post('/tasks', newTask);
        this.fetchTasks();
        this.newTask = '';
      } catch (error) {
        console.error('Error adding task:', error);
      }
    },
    async completeTask(id) {
      try {
        await this.$http.patch(`/tasks/${id}`, { completed: true, completedAt: new Date() });
        this.fetchTasks();
      } catch (error) {
        console.error('Error completing task:', error);
      }
    },
    async fetchTasks() {
      try {
        const response = await this.$http.get('/tasks');
        this.tasks = response.data;
      } catch (error) {
        console.error('Error fetching tasks:', error);
      }
    },
  },
  mounted() {
    this.fetchTasks();
  },
};
</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.add-task {
  width: 80%;
  margin-bottom: 20px;
}

.input-wrapper {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

input {
  flex: 1; 
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin-right: 10px;
}

button {
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  background-color: #4CAF50;
  color: white;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #45a049;
}

.header {
  background-color: #2c3e50; 
  color: white;
  padding: 20px 0;
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

h1 {
  margin: 0;
  font-size: 24px;
}

.todo-icon {
  width: 50px; 
  height: 50px;
  margin-left: 10px;
}

.task-list-wrapper {
  width: 80%;
}
</style>