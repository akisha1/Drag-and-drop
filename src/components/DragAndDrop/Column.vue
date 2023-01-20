<template>
  <div
    :class="['column', { 'column-drag': isDrop }]"
    @dragover.prevent
    @dragenter.prevent
    @drop="setDropEmit($event)">
    <h2>{{ title }}</h2>
    <slot />
  </div>
</template>

<script setup>
import { ref } from "vue";

const emits = defineEmits(["setDropEmit"]);

const isDrop = ref(false);

const props = defineProps({
  id: {
    type: Number,
    required: true,
  },
  title: {
    type: String,
    required: true,
  },
});

const setDropEmit = (event) => {
  isDrop.value = true;
  setTimeout(() => {
    isDrop.value = false;
  }, "300");
  emits("setDropEmit", {
    event,
    id: props.id,
  });
};
</script>

<style scoped>
.column {
  padding: 15px;
  border-radius: 5px;
  background-color: rgb(193, 193, 193);
  margin-bottom: 10px;
}
.column h4 {
  color: white;
}
.column-drag {
  transition: background-color 0.3s ease-out;
  background-color: rgb(144, 141, 141);
}
</style>
