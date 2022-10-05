<template>
  <div class="container">
    <Header
      title="Task Tracker"
      @toggle-add-task="toggleAddTask"
      :showAddTask="showAddTask"
    />
    <div v-show="showAddTask">
      <AddTask @add-task="addTask" />
    </div>
    <Tasks
      :tasks="tasks"
      @toggle-remainder="(id) => onToggleRemainder(id)"
      @delete-task="(id) => onTaskDelete(id)"
    />
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import Tasks from "./components/Tasks.vue";
import AddTask from "./components/AddTask.vue";

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
    async onTaskDelete(id) {
      if (confirm("Are you sure ?")) {
        const res = await fetch(`http://localhost:5000/tasks/${id}`, {
          method: "DELETE",
        });

       res.status === 200 ? (this.tasks = this.tasks.filter(v => v.id !== id)) : alert("Something went wrong")
      }
    },
    async onToggleRemainder(id) {
      const toggleTask = await this.fetchTaskById(id)
      const upTaskRemainder = {...toggleTask, remainder: !toggleTask?.remainder}

      const res = await fetch(`http://localhost:5000/tasks/${id}`, {
        method: 'PUT',
        headers: {
          'Content-type': 'application/json'
        },
        body: JSON.stringify(upTaskRemainder)
      })

      const data = await res.json()
      this.tasks = this.tasks.map((v) =>
        v?.id === id ? { ...v, remainder: data?.remainder } : v
      );
    },
    async addTask(newTask) {
      const res = await fetch("http://localhost:5000/tasks", {
        method: "POST",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(newTask),
      });

      const data = await res.json();
      this.tasks = [...this.tasks, data];
    },
    toggleAddTask() {
      this.showAddTask = !this.showAddTask;
    },
    async fetchTask() {
      const resp = await fetch("http://localhost:5000/tasks");
      const data = await resp.json();

      return data;
    },
    async fetchTaskById(id) {
      const resp = await fetch(`http://localhost:5000/tasks/${id}`);
      const data = await resp.json();

      return data;
    },
  },
  async created() {
    this.tasks = await this.fetchTask();
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Bree+Serif&display=swap");
* {
  font-family: "Bree Serif", serif;
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
.container {
  /* margin: 25px 15px; */
  padding: 1rem 2rem;
  border: 1px solid #000;
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
</style>
