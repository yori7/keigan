<script setup lang="ts">
import { onBeforeUpdate, ref } from 'vue';

const props = defineProps({
    src: String
})
const centerimage = ref<HTMLImageElement>();
onBeforeUpdate(() => {
    console.log("src update");
    (centerimage.value as HTMLImageElement).src = props.src as string;
})
const scale = ref(100);
let cursolY = 0;
let cursolX = 0;
let dx = ref(0);
let dy = ref(0);

// ------------------------------------------
// 拡大縮小関連の処理
// タッチスクリーン
function onTouchmove(event: TouchEvent) {
  event.preventDefault()
  var touches = event.changedTouches;
  if (touches.length == 2) {
    // 座標1 (1本目の指)
		var x1 = touches[0].pageX;
		var y1 = touches[0].pageY;
		// 座標2 (2本目の指)
		var x2 = touches[1].pageX;
		var y2 = touches[1].pageY;

    var d = ((x2 - x1)**2 + (y2 - y1)**2)**(1/2);
    console.log(d);
  }
}

// マウスのwheel、タッチパッド
function onWheel(event: WheelEvent) {
  event.preventDefault();
  if (event.deltaY**2 < 50**2) {
    return
  }
  const prev_scale = scale.value;
  scale.value -= event.deltaY / 10

  // 拡大縮小時のカーソル位置あわせて画像をずらす
  const mag = scale.value / prev_scale;
  let sign = (event.deltaY > 0)? 1 : -1;
  dx.value += (cursolX/mag)*0.1*sign;
  dy.value += (cursolY/mag)*0.1*sign;
}
// ------------------------------------------

// ------------------------------------------
// 画像のドラッグによる移動
let startX: number | undefined = undefined;
let startY: number | undefined = undefined;
function onDragStart(event: MouseEvent) {
  startX = event.x;
  startY = event.y;
}

function onMousemove(event: MouseEvent) {
  let el = event.target as HTMLDivElement;
  cursolX = event.offsetX - (el.clientWidth / 2);
  cursolY = event.offsetY - (el.clientHeight / 2);
  if (startX != undefined) {
    dx.value -= startX as number - event.x;
    dy.value -= startY as number - event.y;
    startX = event.x;
    startY = event.y;
  }
}

function onDrag(event: DragEvent) {
  dx.value -= startX as number - event.x;
  dy.value -= startY as number - event.y;
  startX = event.x;
  startY = event.y;
}

function onDragEnd(_: Event) {
  startX = undefined;
  startY = undefined;
}
// ------------------------------------------
</script>

<template>
<div class="imageArea" @touchmove="onTouchmove" @wheel="onWheel" @mousemove="onMousemove">
  <img 
    id="centerimage"
    ref="centerimage"
    :style="{ 
    height: scale + '%', 
    transform: 'translate('+dx+'px, '+dy+'px)'}"
    draggable="false"
    @mousedown="onDragStart"
    @drag="onDrag"
    @mouseup="onDragEnd"
    >
</div>
</template>

<style scoped>
.imageArea {
  overflow: hidden;
  width: 100vw;
  height: 70vh;
}
#centerimage {
  image-rendering: crisp-edges;
}
</style>