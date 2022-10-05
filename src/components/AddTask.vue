<template>
  <form class="mb-3" @submit="onSubmit">
    <div class="mb-3">
      <label for="text" class="form-label">Task</label>
      <input type="text" v-model="text" class="form-control" id="text" />
    </div>
    <div class="mb-3">
      <label for="day" class="form-label">Day & Time</label>
      <input type="text" v-model="day" class="form-control" id="day" />
    </div>
    <div class="d-flex align-items-center mb-3">
      <label class="form-check-label col-md-2" for="remainder"
        >Set Remainder</label
      >
      <input
        type="checkbox"
        v-model="remainder"
        class="form-check-input ml-2"
        id="remainder"
      />
    </div>
    <button type="submit" class="btn btn-primary form-control">Submit</button>
  </form>
</template>

<script>
export default {
  name: "add-task",
  data() {
    return {
      text: "",
      day: "",
      remainder: false,
    };
  },
  methods: {
    onSubmit(e) {
      e?.preventDefault();

      if (!this.text) {
        alert("Please Fill the task");
      }

      let newTask = {
        id: Math.floor(Math.random() * 100000),
        text: this.text,
        day: this.day,
        remainder: this.remainder,
      };
      this.$emit("add-task", newTask);

      this.text = "";
      this.day = "";
      this.remainder = false;
    },
  },
};
</script>