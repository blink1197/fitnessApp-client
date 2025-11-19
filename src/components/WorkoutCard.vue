<template>
  <div class="card w-100 h-100 shadow-sm workout-card position-relative">
    <span class="badge position-absolute top-0 end-0 m-2"
      :class="status === 'completed' ? 'bg-success' : 'bg-warning text-dark'">
      {{ status }}
    </span>

    <div class="card-body d-flex flex-column justify-content-between">
      <div>
        <h5 class="card-title mb-2">{{ name }}</h5>
        <p class="card-text text-muted mb-3">
          Duration: <strong>{{ duration }} mins</strong>
        </p>
      </div>

      <div class="d-flex gap-2 mt-auto">
        <button class="btn btn-outline-primary btn-sm w-100" @click="openModal">
          Update
        </button>

        <button v-if="status === 'pending'" class="btn btn-outline-success btn-sm w-100" @click="completeWorkout(id)">
          Complete
        </button>

        <button class="btn btn-outline-danger btn-sm w-100" @click="deleteWorkOut(id)">
          Delete
        </button>
      </div>
    </div>

    <UpdateWorkoutModal ref="modalRef" @save="onSave" :workoutId="id" />
  </div>
</template>

<script setup>
import { useWorkoutStore } from '@/stores/workout';
import { Notyf } from 'notyf';
import { reactive, ref, watch } from 'vue';
import UpdateWorkoutModal from './UpdateWorkoutModal.vue';

const props = defineProps({
  name: String,
  duration: String,
  id: String,
  status: { type: String, default: 'pending' }
});

const workout = reactive({
  id: props.id,
  name: props.name,
  duration: props.duration,
  status: props.status
});

// Keep workout in sync with props
watch(
  () => [props.name, props.duration, props.status],
  ([name, duration, status]) => {
    workout.name = name;
    workout.duration = duration;
    workout.status = status;
  }
);

const modalRef = ref(null);
const notyf = new Notyf();
const workoutStore = useWorkoutStore();

const openModal = () => modalRef.value.open();

const onSave = async (updatedWorkout) => {
  try {
    await workoutStore.updateWorkout(props.id, updatedWorkout);
    notyf.success('Workout updated successfully');
  } catch (err) {
    console.error(err);
    notyf.error('Failed to update workout');
  }
};

const deleteWorkOut = async (id) => {
  const result = await workoutStore.deleteWorkout(id);
  if (result.success) notyf.success('Workout deleted successfully.');
  else notyf.error('Something went wrong.');
};

const completeWorkout = async (id) => {
  const result = await workoutStore.completeWorkout(id);
  if (result.success) notyf.success('Workout completed.');
  else notyf.error('Something went wrong.');
};
</script>
