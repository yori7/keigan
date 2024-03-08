<script setup lang="ts">
import { ref } from 'vue';
const emit = defineEmits<{
    (e: "onUpload", f: File): void
}>()
const openButton = ref<HTMLInputElement>()

function onOpen(_: MouseEvent) {
    (openButton.value as HTMLInputElement).click();
}

function onFileUpload(event: Event) {
  const el = event.target as HTMLInputElement;
  let imgFile = el.files != null? el.files[0] : undefined;
  if (imgFile != undefined) {
    emit("onUpload", imgFile);
  }
}
</script>

<template>
<div class="fileMenuArea">
    <div class="menuBlock">
        <img src="../../assets/testimg.png" class="menuImage" @click="onOpen" draggable="false">
        <input ref="openButton" type="file" accept="image/*" @change="onFileUpload" hidden/>
    </div>
    <div class="menuBlock">
        <img src="../../assets/testimg.png" class="menuImage" @click="onOpen" draggable="false">
        <input ref="openButton" type="file" accept="image/*" @change="onFileUpload" hidden/>
    </div>
</div>
</template>

<style scoped>
.fileMenuArea {
    width: 100%;
    height: 100%;
    text-align: left;
}
.menuBlock {
    display: inline-block;
    width: fit-content;
    height: 100%;
    text-align: center;
}
.menuBlocks {
    border-left: 1px solid rgb(201, 201, 201);
}
.menuImage {
    height: calc(100% - 10px);
    margin: 5px 15px;
}
</style>