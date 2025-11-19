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
            <div class="mb-3">
              <label class="form-label">Workout Name</label>
              <input type="text" class="form-control" v-model="form.name" placeholder="Enter workout name" required />
            </div>

            <div class="mb-3">
              <label class="form-label">Duration (minutes)</label>
              <input type="number" class="form-control" v-model="form.duration" placeholder="e.g. 30" min="1"
                required />
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
  workout: { type: Object, default: () => ({ name: '', duration: '' }) },
  title: { type: String, default: 'Add Workout' }
});

const emit = defineEmits(['save']);

const form = ref({ name: '', duration: '' });
const errorMessage = ref('');
const modalRef = ref(null);
let modalInstance = null;

onMounted(() => {
  modalInstance = new bootstrap.Modal(modalRef.value, { backdrop: 'static' });
});

// Update form when workout prop changes
watch(() => props.workout, (newWorkout) => {
  form.value.name = newWorkout.name || '';
  form.value.duration = newWorkout.duration || '';
  errorMessage.value = '';
}, { immediate: true });

// Expose methods to open/close modal
const open = () => modalInstance.show();
const close = () => modalInstance.hide();

const save = () => {
  if (!form.value.name.trim() || !form.value.duration) {
    errorMessage.value = 'Please fill in all fields.';
    return;
  }
  emit('save', { ...props.workout, ...form.value });

  close();

  form.value = { name: '', duration: '' };
  errorMessage.value = '';
};

defineExpose({ open, close });
</script>
