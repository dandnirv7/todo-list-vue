<template>
  <div class="wrapper mx-5 my-5">
    <div class="list-header">
      <h1>Todo List</h1>
    </div>
    <div class="list-task">
      <div v-if="tasks.length === 0" class="empty-task">
        {{ emptyTask }}
      </div>
      <div
        v-for="item of tasks"
        class="item-task d-flex align-items-start border-bottom pt-3 pb-4"
        :key="item.id"
      >
        <input
          type="checkbox"
          name="status"
          id="task"
          class="me-2 mt-2"
          :checked="item.isDone"
          :style="getChecked(item)"
          @change="updateTaskStatus(item)"
        />
        <div class="d-flex flex-column">
          <div class="title-task mb-1" :style="getTitleStyle(item)">
            {{ item.title }}
          </div>
          <div class="description-task small text-muted">
            {{ item.description }}
          </div>
          <button
            class="btn btn-danger btn-sm me-2 mt-2"
            @click="deleteTask(item.id)"
          >
            Delete
          </button>
        </div>
      </div>
      <div class="action py-2">
        <a href="#" class="add-button" v-if="!isCreating" @click="toggleForm"
          >Add Task</a
        >
        <div class="add-card" v-else>
          <div class="card mb-2 border-0" v-if="isCreating">
            <div class="card-body d-flex flex-column p-0">
              <input
                v-model="titleValue"
                class="form-control border-1 mb-2"
                placeholder="Title"
                type="text"
              />
              <textarea
                v-model="descriptionValue"
                class="form-control border-1 small"
                placeholder="Description"
                rows="3"
              ></textarea>
            </div>
          </div>
          <div class="button-wrapper d-flex">
            <button class="btn btn-primary me-2" @click="saveTask">Save</button>
            <button class="btn btn-outline-secondary" @click="toggleForm">
              Cancel
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import "bootstrap/dist/css/bootstrap.css";
import "bootstrap-vue/dist/bootstrap-vue.css";
export default {
  data() {
    return {
      tasks: [
        {
          id: 1,
          title: "Task 1",
          description: "ini deskripsi",
          isDone: false,
        },
      ],
      isCreating: false,
      isHide: false,
      titleValue: "",
      descriptionValue: "",
      emptyTask: "Belum ada task",
      titleStyle: {
        "text-decoration": "line-through",
      },
      checkboxStyle: {
        "background-color": "#999",
      },
    };
  },
  methods: {
    saveTask() {
      if (
        this.titleValue.trim() === "" ||
        this.descriptionValue.trim() === ""
      ) {
        alert("Title dan Deskripsi harus diisi");
        return;
      }
      this.tasks.unshift({
        id: this.tasks.length + 1,
        title: this.titleValue,
        description: this.descriptionValue,
        isDone: false,
      });
      this.titleValue = "";
      this.descriptionValue = "";
      this.isCreating = false;
      this.isHide = true;
    },
    deleteTask(taskId) {
      if (confirm("Apakah Anda yakin ingin menghapus task ini?")) {
        this.tasks = this.tasks.filter((task) => task.id !== taskId);
        alert("Task berhasil dihapus");
      }
    },
    toggleForm() {
      this.isCreating = !this.isCreating;
      this.isHide = false;
    },
    updateTaskStatus(task) {
      task.isDone = !task.isDone;
    },
    getTitleStyle(item) {
      return item.isDone ? this.titleStyle : "";
    },
    getChecked(item) {
      return item.isDone ? { "accent-color": "gray" } : {};
    },
  },
};
</script>
