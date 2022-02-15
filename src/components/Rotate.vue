<script setup>
import { ref, onMounted, computed, watch } from 'vue'

defineProps({
  msg: String
})

const rotate = ref(0)

const transform = computed(() => `rotate(${rotate.value}deg)`)

watch(transform, value => {
  drawRect()
})

const rotateCoordinates = (coor, width, height, rotate) => {
  const result = [coor]
  const [x, y] = coor
  const radian = rotate / 180 * Math.PI
  let a = x + width *  Math.cos(radian)
  let b = y + height *  Math.sin(radian)
  result.push([a, b])
  const c = Math.sqrt(width * width + height * height)
  const originRadian =  Math.atan(height / width)
  a = x + c *  Math.cos(radian + originRadian)
  b = y + c *  Math.sin(radian + originRadian)
  result.push([a, b])
  a = x + height * Math.cos(radian + Math.PI / 2)
  b = y + height * Math.sin(radian + Math.PI / 2)
  result.push([a, b])
  return result
}

const width = 100
const height = 100
const beigin = [450, 300]
const ctx = ref(null)

const drawRect = () => {
  ctx.value.clearRect(0, 0, 600, 600)
  ctx.value.fillRect(300, 300, 100, 100)
  const coordinates = rotateCoordinates(beigin, width, height, rotate.value)
  ctx.value.beginPath();
  ctx.value.moveTo(...beigin);
  for (const coor of coordinates) {
    ctx.value.lineTo(...coor);
  }
  // ctx.value.lineTo(...beigin)
  ctx.value.closePath();
  // ctx.value.stroke()
  ctx.value.fill()
}

const canvas = ref(null)
onMounted(() => {
  ctx.value = canvas.value.getContext('2d')
  ctx.value.fillStyle = 'green'
  // ctx.value.fillRect(300, 300, 100, 100)
  // ctx.fillRect(150, 0, 100, 100)
  // getRect(ctx, 150, 0, 100, 100, rotate)
  drawRect()
  setInterval(() => {
    rotate.value += 1
  }, 100);
})
</script>

<template>
  <div class="box">
    <div class="item">1</div>
    <div class="item rotate">2</div>
  </div>
  <div class="box">
    <canvas class="canvas" ref="canvas" width="600" height="600"></canvas>
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
