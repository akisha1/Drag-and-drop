<template>
  <div class="add-board">
    <input class="input-text" type="text" v-model="newColumnName" />
    <button class="btn" @click="addNewColumn">+ Add a new column</button>
  </div>
  <div class="board" :style="{ gridTemplateColumns: `repeat(${columnsNumber}, 1fr)` }">
    <Column
      v-for="column in columns"
      :key="column.id"
      :id="column.id"
      :title="column.title"
      @setDropEmit="dropEvent">
      <Card
        v-for="card in cards.filter((filteredCard) => filteredCard.columnId === column.id)"
        :key="card.id"
        :card="card"
        @setDragsEmit="dragEvent" />
    </Column>
  </div>
</template>

<script setup>
import Column from "@/components/DragAndDrop/Column.vue";
import Card from "@/components/DragAndDrop/Card.vue";

import { ref, computed } from "vue";

const columnsNumber = computed(() => Object.keys(columns.value).length);

let newColumnName = ref("");

const columns = ref([
  {
    id: 0,
    title: "В очереди",
  },
  {
    id: 2,
    title: "В работе",
  },
  {
    id: 3,
    title: "Сделано",
  },
]);

const cards = ref([
  {
    id: 1,
    title: "Таск №1",
    columnId: 0,
  },
  {
    id: 2,
    title: "Таск №2",
    columnId: 0,
  },
  {
    id: 3,
    title: "Таск №3",
    columnId: 0,
  },
  {
    id: 4,
    title: "Таск №4",
    columnId: 2,
  },
  {
    id: 5,
    title: "Таск №5",
    columnId: 2,
  },
  {
    id: 6,
    title: "Таск №6",
    columnId: 3,
  },
]);

const dragEvent = ({ event, id }) => {
  event.dataTransfer.dropEffect = "move";
  event.dataTransfer.effectAllowed = "move";
  event.dataTransfer.setData("cardId", id.toString());
};
const dropEvent = ({ event, id }) => {
  const cardId = parseInt(event.dataTransfer.getData("cardId"));
  cards.value = cards.value.map((filteredCard) => {
    if (filteredCard.id == cardId) filteredCard.columnId = id;
    return filteredCard;
  });
};

const addNewColumn = () => {
  const lastIdOfColumn = Object.keys(columns.value).length;
  if (!!newColumnName.value.length) {
    let title = newColumnName.value;
    columns.value.push({
      id: lastIdOfColumn + 1,
      title,
    });
    newColumnName.value = "";
  }
};
</script>

<style scoped>
.input-text {
  padding: 5px;
  margin: 15px 0 15px 15px;
  border-radius: 5px;
}

.btn {
  padding: 5px;
  margin: 15px;
  border-radius: 5px;
}

.board {
  margin: 60px 0;
  height: calc(100vh - 120px);
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  column-gap: 20px;
}
</style>
