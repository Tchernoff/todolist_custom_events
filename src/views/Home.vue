<template>
  <div class="page">
    <h1 class="page__title"><span class="primary">Vue.js</span> To Do List</h1>
    <h4 class="page__title text-muted">with custom events Child => Parent</h4>
    <NotesApp></NotesApp>
  </div>
</template>

<script>
import NotesApp from '@/components/NotesApp.vue';

export default {
  name: 'Home',
  components: {
    NotesApp,
  },
};
</script>

<style lang="scss">
  @import "@/scss/_variables.scss";

  .page {

    &__title {
      text-align: center;
      margin: 0 auto;

      span {
        color: $base-color;
      }
    }
  }
</style>
