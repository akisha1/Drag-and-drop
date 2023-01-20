<template>
  <div class="card" @dragstart="setDragsEmit($event)" draggable="true">
    <h5>{{ card.title }}</h5>
  </div>
</template>

<script setup>
const emits = defineEmits(["setDragsEmit"]);

const props = defineProps({
  card: {
    type: Object,
    required: true,
  },
});

const setDragsEmit = (event) => {
  emits("setDragsEmit", {
    event,
    id: props.card.id,
  });
};
</script>

<style scoped>
.card {
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 50px;
  background: rgba(255, 255, 255);
  padding: 5px;
  border-radius: 5px;
  margin-bottom: 5px;
}
</style>
