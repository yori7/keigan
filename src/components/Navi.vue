<script setup lang="ts">


function onClick(event: Event) {
  console.log(event.target);
}
</script>

<template>
<div class="navigateArea">
  <button @click="onClick">button</button>
</div>
</template>

<style scoped>
.navigateArea {
  height: 100%;
  width: 100%;
}
</style>