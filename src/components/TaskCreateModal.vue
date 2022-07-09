<script setup>
import { ref, defineEmits, onMounted } from "vue";

const emit = defineEmits(["addTask", "cancel"]);

const newTask = ref({
  title: "",
  date: "",
  tags: [],
});

onMounted(() => {
  const newTaskChip = document.getElementById("newTaskTags");
  const createModal = document.getElementById("createModal");
  window.M.Modal.init(createModal, {});
  window?.M.Chips.init(newTaskChip, {
    onChipAdd: (e, chip) => {
      newTask.value.tags.push({
        tag: chip.childNodes[0].textContent,
      });
    },
    onChipDelete: (e, chip) => {
      newTask.value.tags = newTask.value.tags.filter(
        (tag) => tag.tag !== chip.childNodes[0].textContent
      );
    },
  });
});

function createTask() {
  emit("addTask", newTask.value);
}
</script>
<template>
  <p
    data-target="createModal"
    class="btn-floating btn-large waves-effect waves-light red right modal-trigger"
    id="btnadd"
  >
    <i class="material-icons">add</i>
  </p>

  <div id="createModal" class="modal">
    <div class="modal-content">
      <div class="row">
        <div class="input-field col s12">
          <input
            placeholder="Task title"
            id="tast-title"
            type="text"
            class="validate"
            v-model="newTask.title"
          />
          <label class="active" for="tast-title">Title</label>
        </div>
      </div>
      <div class="row">
        <div class="col s12">
          <p class="my-0">Set initial tags.</p>
          <div class="chips my-0" id="newTaskTags"></div>
        </div>
      </div>
    </div>
    <div class="modal-footer" id="modalclick">
      <a
        href="#!"
        class="modal-close waves-effect waves-green btn-flat"
        id="addclick"
        @click="createTask"
      >
        Add
      </a>
      <a href="#!" class="modal-close waves-effect waves-red btn-flat">
        Cancel
      </a>
    </div>
  </div>
</template>
