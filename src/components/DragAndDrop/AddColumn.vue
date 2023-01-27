<template>
  <div class="add-board">
    <input
      class="input-text"
      placeholder="Введите название колонки"
      type="text"
      v-model="newColumnName"
      @keyup.enter="setNewColumnAddingEmit" />
    <button class="btn" @click="setNewColumnAddingEmit">+ Добавить новую колонку</button>
  </div>
</template>

<script setup>
import { ref } from "vue";

const emits = defineEmits(["setNewColumnAddingEmit"]);

const newColumnName = ref("");

const setNewColumnAddingEmit = () => {
  if (!!newColumnName.value) emits("setNewColumnAddingEmit", newColumnName.value);
  newColumnName.value = "";
};
</script>
