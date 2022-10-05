<template>
  <div :key="task.id" v-for="task in tasks">
    <Task :task="task" @toggle-remainder="(id) => $emit('toggle-remainder', id)" @delete-task="(id) => $emit('delete-task',id)" />
  </div>
</template>

<script>
import Task from "./Task.vue";

export default {
  name: "tasks-list",
  components: {
    Task,
  },
  props: {
    tasks: Array,
  },
  emits:['delete-task', 'toggle-remainder']
};
</script>

<style scoped>
</style>