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
  </header>
</template>

<script setup>
import Button from './Button.vue';
import { computed } from 'vue';
import { useRoute } from 'vue-router';

// props
defineProps({
  title: String,
  showAddTask: Boolean,
});

// computed
const homePage = computed(() => {
  const route = useRoute();
  if (route.path === '/') return true;
  else return false;
});
</script>

<style scoped>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}
</style>
