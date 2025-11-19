<script setup>
import { useRouter } from 'vue-router';
const router = useRouter();

const goToWorkouts = () => {
  router.push('/workouts');
};
</script>

<template>
  <main class="d-flex flex-column justify-content-center align-items-center vh-100 text-center px-3">
    <!-- Logo -->
    <img src="/logo.png" alt="Logo" class="mb-4" style="width: 120px; height: auto;" />

    <!-- Title -->
    <h1 class="mb-2">Fitness Tracker</h1>

    <!-- Subtitle -->
    <p class="mb-4 text-muted">Track your workouts and progress with ease</p>

    <!-- Button -->
    <button class="btn btn-primary btn-lg" @click="goToWorkouts">
      Go to Workouts
    </button>
  </main>
</template>

<style scoped>
main {
  max-width: 500px;
  margin: 0 auto;
}
</style>
