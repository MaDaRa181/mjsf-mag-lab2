<template>
  <div>
    <AddTask @add-task="addTask" />
    <hr class="my-4">
    <div class="d-flex justify-content-end align-items-center mb-4 pt-2 pb-3">
      <p class="small mb-0 me-2 text-muted">Filter</p>
      <select class="select" v-model="filter">
        <option value="1">All</option>
        <option value="2">Completed</option>
        <option value="3">Active</option>
        <option value="4">Has due date</option>
      </select>
      <p class="small mb-0 ms-4 me-2 text-muted">Sort</p>
      <select class="select" v-model="sort">
        <option value="1">Added date</option>
        <option value="2">Due date</option>
      </select>
      <a href="#!" style="color: #23af89;" @click="toggleSortDirection" data-mdb-toggle="tooltip" title="Ascending">
        <i :class="sortDirectionClass"></i>
      </a>
    </div>
    <div v-for="task in sortedTasks" :key="task.id">
      <TaskItem 
        :task="task" 
        @toggle-task="toggleTask"
        @delete-task="deleteTask"
      />
    </div>
  </div>
</template>

<script>
import AddTask from './AddTask.vue';
import TaskItem from './TaskItem.vue';

export default {
  components: {
    AddTask,
    TaskItem
  },
  data() {
    return {
      tasks: [
        { id: 1, text: 'Buy groceries for next week', completed: true, createdAt: '2024-05-30T08:00:00'},
        { id: 2, text: 'Renew car insurance', completed: false, createdAt: '2024-05-29T10:00:00', dueDate: '2024-05-31' },
        { id: 3, text: 'Sign up for online course', completed: false, createdAt: '2024-05-28T12:00:00' }
      ],
      filter: '1',
      sort: '1',
      sortDirection: 'desc'
    };
  },
  computed: {
    sortedTasks() {
      let sortedTasks = [...this.tasks];
      if (this.sort === '1') {
        sortedTasks.sort((a, b) => {
          if (this.sortDirection === 'asc') {
            return new Date(a.createdAt) - new Date(b.createdAt);
          } else {
            return new Date(b.createdAt) - new Date(a.createdAt);
          }
        });
      } else if (this.sort === '2') {
        sortedTasks.sort((a, b) => {
          if (this.sortDirection === 'asc') {
            return new Date(a.dueDate || '9999-12-31') - new Date(b.dueDate || '9999-12-31');
          } else {
            return new Date(b.dueDate || '9999-12-31') - new Date(a.dueDate || '9999-12-31');
          }
        });
      }
      if (this.filter === '2') {
        sortedTasks = sortedTasks.filter(task => task.completed);
      } else if (this.filter === '3') {
        sortedTasks = sortedTasks.filter(task => !task.completed);
      } else if (this.filter === '4') {
        sortedTasks = sortedTasks.filter(task => task.dueDate);
      }
      return sortedTasks;
    },
    sortDirectionClass() {
      return this.sortDirection === 'asc' ? 'fas fa-sort-amount-down-alt ms-2' : 'fas fa-sort-amount-up-alt ms-2';
    }
  },
  methods: {
    addTask(newTask) {
      this.tasks.push({
        id: this.tasks.length + 1,
        text: newTask.text,
        completed: false,
        createdAt: new Date().toISOString(),
        dueDate: newTask.dueDate
      });
    },
    toggleTask(id) {
      const task = this.tasks.find(task => task.id === id);
      if (task) {
        task.completed = !task.completed;
      }
    },
    deleteTask(id) {
      this.tasks = this.tasks.filter(task => task.id !== id);
    },
    toggleSortDirection() {
      this.sortDirection = this.sortDirection === 'asc' ? 'desc' : 'asc';
    }
  }
};
</script>
