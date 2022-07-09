<script setup>
import { ref, defineAsyncComponent } from "vue";

const tasks = ref([]);

const editableTask = ref({
  title: "",
  date: "",
  tags: [],
});

function taskSortLogic(a, b) {
  if (a.completed === b.completed) {
    return a.id - b.id;
  } else if (a.completed) {
    return 1;
  } else {
    return -1;
  }
}

function tasksSort() {
  tasks.value.sort(taskSortLogic);
}

function addTask(task) {
  tasks.value.push({
    id: tasks.value.length + 1,
    title: task.title,
    date: Intl.DateTimeFormat().format(),
    completed: false,
    tags: task.tags,
  });
  tasksSort();
}

function selectEditableTask(task) {
  editableTask.value = task;
}

function updateTask(updatedTask) {
  const task = tasks.value.find((task) => task.id === updatedTask.id);
  task.title = updatedTask.title;
  task.date = updatedTask.date;
  task.tags = updatedTask.tags;
}

function removeTask(id) {
  tasks.value = tasks.value.filter((task) => task.id !== id);
  tasksSort();
}

const TaskUpdateModal = defineAsyncComponent(() =>
  import("@/components/TaskUpdateModal.vue")
);
const TaskCreateModal = defineAsyncComponent(() =>
  import("@/components/TaskCreateModal.vue")
);
</script>

<template>
  <nav>
    <div class="nav-wrapper">
      <a href="#" class="brand-logo center">Task Manager</a>
    </div>
  </nav>

  <div class="container">
    <ul class="collection with-header">
      <li class="collection-header">
        <h4>Tasks</h4>
      </li>
      <li class="collection-item task" v-for="task in tasks" :key="task.id">
        <label class="task-item" :class="{ completed: task.completed }">
          <div>
            <input
              type="checkbox"
              v-model="task.completed"
              @change="tasksSort"
            />
            <span> </span>
          </div>
          <div class="task-item__body">
            <div>
              <p class="task-item__title">{{ task.title }}</p>
              <span>{{ task.date }}</span>
            </div>
            <div class="secondary-content task-item__actions">
              <a
                data-target="updateModal"
                class="waves-effect waves-light btn modal-trigger"
                @click="selectEditableTask(task)"
              >
                <i class="material-icons" id="cedit">edit</i>
              </a>
              <a
                class="waves-effect waves-light btn red"
                @click.prevent="removeTask(task.id)"
              >
                <i class="material-icons cdel" id="cdelete">delete</i>
              </a>
            </div>
          </div>
        </label>
      </li>
      <li v-if="!tasks.length" class="collection-item center-align">
        List of tasks is empty.
      </li>
    </ul>

    <task-create-modal @addTask="addTask" />
    <task-update-modal :task="editableTask" @update="updateTask" />
  </div>
</template>
<style></style>
