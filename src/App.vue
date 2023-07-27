<template>
  <div class="app">
    <h1>TODO List</h1>
    <form @submit.prevent="addTask">
      <input v-model="newTaskText" type="text" placeholder="Enter your task" />
      <button type="submit">Add Task</button>
    </form>
    <ul>
      <li v-for="task in tasks" :key="task.id">
        <span v-if="editTaskId !== task.id">{{ task.text }}</span>
        <input
          v-else
          v-model="newTaskText"
          @keyup.enter="updateTask"
          @blur="updateTask"
          type="text"
        />
        <div class="task-buttons">
          <button @click="editTask(task)">Update</button>
          <button @click="deleteTask(task.id)">Delete</button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTaskText: "", // input text entered by user
      tasks: [
        { id: 1, text: "Read" },
        { id: 2, text: "Write" },
        { id: 3, text: "Grow" },
      ],
      editTaskId: null,
    };
  },
  methods: {
    addTask() {
      if (this.newTaskText.trim() !== "") {
        const newTask = {
          id: Date.now(),
          text: this.newTaskText,
        };
        this.tasks.push(newTask);
        this.newTaskText = ""; //  clears the input field
      }
    },
    editTask(task) {
      // task that the user wants to edit.
      this.newTaskText = task.text;
      this.editTaskId = task.id;
    },
    updateTask() {
      if (this.editTaskId && this.newTaskText.trim() !== "") {
        const taskIndex = this.tasks.findIndex(
          (task) => task.id === this.editTaskId
        );
        if (taskIndex !== -1) {
          this.tasks[taskIndex].text = this.newTaskText;
          this.newTaskText = "";
          this.editTaskId = null;
        }
      }
    },
    deleteTask(id) {
      this.tasks = this.tasks.filter((task) => task.id !== id);
    },
  },
};
</script>

<style>
.app {
  max-width: 500px;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
  font-family: Arial, sans-serif;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 8px;
  margin: 8px 0;
  border: 1px solid #ddd;
  border-radius: 3px;
  background-color: #f9f9f9;
}

li span {
  flex: 1;
  margin-right: 10px;
}

button {
  padding: 5px 10px;
  cursor: pointer;
  background-color: #1a75ff;
  color: #fff;
  border: none;
  border-radius: 3px;
  margin-right: 5px;
}

button:hover {
  background-color: #0056b3;
}

form {
  display: flex;
  margin-bottom: 10px;
}

input {
  flex: 1;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 3px;
}

input:focus {
  outline: none;
  border-color: #007bff;
}

button[type="submit"] {
  margin-left: 10px;
}
</style>
