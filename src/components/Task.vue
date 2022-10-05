<template>
  <div @dblclick="$emit('toggle-remainder', task.id)" :class="[task.remainder ? 'remainder' : '', 'task']" >
    <h4>
      {{ task?.text }}
      <i class="fa-solid fa-trash" @click="$emit('delete-task', task.id)"  />
    </h4>
    <p>{{ task?.day }}</p>
  </div>
</template>

<script>
export default {
  name: "task-component",
  props: {
    task: Object,
  },
};
</script>

<style scoped>
.fa-solid{
    color:red;
}
.task {
  background: #f4f4f4;
  margin: 5px;
  padding: 10px 20px;
  cursor: pointer;
}

.task.remainder {
  border-left: 5px solid green;
}

.task h4 {
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 18px;
}

.task p {
  font-size: 14px;
}

</style>