<template>
  <MdEditor :value="value" :handle-change="onChange" />
  <CodeEdit />
</template>

<script setup lang="ts">
import { ref } from "vue";
import MdEditor from "@/components/MdEditor.vue";
import CodeEdit from "@/components/CodeEdit.vue";

const value = ref();

const onChange = (v: string) => {
  value.value = v;
};
</script>
