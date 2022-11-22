<!-- !contribution :  https://github.com/iamshaunjp/vue-animations/blob/lesson-4/src/components/Toast.vue -->

<script setup lang="ts">
defineProps<{ msg: string }>();
</script>

<template>
  <div class="toast-wrapper">
    <div
      :class="`toast  ${
        msg.includes('win')
          ? 'bg-green'
          : msg.includes('lose')
          ? 'bg-pink'
          : 'bg-purple'
      }`"
    >
      <p>{{ msg }}</p>
    </div>
  </div>
</template>

<style>
.toast-wrapper {
  position: fixed;
  width: 170%;
  top: 90px;
}
.toast {
  padding: 20px;
  color: white;
  border-radius: 10px;
  box-shadow: 1px 3px 5px rgba(0, 0, 0, 0.2);
  max-width: 400px;
  margin: 0 auto;
}
</style>
