<template>
  <div class="min-h-screen bg-gray-100 flex items-center justify-center">
    <TaskList />
  </div>
</template>

<script>
import TaskList from './components/TaskList.vue';

export default {
  name: 'App',
  components: {
    TaskList,
  },
};
</script>

<style>
/* Global styles */
</style>