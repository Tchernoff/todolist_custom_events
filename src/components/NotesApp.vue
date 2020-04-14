
<template>
  <div class="container notes-app">
    <div class="notes-app__form row">
      <p class="col-3">Add your note:</p>
      <input
        class="form-control col-5"
        @keyup.enter="saveUserInput"
        type="text"
        v-model="userInput" />
        <div class="control">
          <button class="btn btn-outline-primary" @click="saveUserInput">Add</button>
        </div>
    </div>
    <div class="notes-app__list">
      <NotesItem
        v-for="item in sortedTodoList"
        :created="item.created"
        :message="item.message"
        :description="item.description"
        :isReady="item.isReady"
        :id="item.id"
        :key="-item.id"
        :lastEdit="item.lastEdit"
        :priority="item.priority"
        :sortingPriority="item.sortingPriority"
        @editTitle="item.message = $event"
        @editDescription="item.description = $event"
        @update="item.lastEdit = $event"
        @changePriority="item.priority = $event"
        @changeSortingPriority="item.sortingPriority = $event"
        @delete="deleteTodo">
      </NotesItem>
    </div>
  </div>
</template>

<script>
import NotesItem from './NotesItem.vue';
import getFullDate from '../helpers';

export default {
  data() {
    return {
      userInput: '',
      todoList: [],
      editingTodo: false,
    };
  },
  components: {
    NotesItem,
  },
  methods: {
    saveUserInput() {
      if (this.userInput.trim().length === 0) {
        // eslint-disable-next-line no-alert
        alert('u need to write something');
        return;
      }
      this.todoList.splice(0, 0, {
        id: Date.now(),
        created: getFullDate(),
        message: this.userInput.trim(),
        isReady: false,
        lastEdit: '',
        priority: 'status-default',
        sortingPriority: 0,
        description: '',
      });
      this.userInput = '';
    },
    deleteTodo(id) {
      this.todoList.forEach((item) => {
        if (item.id === id) {
          this.todoList.splice(this.todoList.indexOf(item), 1);
        }
      });
    },
  },
  computed: {
    sortedTodoList() {
      return this.todoList.slice(0).sort((a, b) => b.sortingPriority - a.sortingPriority);
    },
  },
};
</script>

<style scoped lang="scss">
  .notes-app {
    margin-top: 32px;

    input {
      padding-left: 21px;
    }

    &__form {
      height: 50px;
    }
  }
</style>
