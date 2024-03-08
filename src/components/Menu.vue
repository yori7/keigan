<script setup lang="ts">
import FileMenu from './menuComponents/FileMenu.vue';

const emit = defineEmits<{
    (e: "onUpload", f: File): void
}>()

function onFileUpload(imgFile: File) {
    emit("onUpload", imgFile);
}
</script>

<template>
<div class="menuArea">
    <div data-tauri-drag-region class="menutab">
        <img src="../assets/vue.svg" class="icon">
        <button>File</button>
        <button>Measure</button>
        <button>Edit</button>
    </div>
    <div class="menuContentArea">
        <FileMenu @on-upload="onFileUpload"></FileMenu>

    </div>
</div>
</template>

<style scoped>
.menuArea {
    width: 100%;
    height: 100%;
}
.menutab {
    width: 100%;
    height: 2em;
    display: flex;
}
.icon {
    margin: 4px 4px 4px 6px;
    object-fit: contain;
}
button {
    color: rgb(14, 14, 14);
    border-color: transparent;
    background-color: transparent   ;
    margin: 0 3px;
    border-radius: 10px;
}
button:hover {
    background-color: rgb(229, 230, 229);
}
.menuContentArea {
    width: 99%;
    height: calc(100% - 2em);
    border-radius: 10px;
    background-color: rgb(235, 235, 235);
}
</style>