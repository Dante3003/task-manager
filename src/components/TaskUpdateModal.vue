<script setup>
import { defineProps, ref, defineEmits, onMounted, watch } from "vue";

const props = defineProps({
  task: {
    type: Object,
    required: true,
  },
});

const emit = defineEmits(["update", "cancel"]);

let tagsInstance = null;
const editableTask = ref(JSON.parse(JSON.stringify(props.task)));

watch(
  () => props.task,
  (newValue) => {
    updateValue(newValue);
  }
);

onMounted(() => {
  const editableTaskChip = document.getElementById("task-tags");
  const editModal = document.getElementById("updateModal");
  window?.M.Modal.init(editModal, {});
  window?.M.Chips.init(editableTaskChip, {
    onChipAdd: (e, chip) => {
      editableTask.value.tags.push({
        tag: chip.childNodes[0].textContent,
      });
    },
    onChipDelete: (e, chip) => {
      editableTask.value.tags = editableTask.value.tags.filter(
        (tag) => tag.tag !== chip.childNodes[0].textContent
      );
    },
  });
  tagsInstance = window?.M.Chips.getInstance(editableTaskChip);
  editableTask.value.tags.forEach((tag) => {
    tagsInstance.addChip(tag);
  });
});

function update() {
  emit("update", editableTask.value);
}
function updateValue(newValue) {
  editableTask.value = JSON.parse(JSON.stringify(newValue));
  editableTask.value.tags.forEach((tag) => {
    tagsInstance.addChip(tag);
  });
}
</script>

<template>
  <div id="updateModal" class="modal">
    <div class="modal-content">
      <div class="row">
        <div class="input-field col s12">
          <input
            placeholder="Task title"
            id="tast-title"
            type="text"
            class="validate"
            v-model="editableTask.title"
          />
          <label class="active" for="tast-title">Title</label>
        </div>
      </div>
      <div class="row">
        <div class="col s12">
          <p class="my-0">Set initial tags.</p>
          <div class="chips my-0" id="task-tags"></div>
        </div>
      </div>
    </div>
    <div class="modal-footer" id="modalclick">
      <a
        href="#!"
        class="modal-close waves-effect waves-green btn-flat"
        id="addclick"
        @click="update"
      >
        Update
      </a>
      <a href="#!" class="modal-close waves-effect waves-light btn-flat">
        Cancel
      </a>
    </div>
  </div>
</template>
