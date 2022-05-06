<template>
  <div class="container">
    <span class="s">
      <span class="t" v-for="(i, x) in clock">
        <span :class="[x % 2 == 1 && x != 5 ? 'x' : '']">{{ i }}</span>
      </span>
    </span>
    <div class="clock">
      <div class="col" v-for="(i, n) in clock">
        <div v-for="x in 4" :class="render(i, x)"></div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue'

const clock = ref([0, 0, 0, 0, 0, 0])

const precede0 = t => {
  const s = []
  if (t < 10) {
    s[0] = 0
    s[1] = t
  } else {
    const m = `${t}`
    s[0] = Number(m[0])
    s[1] = Number(m[1])
  }
  return s
}

/**
 * @param arr 目标数组
 * @param sum 目标和
 * @returns [] 计算结果的数组
 */
const algorithm = (arr, result) => {
  if (result == 0) return [0]
  for (let i = 1; i < 1 << arr.length; i++) {
    let sum = 0
    let temp = ''
    for (let j = 0; j < arr.length; j++) {
      if ((i & (1 << j)) != 0) {
        //用i与2^j进行位与运算，若结果不为0,则表示第j位不为0,从数组中取出第j个数
        sum += arr[j]
        temp += arr[j] + '+'
      }
    }
    if (sum == result) {
      let t = temp.split('+')
      let p = []
      for (let j = 0; j < t.length; j++) {
        if (t[j] != '') {
          const n = Number(t[j])
          p.push(n)
        }
      }
      return p
    }
  }
}

const render = (sum, index) => {
  const map = {
    1: 8,
    2: 4,
    3: 2,
    4: 1,
  }
  const t = map[index]
  const a = [1, 2, 4, 8]
  const n = algorithm(a, sum)
  const s = new Set(n)
  return s.has(t) ? 'active item' : 'item'
}

const time = () => {
  const time = new Date()
  const H = time.getHours()
  const s = time.getMinutes()
  const i = time.getSeconds()
  const clock = [...precede0(H), ...precede0(s), ...precede0(i)]
  return clock
}

onMounted(() =>
  setInterval(() => {
    clock.value = time()
  }, 1000)
)
</script>

<style>
html body {
  margin: 0;
}
.s {
  margin-left: 0.5em;
}
.t {
  user-select: none;
  color: #abb8c3;
  font-size: 0.5em;
}
.x::after {
  content: ':';
  margin: 0.2em;
}
.container {
  height: 100vh;
  display: flex;
  background-color: #f8f8f8;
}
.clock {
  margin: auto;
  display: flex;
}
.item {
  background: #fff;
  color: #000;
  width: 20px;
  height: 20px;
  margin: 0.2em;
}
.active {
  background-color: #fcb900;
}
</style>
