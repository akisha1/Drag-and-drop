<template>
  <AddColumn @setNewColumnAddingEmit="addNewColumn" />
  <div class="board" :style="{ gridTemplateColumns: `repeat(${columnsNumber}, 1fr)` }">
    <Column
      v-for="column in columns"
      :key="column.id"
      :id="column.id"
      :title="column.title"
      @setDropEmit="dropEvent"
      @setDragsEmitFromColumn="dragEvent">
      <Card
        v-for="card in cards.filter((filteredCard) => filteredCard.columnId === column.id)"
        :key="card.id"
        :card="card"
        @setDragsEmit="dragEvent" />
    </Column>
  </div>
</template>

<script setup>
import AddColumn from "@/components/DragAndDrop/AddColumn.vue";
import Column from "@/components/DragAndDrop/Column.vue";
import Card from "@/components/DragAndDrop/Card.vue";
import { startColumns, startCards } from "@/common/boardData.js";
import { ref, computed, onBeforeMount } from "vue";

onBeforeMount(() => {
  columns.value = startColumns;
  cards.value = startCards;
});

const columns = ref([]);

const cards = ref([]);

const columnsNumber = computed(() => Object.keys(columns.value).length);

const dragEvent = ({ event, id, isColumn }) => {
  let setId = !!isColumn ? "columnId" : "cardId";
  event.dataTransfer.dropEffect = "move";
  event.dataTransfer.effectAllowed = "move";
  event.dataTransfer.setData(setId, id.toString());
};

const dropEvent = ({ event, id }) => {
  const columnId = parseInt(event.dataTransfer.getData("columnId"));
  const cardId = parseInt(event.dataTransfer.getData("cardId"));
  if (!!columnId) {
    let mutateCloneOfColumns = JSON.parse(JSON.stringify(columns.value));
    const staticClone = JSON.parse(JSON.stringify(mutateCloneOfColumns));
    mutateCloneOfColumns[id - 1] = mutateCloneOfColumns[columnId - 1];
    mutateCloneOfColumns[columnId - 1] = staticClone[id - 1];
    columns.value = mutateCloneOfColumns;
    return;
  }
  cards.value = cards.value.map((filteredCard) => {
    if (filteredCard.id == cardId) filteredCard.columnId = id;
    return filteredCard;
  });
};

const addNewColumn = (name) => {
  const lastIdOfColumn = columnsNumber.value;
  let title = name;
  columns.value.push({
    id: lastIdOfColumn + 1,
    title,
  });
};
</script>

<style>
@import "./assets/base.css";

body {
  background: rgba(235, 160, 95, 0.9);
}

.board {
  height: calc(100vh - 100px);
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  column-gap: 30px;
}
</style>
