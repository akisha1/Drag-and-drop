<template>
  <div class="card" @dragstart="setDragsEmit($event)" draggable="true">
    <div class="card_wrapper">
      <div class="card__decorate" />
      <span class="card__title">{{ card.title }}</span>
    </div>
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
  event.stopPropagation();
  emits("setDragsEmit", {
    event,
    id: props.card.id,
    isColumn: false,
  });
};
</script>

<style lang="scss" scoped>
.card {
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 80px;
  background: rgba(255, 255, 255);
  padding: 5px;
  border-radius: 5px;
  margin-bottom: 5px;
  &_wrapper {
    position: relative;
    height: 100%;
    width: 100%;
  }
  &__title {
    height: 100%;
    align-items: center;
    display: flex;
  }
  &__decorate {
    position: absolute;
    top: 6px;
    left: 0;
    display: inline-block;
    border-radius: 4px;
    padding: 0 8px;
    height: 8px;
    width: 40px;
    background-color: rgb(3, 188, 3);
  }
}
</style>
