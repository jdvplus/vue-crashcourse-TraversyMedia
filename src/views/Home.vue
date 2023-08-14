<template>
  <AddTask v-show="showAddTask" @add-task="addTask" />
  <Tasks
    @toggle-reminder="toggleReminder"
    @delete-task="deleteTask"
    :tasks="tasks"
  />
</template>

<script setup>
import Tasks from '../components/Tasks.vue';
import AddTask from '../components/AddTask.vue';
import { ref, onMounted } from 'vue';

// props
defineProps({
  showAddTask: Boolean,
});

// data
const tasks = ref([]);

// methods
const addTask = async (task) => {
  let data = await fetch('http://localhost:3000/tasks', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(task),
  });
  data = await data.json();

  tasks.value = [...tasks.value, data];
};

const deleteTask = async (id) => {
  // console.log('App level id', id);
  let res;
  if (confirm('Are you sure?')) {
    res = await fetch(`http://localhost:3000/tasks/${id}`, {
      method: 'DELETE',
    });

    res.status === 200
      ? (tasks.value = tasks.value.filter((task) => task.id !== id))
      : alert('There was an error deleting the task.');
  }
};

const toggleReminder = async (id) => {
  // console.log('App level id', id);
  const taskToToggle = await fetchTask(id);
  const updatedTask = { ...taskToToggle, reminder: !taskToToggle.reminder };

  let data = await fetch(`http://localhost:3000/tasks/${id}`, {
    method: 'PUT',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(updatedTask),
  });
  data = await data.json();

  tasks.value = tasks.value.map((task) => {
    if (task.id === id) return { ...task, reminder: data.reminder };
    else return task;
  });
  /* ternary syntax */
  // tasks.value = tasks.value.map((task) =>
  //   task.id === id ? { ...task, reminder: !data.reminder } : task;
  // );
};

const fetchTasks = async () => {
  let data = await fetch('http://localhost:3000/tasks');
  data = await data.json();
  // console.log('data', data);
  return data;
};

const fetchTask = async (id) => {
  let data = await fetch(`http://localhost:3000/tasks/${id}`);
  data = await data.json();
  // console.log('data', data);
  return data;
};

// onMounted
onMounted(async () => {
  tasks.value = await fetchTasks();
});
</script>
