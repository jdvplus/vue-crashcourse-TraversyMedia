<template>
  <header>
    <h1>{{ title }}</h1>
    <Button
      v-show="homePage"
      @btnClick="$emit('toggle-add-task')"
      :text="showAddTask ? 'Close' : 'Add task'"
      :color="showAddTask ? 'mediumaquamarine' : 'dodgerblue'"
      fontColor="black"
    />
    <!-- <Button text="Update task" color="blue" /> -->
    <!-- <Button text="Delete task" color="red" /> -->
  </header>
</template>

<script>
import Button from './Button.vue';

export default {
  name: 'Header',
  props: {
    title: String,
    showAddTask: Boolean,
  },
  components: {
    Button,
  },
  computed: {
    homePage() {
      if (this.$route.path === '/') return true;
      else return false;
    },
  },
};
</script>

<style scoped>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}
</style>
