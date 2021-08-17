<template>
  <div class="container">
    <Header
      @btn-click="toggleAddForm"
      title="Task Tracker"
      :showAddTask="showAddTask"
    />
    <div v-show="showAddTask">
      <AddTask @add-task="addTask" />
    </div>
    <Tasks @delete-task="deleteTask" @toggle-task="toggleTask" :tasks="tasks" />
  </div>
</template>

<script>
import Header from "./components/Header";
import Tasks from "./components/Tasks";
import AddTask from "./components/AddTask";
export default {
  name: "App",
  components: {
    Header,
    Tasks,
    AddTask,
  },
  data() {
    return {
      tasks: [],
      showAddTask: false,
    };
  },
  methods: {
    toggleAddForm() {
      this.showAddTask = !this.showAddTask;
    },
    deleteTask(id) {
      if (confirm(`Are you sure you want to delete task ${id}?`)) {
        this.tasks = this.tasks.filter((task) => task.id !== id);
      }
    },
    toggleTask(id) {
      this.task = this.tasks.map((task) => {
        if (task.id === id) {
          task.reminder = !task.reminder;
        }
      });
    },
    addTask(newTask) {
      this.tasks = [...this.tasks, newTask];
    },
  },
  emits: ["delete-task", "toggle-task"],
  created() {
    this.tasks = [
      {
        id: 1,
        text: "Bla",
        day: "August 5th at 2:30pm",
        reminder: true,
      },
      {
        id: 2,
        text: "Foo",
        day: "August 5th at 1:30pm",
        reminder: true,
      },
      {
        id: 3,
        text: "Zoo",
        day: "August 5th at 3:30pm",
        reminder: false,
      },
      {
        id: 4,
        text: "Naaaa",
        day: "August 5th at 4:30pm",
        reminder: true,
      },
    ];
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap");
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: "Poppins", sans-serif;
}
.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}
.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}
.btn:focus {
  outline: none;
}
.btn:active {
  transform: scale(0.98);
}
.btn-block {
  display: block;
  width: 100%;
}
</style>
