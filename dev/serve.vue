<script>
import { defineComponent } from 'vue';
import Vue3laravelEasyPagination from '@/vue3laravel-easy-pagination.vue';

export default defineComponent({
  name: 'ServeDev',
  components: {
    Vue3laravelEasyPagination
  }
});
</script>

<template>
  <div id="app">
    <vue3laravel-easy-pagination />
  </div>
</template>
