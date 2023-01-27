<template>
  <div
    :class="['column', { 'column-drag': isDrop }]"
    @dragover.prevent
    @dragenter.prevent
    @dragstart="setDragsEmitFromColumn($event)"
    draggable="true"
    @drop="setDropEmit($event)">
    <h2>{{ title }}</h2>
    <slot />
  </div>
</template>

<script setup>
import { ref } from "vue";

const emits = defineEmits(["setDropEmit", "setDragsEmitFromColumn"]);

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
  }, "400");
  emits("setDropEmit", {
    event,
    id: props.id,
  });
};

const setDragsEmitFromColumn = (event) => {
  emits("setDragsEmitFromColumn", {
    event,
    id: props.id,
    isColumn: true,
  });
};
</script>

<style scoped>
.column {
  cursor: pointer;
  padding: 15px;
  border-radius: 5px;
  background-color: #ebecf0;
  margin-bottom: 10px;
}
.column h4 {
  color: white;
}
.column-drag {
  transition: background-color 0.4s ease-out;
  background-color: rgb(225, 223, 223);
}
</style>
