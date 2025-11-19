<template>
  <div class="modal fade" tabindex="-1" ref="modalRef">
    <div class="modal-dialog modal-dialog-centered">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title">{{ title }}</h5>
          <button type="button" class="btn-close" @click="close"></button>
        </div>

        <div class="modal-body">
          <form @submit.prevent="save">
            <!-- Name -->
            <div class="mb-3">
              <label class="form-label">Workout Name</label>
              <input type="text" class="form-control" v-model="form.name" required />
            </div>

            <!-- Duration -->
            <div class="mb-3">
              <label class="form-label">Duration (minutes)</label>
              <input type="number" class="form-control" v-model="form.duration" min="1" required />
            </div>

            <!-- Status Dropdown -->
            <div class="mb-3">
              <label class="form-label">Status</label>
              <select class="form-select" v-model="form.status">
                <option value="pending">Pending</option>
                <option value="completed">Completed</option>
              </select>
            </div>

            <p v-if="errorMessage" class="text-danger small">{{ errorMessage }}</p>
          </form>
        </div>

        <div class="modal-footer d-flex justify-content-center">
          <button type="button" class="btn btn-secondary" @click="close">Cancel</button>
          <button type="button" class="btn btn-primary" @click="save">Save Workout</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import bootstrap from 'bootstrap/dist/js/bootstrap.bundle';
import { onMounted, ref, watch } from 'vue';

const props = defineProps({
  workout: Object,
  title: { type: String, default: 'Update Workout' }
});

const emit = defineEmits(['save']);

const form = ref({
  name: '',
  duration: '',
  status: 'pending'
});
const errorMessage = ref('');
const modalRef = ref(null);
let modalInstance = null;

// Initialize Bootstrap modal
onMounted(() => {
  modalInstance = new bootstrap.Modal(modalRef.value, { backdrop: 'static' });
});

// Populate form initially and whenever props.workout changes
watch(
  () => props.workout,
  (newWorkout) => {
    form.value.name = newWorkout?.name || '';
    form.value.duration = newWorkout?.duration || '';
    form.value.status = newWorkout?.status || 'pending';
    errorMessage.value = '';
  },
  { immediate: true }
);

const open = () => modalInstance.show();
const close = () => modalInstance.hide();

const save = () => {
  if (!form.value.name || !form.value.duration) {
    errorMessage.value = 'Please fill all fields';
    return;
  }

  emit('save', { ...props.workout, ...form.value });
  errorMessage.value = '';
  close();
};

defineExpose({ open, close });
</script>
