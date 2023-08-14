<template>
  <AddTask v-show="showAddTask" @add-task="addTask" />
  <Tasks
    @toggle-reminder="toggleReminder"
    @delete-task="deleteTask"
    :tasks="tasks"
  />
</template>

<script>
import Tasks from '../components/Tasks.vue';
import AddTask from '../components/AddTask.vue';

export default {
  name: 'Home',
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
  methods: {
    async addTask(task) {
      let data = await fetch('http://localhost:3000/tasks', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(task),
      });
      data = await data.json();

      this.tasks = [...this.tasks, data];
    },
    async deleteTask(id) {
      // console.log('App level id', id);
      let res;
      if (confirm('Are you sure?')) {
        res = await fetch(`http://localhost:3000/tasks/${id}`, {
          method: 'DELETE',
        });

        res.status === 200
          ? (this.tasks = this.tasks.filter((task) => task.id !== id))
          : alert('There was an error deleting the task.');
      }
    },
    async toggleReminder(id) {
      // console.log('App level id', id);
      const taskToToggle = await this.fetchTask(id);
      const updatedTask = { ...taskToToggle, reminder: !taskToToggle.reminder };

      let data = await fetch(`http://localhost:3000/tasks/${id}`, {
        method: 'PUT',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(updatedTask),
      });
      data = await data.json();

      this.tasks = this.tasks.map((task) => {
        if (task.id === id) return { ...task, reminder: data.reminder };
        else return task;
      });
      /* ternary syntax */
      // this.tasks = this.tasks.map((task) =>
      //   task.id === id ? { ...task, reminder: !data.reminder } : task;
      // );
    },
    async fetchTasks() {
      let data = await fetch('http://localhost:3000/tasks');
      data = await data.json();
      // console.log('data', data);
      return data;
    },
    async fetchTask(id) {
      let data = await fetch(`http://localhost:3000/tasks/${id}`);
      data = await data.json();
      // console.log('data', data);
      return data;
    },
  },
  async created() {
    this.tasks = await this.fetchTasks();
  },
};
</script>
