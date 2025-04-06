<script setup>
const props = defineProps({
  size: {
    type: Number,
    default: 40
  },
  color: {
    type: String,
    default: '#ffffff'
  }
})
</script>

<template>
  <div class="qingming-flower" :style="{
    width: `${size}px`, 
    height: `${size}px`
  }">
    <div class="petals" :style="{ backgroundColor: color }"></div>
  </div>
</template>

<style scoped>
.qingming-flower {
  position: relative;
  display: inline-block;
}

.petals {
  position: absolute;
  width: 100%;
  height: 100%;
  border-radius: 50%;
  box-shadow: 
    calc(var(--size) * 0.25) 0 0 calc(var(--size) * -0.125) var(--color),
    calc(var(--size) * -0.25) 0 0 calc(var(--size) * -0.125) var(--color),
    0 calc(var(--size) * 0.25) 0 calc(var(--size) * -0.125) var(--color),
    0 calc(var(--size) * -0.25) 0 calc(var(--size) * -0.125) var(--color),
    calc(var(--size) * 0.176) calc(var(--size) * 0.176) 0 calc(var(--size) * -0.125) var(--color),
    calc(var(--size) * -0.176) calc(var(--size) * 0.176) 0 calc(var(--size) * -0.125) var(--color),
    calc(var(--size) * 0.176) calc(var(--size) * -0.176) 0 calc(var(--size) * -0.125) var(--color),
    calc(var(--size) * -0.176) calc(var(--size) * -0.176) 0 calc(var(--size) * -0.125) var(--color);
  --size: v-bind(size + 'px');
  --color: v-bind(color);
}

.petals::after {
  content: "";
  position: absolute;
  width: 35%;
  height: 35%;
  background-color: #ffc107;
  border-radius: 50%;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
</style> 