<template>
  <div>
    <div class="container">
      <h1>To Do List</h1>
      <div class="mb-3">
        <input
          type="text"
          v-model="newTask"
          class="form-control"
          placeholder="Enter a new task"
        />
        <button @click="addTask" class="btn btn-primary mt-2">Add Task</button>
      </div>

      <div class="mb-3">
        <div class="task-list">
          <div v-for="(task, index) in tasks" :key="index" class="task-item">
            <h5>{{ task.title }}</h5>
            <p>{{ task.content }}</p>
            <div>
              <button @click="editTask(index)" class="btn btn-primary btn-sm">
                Edit
              </button>
              <button
                @click="toggleTaskCompletion(index)"
                class="btn btn-success btn-sm"
              >
                {{ task.completed ? "Mark Incomplete" : "Mark Complete" }}
              </button>
              <button @click="deleteTask(index)" class="btn btn-danger btn-sm">
                Delete
              </button>
            </div>
          </div>
        </div>
        <div v-if="tasks.length === 0" class="alert alert-info">
          No tasks found.
        </div>
      </div>

      <div class="alert alert-info">
        Number of not completed tasks: {{ getNotCompletedTasksCount() }}
      </div>
      <div
        v-if="editingTaskIndex !== null"
        class="modal fade show d-block"
        style="background-color: rgba(0, 0, 0, 0.5)"
      >
        <div class="modal-dialog">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title">Edit Task Details</h5>
              <button @click="closeModal" type="button" class="close">
                <span>&times;</span>
              </button>
            </div>
            <div class="modal-body">
              <div class="form-group">
                <label>Title:</label>
                <input
                  type="text"
                  v-model="editedTask.title"
                  class="form-control"
                />
              </div>
              <div class="form-group">
                <label>Content:</label>
                <textarea
                  v-model="editedTask.content"
                  class="form-control"
                ></textarea>
              </div>
            </div>
            <div class="modal-footer">
              <button @click="saveChanges" class="btn btn-primary">
                Save Changes
              </button>
            </div>
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
      newTask: "",
      tasks: [],
      editingTaskIndex: null,
      editedTask: { title: "", content: "" },
    };
  },

  methods: {
    addTask() {
      if (this.newTask.trim() !== "") {
        this.tasks.push({ title: this.newTask, content: "", completed: false });
        this.newTask = "";
      }
    },
    editTask(index) {
      this.editingTaskIndex = index;
      this.editedTask = {
        title: this.tasks[index].title,
        content: this.tasks[index].content,
      };
    },
    saveChanges() {
      this.tasks[this.editingTaskIndex].title = this.editedTask.title;
      this.tasks[this.editingTaskIndex].content = this.editedTask.content;
      this.closeModal();
    },
    closeModal() {
      this.editingTaskIndex = null;
      this.editedTask = { title: "", content: "" };
    },
    toggleTaskCompletion(index) {
      this.tasks[index].completed = !this.tasks[index].completed;
    },
    deleteTask(index) {
      if (confirm("Are you sure you want to delete this task?")) {
        this.tasks.splice(index, 1);
      }
    },
    getNotCompletedTasksCount() {
      return this.tasks.filter((task) => !task.completed).length;
    },
  },
};
</script>

<style>
</style>