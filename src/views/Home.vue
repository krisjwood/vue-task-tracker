<template>
  <AddTask v-show="showAddTask" @add-task="addTask" />
  <Tasks @delete-task="deleteTask" @toggle-task="toggleTask" :tasks="tasks" />
</template>

<script>
import Tasks from "../components/Tasks";
import AddTask from "../components/AddTask";

export default {
  name: "Home",
  props: {
    showAddTask: Boolean,
  },
  components: {
    Tasks,
    AddTask,
  },
  data() {
    return {
      tasks: [],
    };
  },
  emits: ["delete-task", "toggle-task"],
  async created() {
    this.tasks = await this.fetchTasks();
  },
  methods: {
    async deleteTask(id) {
      if (confirm(`Are you sure you want to delete task ${id}?`)) {
        const res = await fetch(`api/tasks/${id}`, {
          method: "DELETE",
        });
        if (res.status === 200) {
          this.tasks = this.tasks.filter((task) => task.id !== id);
        } else {
          alert("Error deleting task");
        }
      }
    },
    async toggleTask(id) {
      const taskToToggle = await this.fetchTask(id);
      const updateTask = { ...taskToToggle, reminder: !taskToToggle.reminder };

      await fetch(`api/tasks/${id}`, {
        method: "PUT",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(updateTask),
      });

      this.tasks = await this.fetchTasks();
    },
    async addTask(newTask) {
      const res = await fetch("api/tasks", {
        method: "POST",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(newTask),
      });

      const data = await res.json();

      this.tasks = [...this.tasks, data];
    },
    async fetchTasks() {
      const res = await fetch("api/tasks");

      const data = await res.json();
      return data;
    },
    async fetchTask(id) {
      const res = await fetch(`api/tasks/${id}`);

      const data = await res.json();
      return data;
    },
  },
};
</script>
