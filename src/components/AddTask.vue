<template>
  <div class="pb-2">
    <div class="card">
      <div class="card-body">
        <div class="d-flex flex-row align-items-center">
          <input 
            type="text" 
            class="form-control form-control-lg" 
            v-model="newTask" 
            placeholder="Add new..." 
            @keyup.enter="addTask"
          />
          <a href="#!" data-mdb-toggle="tooltip" title="Set due date" @click="toggleDueDate">
            <i class="fas fa-calendar-alt fa-lg me-3"></i>
          </a>
          <div v-if="showDueDate">
            <input 
              type="date" 
              class="form-control" 
              v-model="dueDate" 
              @change="setDueDate"
            />
          </div>
          <div>
            <button type="button" class="btn btn-primary" @click="addTask">Add</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTask: '',
      showDueDate: false,
      dueDate: ''
    };
  },
  methods: {
    addTask() {
      if (this.newTask.trim()) {
        this.$emit('add-task', { text: this.newTask, dueDate: this.dueDate });
        this.newTask = '';
        this.dueDate = '';
        this.showDueDate = false;
      }
    },
    toggleDueDate() {
      this.showDueDate = !this.showDueDate;
    },
    setDueDate(event) {
      this.dueDate = event.target.value;
    },
    formatDate(dateString) {
      const options = { year: 'numeric', month: 'short', day: 'numeric' };
      const date = new Date(dateString);
      return date.toLocaleDateString('en-US', options);
    }
  }
};
</script>
