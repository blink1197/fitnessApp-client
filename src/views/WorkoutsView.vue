<template>
  <div class="page-wrapper position-relative vh-100 d-flex flex-column align-items-center pt-5 mx-auto">

    <!-- Logo -->
    <img src="/logo.png" alt="Logo" class="mb-4" style="width: 120px; height: auto;" />
    <!-- Logout Button -->
    <button class="btn btn-outline-danger position-absolute top-0 end-0 m-3" @click="logout">
      Logout
    </button>

    <!-- Open Modal Button -->
    <button class="btn btn-primary mt-5 w-75 d-flex justify-content-center align-items-center" @click="openModal"
      :disabled="isLoading">
      <i class="bi bi-plus fs-3"></i>
      Add Workout
    </button>

    <h2 class="my-4">Your Workouts</h2>

    <div class="w-100 px-3">

      <!-- Loading State -->
      <div v-if="isLoading" class="text-center py-5 text-muted">
        <div class="spinner-border text-primary" role="status">
          <span class="visually-hidden">Loading...</span>
        </div>
        <p class="mt-3">Fetching your workouts...</p>
      </div>

      <!-- Empty State -->
      <div v-else-if="!workoutStore.workouts.length" class="text-center py-5 text-muted">
        <i class="bi bi-calendar-x fs-1 mb-3"></i>
        <p class="mb-0">No workouts yet.</p>
        <small>Add a workout to get started!</small>
      </div>

      <!-- Workouts List -->
      <div v-else class="row g-3">
        <div class="col-12" v-for="w in workoutStore.workouts" :key="w._id">
          <WorkoutCard :name="w.name" :duration="w.duration" :id="w._id" :status="w.status" />
        </div>
      </div>

    </div>

    <!-- Workout Modal Component -->
    <AddWorkoutModal ref="workoutModalRef" @save="saveWorkout" />
  </div>
</template>

<script setup>
import AddWorkoutModal from '@/components/AddWorkoutModal.vue';
import WorkoutCard from '@/components/WorkoutCard.vue';
import { useUserStore } from '@/stores/user';
import { useWorkoutStore } from '@/stores/workout';
import { Notyf } from 'notyf';
import { onBeforeMount, ref } from 'vue';
import { useRouter } from 'vue-router';

const notyf = new Notyf();
const router = useRouter();
const workoutStore = useWorkoutStore();
const userStore = useUserStore();
const { logout } = userStore

const workoutModalRef = ref(null);
const isLoading = ref(false);

const openModal = () => {
  workoutModalRef.value.open();
};

// Save workout from modal
const saveWorkout = async (newWorkout) => {
  const result = await workoutStore.addWorkout({ name: newWorkout.name, duration: Number(newWorkout.duration) });
  if (result.success) {
    notyf.success('Workout added successfully.');
  } else {
    notyf.error('Something went wrong.');
  }
};


// Fetch workouts
onBeforeMount(async () => {
  if (!userStore.isLoggedIn) {
    router.push('/login');
    return;
  }

  isLoading.value = true;
  try {
    await workoutStore.fetchWorkouts();
  } catch (error) {
    console.error(error)
    notyf.error('Failed to fetch workouts.');
  } finally {
    isLoading.value = false;
  }
});
</script>

<style scoped>
.page-wrapper {
  max-width: 600px;
}

.empty-state i {
  font-size: 3rem;
  color: #adb5bd;
}
</style>
