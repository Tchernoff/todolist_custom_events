<template>
  <div class="notes-item__wrapper">
    <div class="notes-item row">
      <div class="col-3">
        <p>created: {{created}}</p>
        <p
          v-if="lastEdit.length > 0">
          updated: {{lastEdit}}
        </p>
      </div>
      <input
        class="notes-item__input form-control col-5"
        ref="input"
        v-bind:class="[{ready: readyStatus}, priorityStatus]"
        type="test"
        v-model="todoTitle"
        :readonly="isDisabled"
        @click="startEdit"
        @input="toggleSaveButton = true"
        @keyup.enter="saveEdit"
      />
      <div class="notes-item__control">
        <button
          class="btn btn-outline-success"
          v-if="!readyStatus"
          @click="toggleStatus">
          Done
        </button>
        <button
          class="btn btn-outline-success"
          v-else
          @click="toggleStatus">
          Undone
        </button>
      </div>
    </div>
    <div class="row">
      <div class="col-3"></div>
      <b-collapse class="col-5 notes-item__collapse" v-model="collapseVisible">
        <b-card>
          <div class="flex">
            <div class="col-8">
              Description
              <b-form-textarea
              @input="toggleSaveButton = true"
              v-model="todoDescription"
              placeholder="Description..."
              class="my-2"
              rows="7"
              max-rows="8">
              </b-form-textarea>
            </div>
            <div class="col-4 column">
              Priority
              <button @click="setPriority(3)" class="btn btn-danger">
                High
              </button>
              <button @click="setPriority(2)" class="btn btn-orange">
                Medium
              </button>
              <button @click="setPriority(1)" class="btn btn-warning">
                Low
              </button>
              <button @click="setPriority(0)" class="btn btn-light">
                None
              </button>
            </div>
          </div>
          <div class="my-2 col-12">
            <button
              class="btn btn-outline-primary"
              v-if="this.toggleSaveButton"
              @click="saveEdit">
              Save
            </button>
            <button
              v-else
              class="btn btn-outline-primary"
              @click="saveEdit">
              Close
            </button>
            <button
              @click="isModalVisible = true"
              class=" ml-2 btn btn-outline-danger">
              Delete
            </button>
          </div>
        </b-card>
      </b-collapse>
      <div></div>
    </div>
    <b-modal class="modal" ref="my-modal" hide-footer hide-header v-model="isModalVisible">
      <div class="modal__content">
        <h4>You really want to delete this note?</h4>
      </div>
      <div class="modal__control">
        <b-button
          variant="outline-warning"
          @click="hideModal">
          NO GOD!! PLEASE NO!!
        </b-button>
        <b-button
          variant="outline-danger"
          @click="deleteItem">
          Yes
        </b-button>
      </div>
    </b-modal>
  </div>
</template>

<script>
import getFullDate from '../helpers';

export default {
  props: {
    message: {
      message: String,
      required: true,
    },
    isReady: {
      type: Boolean,
      required: true,
    },
    id: {
      type: Number,
      required: true,
    },
    created: {
      type: String,
      required: true,
    },
    lastEdit: {
      type: String,
      required: true,
    },
    priority: {
      type: String,
      required: true,
    },
    sortingPriority: {
      type: Number,
      required: true,
    },
    description: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      itemID: this.id.toString(),
      todoTitle: this.message,
      editingTodo: false,
      readyStatus: this.isReady,
      isModalVisible: false,
      collapseVisible: false,
      todoDescription: this.description,
      priorityStatus: this.priority,
      sortPriority: this.sortingPriority,
      toggleSaveButton: false,
    };
  },
  methods: {
    startEdit() {
      if (this.editingTodo === true) {
        this.editingTodo = false;
      }
      this.editingTodo = true;
      this.$nextTick(() => {
        this.$refs.input.focus();
      });
      this.$nextTick(() => {
        this.collapseVisible = true;
      });
    },
    saveEdit() {
      if (this.message === this.todoTitle
      && this.priorityStatus === this.priority
      && this.description === this.todoDescription) {
        this.collapseVisible = false;
        return;
      }
      if (this.todoTitle.length === 0) {
        this.isModalVisible = true;
        return;
      }
      this.$emit('editTitle', this.todoTitle);
      this.$emit('update', getFullDate(this.id));
      this.$emit('editDescription', this.todoDescription);
      this.$emit('changePriority', this.priorityStatus);
      this.$emit('changeSortingPriority', this.sortPriority);
      this.collapseVisible = false;
      this.toggleSaveButton = false;
    },
    setPriority(status) {
      this.toggleSaveButton = true;
      switch (status) {
        case 0:
        default:
          this.priorityStatus = 'status-default';
          this.sortPriority = 0;
          break;
        case 1:
          this.priorityStatus = 'status-low';
          this.sortPriority = 1;
          break;
        case 2:
          this.priorityStatus = 'status-medium';
          this.sortPriority = 2;
          break;
        case 3:
          this.priorityStatus = 'status-high';
          this.sortPriority = 3;
          break;
      }
    },
    toggleStatus() {
      if (!this.readyStatus) {
        this.saveEdit();
      }
      this.readyStatus = !this.readyStatus;
    },
    deleteItem() {
      this.$emit('delete', this.id);
      this.$refs['my-modal'].hide();
    },
    hideModal() {
      if (this.todoTitle.length === 0) {
        this.todoTitle = this.message;
      }
      this.$refs['my-modal'].hide();
    },
    toggleCollapse() {
      this.collapseVisible = !this.collapseVisible;
    },
    isChanged() {
      if (this.todoTitle !== this.message
      || this.todoDescription !== this.description
      || this.priorityStatus !== this.priority) {
        this.toggleSaveButton = true;
      }
    },
  },
  computed: {
    isDisabled() {
      return !this.editingTodo;
    },
  },
};
</script>

<style scoped lang="scss">
  .ready {
    text-decoration: line-through;
    background-color: #e9ecef !important;
  }

  .card {
    border-top-left-radius: 0;
    border-top-right-radius: 0;
    margin-top: -3px;
  }

  .card-body {
    display: flex;
    flex-direction: column;
    padding: 15px 0;

      button {
        max-width: 50px;
        margin: 10px 0 0 0;
      }
  }

  .notes-item {
    &__input {
      z-index: 1 !important;

      &[readonly] {
        background-color: white;
      }
    }

    &__collapse {
      padding: 0;
    }

    &__wrapper {
      margin-bottom: 10px;
    }
  }

  p {
    font-size: 13px;
  }

  .modal {
    &__content {
      padding: 30px 10px;
    }

    &__control {
      padding: 10px 10px;
      display: flex;
      align-items: center;
    }
  }
</style>
