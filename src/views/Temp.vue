<template>
  <div>
    <h3>{{ title }} props</h3>
    <button @click="btnFn">打印</button>
  </div>
</template>

<script setup lang="ts">
import { reactive } from "vue";
let t = reactive({
  num: 30
})
defineProps({
    title:{
      type: String,
      default: "",
    }
  }
);
const emit = defineEmits(['add'])
function btnFn() {
  t.num++;
  emit('add', t.num)
}
</script>
<style lang="scss" scoped></style>
