<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
export default {
  components: {
  },
};
</script>

<style lang="scss">
  @import '@/scss/_variables.scss';
  @import 'node_modules/bootstrap/scss/bootstrap';
  @import 'node_modules/bootstrap-vue/src/index.scss';

  button {
    margin-left: 8px;
    min-width: 100px;
  }

  p {
    margin: 0;
    padding: 0;
  }

  .column {
    display: flex;
    flex-direction: column;
  }

  .flex {
    display: flex;
}

  .form-control:focus {
      box-shadow: none;
  }

  .status {
      &-default {
          border: 1px solid #ced4da;
          border-radius: 0.25rem;
          padding-left: 21px;
      }

      &-low {
          border-left: $bootstrap-warning-color solid 6px;
          padding-left: 15px;
          &:focus {
              border-left: $bootstrap-warning-color solid 6px;
          }
      }

      &-medium {
          border-left: $orange-color solid 6px;
          padding-left: 15px;
          &:focus {
              border-left: $orange-color solid 6px;
          }
      }

      &-high {
          border-left: $bootstrap-danger-color solid 6px;
          padding-left: 15px;
          &:focus {
              border-left: $bootstrap-danger-color solid 6px;
          }
      }
  }
</style>

<style scoped lang="scss">
</style>
