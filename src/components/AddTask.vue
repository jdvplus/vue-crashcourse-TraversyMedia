<template>
  <form @submit.prevent="onSubmit" class="add-form">
    <div class="form-control">
      <label>Task</label>
      <input
        type="text"
        v-model="text"
        name="text"
        placeholder="e.g. Call Dad, run errands"
      />
    </div>
    <div class="form-control">
      <label>Day & Time</label>
      <input
        type="text"
        v-model="day"
        name="day"
        placeholder="03/09, 11:30am"
      />
    </div>
    <div class="form-control form-control-check">
      <label>Set Reminder</label>
      <input type="checkbox" v-model="reminder" name="reminder" />
    </div>
    <input type="submit" value="Save Task" class="btn btn-block" />
  </form>
</template>

<script setup>
import { ref } from 'vue';

// emits
const emit = defineEmits(['add-task']);

// data
const text = ref('');
const day = ref('');
const reminder = ref(false);

// methods
const onSubmit = (e) => {
  if (!text.value) {
    alert('Please add a task');
    return;
  }

  const newTask = {
    // id: Math.floor(Math.random() * 100000),
    text: text.value,
    day: day.value,
    reminder: reminder.value,
  };
  // console.log('new task', newTask);

  emit('add-task', newTask);

  text.value = '';
  day.value = '';
  reminder.value = false;
};
</script>

<style scoped>
.add-form {
  margin-bottom: 40px;
}

.form-control {
  margin: 20px 0;
}

.form-control label {
  display: block;
}

.form-control input {
  width: 100%;
  height: 40px;
  margin: 5px;
  padding: 3px 7px;
  font-size: 17px;
}

.form-control-check {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.form-control-check label {
  flex: 1;
}

.form-control-check input {
  flex: 2;
  height: 20px;
}
</style>
