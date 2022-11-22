<script setup lang="ts">
defineProps<{ choice: string }>();
</script>

<template>
  <div
    :class="`w-40 h-40 flex flex-col justify-center items-center cursor-pointer hover:bg-opacity-50 ${
      choice === 'rock'
        ? 'bg-pink'
        : '' || choice === 'paper'
        ? 'bg-blue'
        : '' || choice === 'scissors'
        ? 'bg-yellow'
        : ''
    }`"
  >
    <img v-if="choice === 'rock'" src="../assets/rock.svg" class="w-20 h-20" />
    <img
      v-else-if="choice === 'scissors'"
      src="../assets/scissors.svg"
      class="w-20 h-20"
    />
    <img v-else src="../assets/paper.svg" class="w-20 h-20" />

    <h1 class="text-white font-extrabold uppercase select-none">
      {{ choice }}
    </h1>
  </div>
</template>
