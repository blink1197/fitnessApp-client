<script setup>
import { useRouter } from 'vue-router';
const router = useRouter();

const goHome = () => {
  router.push('/');
};
</script>

<template>
  <main class="d-flex flex-column justify-content-center align-items-center vh-100 text-center px-3">

    <div class="d-flex justify-content-center">
      <img src="/logo.png" alt="Logo" class="mb-4 mx-auto img-fluid" style="width: 120px; height: auto;" />
    </div>

    <!-- Heading -->
    <h1 class="mb-2">404 - Page Not Found</h1>

    <!-- Subtitle -->
    <p class="mb-4 text-muted">Oops! The page you’re looking for doesn’t exist.</p>

    <!-- Back Home Button -->
    <button class="btn btn-primary btn-lg" @click="goHome">
      Go Home
    </button>
  </main>
</template>

<style scoped>
main {
  max-width: 500px;
  margin: 0 auto;
}
</style>
