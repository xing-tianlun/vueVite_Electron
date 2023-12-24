<template>
  <div>
    <div>{{ obj.num }}</div>
    <button class="btn btn_primary" @click="btn">btn</button>
    <div>{{ numArr }}</div>
    <Temp :title="obj.title" @add="addfn"/>
    <div v-if="numa.a !== 0">
      a: {{ numa.a }}
    </div>
    <a href="https://www.baidu.com">百度两下</a>
  </div>
</template>

<script setup lang="ts">
import { ref,isRef, reactive, isReactive } from "vue";
import Temp from './Temp.vue';
let num = ref(1);
let obj = reactive({
  num: 10,
  title: '标题',
});
console.log(isRef(obj));
console.log(isReactive(obj));

function btn() {
  obj.num++;
}
let numa = reactive({
  a: 0,
});
function addfn(num: number) {
  console.log(num);
  numa.a = num;
} 

let numArr = [2, 1, 34, 3, 2, 45, 22, 34, 4];
function bubbleSort(numArr: number[]) {
  for (let i = 0; i < numArr.length - 1; i++) {
    for (let j = 0; j < numArr.length - 1 - i; j++) {
      if (numArr[j] > numArr[j + 1]) {
        let temp = numArr[j];
        numArr[j] = numArr[j + 1];
        numArr[j + 1] = temp;
      }
    }
  }
  return numArr;
}
console.log(bubbleSort(numArr));

let numberArr2 = [2, 34, 3, 2, 45, 22, 34, 4, 10, 23, 1];
function selectSort(numArr: number[]) {
  for (let i = 0; i < numArr.length; i++) {
    let minIndex = i;
    for (let j = i + 1; j < numArr.length; j++) {
      if (numArr[j] < numArr[minIndex]) {
        minIndex = j;
      }
    }
    if (minIndex !== i) {
      let temp = numArr[minIndex];
      numArr[minIndex] = numArr[i];
      numArr[i] = temp;
    }
  }
  return numArr;
}
console.log(selectSort(numberArr2));
</script>
<style lang="scss" scoped>
</style>
