<script setup>
import { ref, onMounted } from 'vue'

defineProps({
  msg: String
})

const rotate = 72
const transform = ref('')
transform.value = `rotate(${rotate}deg)`

const getRect = (ctx, x, y, w, h, rotate) => {
  const originX = x
  const originY = y
  ctx.beginPath();
  ctx.moveTo(x, y);
  console.log(x, y)
  x = originX + Math.cos(rotate * Math.PI / 180) * w
  y = originY +  Math.sin(rotate * Math.PI / 180) * w
  ctx.lineTo(x, y);
  console.log(x, y)
  const z = Math.sqrt(w * w+ h * h)
  const originRotate = Math.round(Math.atan(w/h) * 180 / Math.PI)
  console.log(originRotate, rotate + originRotate)
  x = originX + Math.cos((rotate + originRotate) * Math.PI / 180) * z
  y = originY + Math.sin((rotate + originRotate) * Math.PI / 180) * z
  console.log(Math.cos((rotate + originRotate) * Math.PI / 180) * z, Math.sin((rotate + originRotate) * Math.PI / 180) * z)
  console.log(x, y)
  ctx.lineTo(x, y);
  x = originX - Math.sin(rotate * Math.PI / 180) * w
  y = originY +  Math.cos(rotate * Math.PI / 180) * w
  console.log(x, y)
  ctx.lineTo(x, y);
  ctx.lineTo(originX, originY);
  ctx.stroke();
}

const canvas = ref(null)
onMounted(() => {
  const ctx = canvas.value.getContext('2d')
  ctx.fillStyle = 'green'
  ctx.fillRect(0, 0, 100, 100)
  // ctx.fillRect(150, 0, 100, 100)
  getRect(ctx, 150, 0, 100, 100, rotate)
})
</script>

<template>
  <div class="box">
    <div class="item">1</div>
    <div class="item rotate">2</div>
  </div>
  <div class="box">
    <canvas class="canvas" ref="canvas"></canvas>
  </div>
</template>

<style lang="scss" scoped>
.box {
  width: 250px;
  display: flex;
  margin-top: 50px;
  justify-content: space-between;
  .item {
    width: 100px;
    height: 100px;
    background-color: yellow;
  }
  .rotate {
    transform: v-bind(transform);
    transform-origin: top left;
  }
}
</style>
