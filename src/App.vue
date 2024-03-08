<script setup lang="ts">
import Menu from './components/Menu.vue';
import ImageArea from './components/ImageArea.vue';
import Navi from './components/Navi.vue';
import { ref } from 'vue';

let file: File | undefined = undefined;
let isImageExsist = ref(false);
const src = ref<string>()

function onUpload(f: File) {
    file = f;
    src.value = URL.createObjectURL(file);
}
</script>

<template>
    <div class="container">
        <div class="menuarea">
            <Menu @onUpload="onUpload"></Menu>
        </div>
        <div class="imagearea">
            a
            <ImageArea :src="src" v-if="isImageExsist"></ImageArea>
        </div>
        <div class="naviarea">
            <Navi></Navi>
        </div>
    </div>
</template>

<style scoped>
.menuarea {
    background-color: whitesmoke;
    width: 100%;
    height: 20%;
}
.imagearea {
    background-color: whitesmoke;
    width: 100%;
    height: 70%;
}
.naviarea {
    width: 100%;
    height: 10%;
}

</style>