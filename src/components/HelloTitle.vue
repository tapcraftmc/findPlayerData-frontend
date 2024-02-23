<script setup lang="ts">
defineProps<{ msg: string }>();
</script>

<template>
  <h1 color="$ep-color-primary">{{ msg }}</h1>

  <p>
    Find player data from Essentials\userdata
  </p>
</template>

<style>
.ep-button {
  margin: 4px;
}
.ep-button + .ep-button {
  margin-left: 0;
  margin: 4px;
}
</style>
