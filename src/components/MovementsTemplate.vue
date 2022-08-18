<template>
  <section :class="!showMovements ? 'movements' : 'showMovements'">
    <section class="clickSection" @click="showMovements = !showMovements">
      <div class="dropdown"></div>
      <p v-text="'Historial'" />
    </section>
    <section class="items-container" v-if="item.length !== 0">
      <HistoryItem
        v-for="item in item"
        @remove="removeItem"
        :key="item.id"
        :title="item.title"
        :description="item.description"
        :amount="item.amount"
        :movementType="item.movementType"
        :time="item.time"
        :id="item.id"
      />
    </section>
  </section>
</template>

<script setup>
import { ref, toRefs, defineProps, defineEmits } from "vue";
import HistoryItem from "./HistoryItem.vue";
const props = defineProps({
  item: {
    type: Array,
  },
});

const { item } = toRefs(props);

const showMovements = ref(false);
const emit = defineEmits(["remove"]);
const removeItem = (id) => emit("remove", id);
</script>

<style scoped>
.movements {
  background: white;
  width: 100%;
  height: 470px;
  padding: 0 10px;
  position: fixed;
  box-shadow: 0px -4px 16px rgba(6, 137, 176, 0.3);
  border-radius: 32px 32px 0px 0px;
  transform: translateY(-20%);
  transition: all 1s;
}
.showMovements {
  background: white;
  width: 100%;
  height: 470px;
  padding: 0 10px;
  position: fixed;
  box-shadow: 0px -4px 16px rgba(6, 137, 176, 0.3);
  border-radius: 32px 32px 0px 0px;
  transform: translateY(-100%);
  transition: all 500ms;
}
.items-container {
  height: calc(470px - 110px);
  width: 100%;
  overflow-y: auto;
  padding: 10px 0;
}
.clickSection {
  padding: 30px 10px 20px;
}
p {
  margin-top: 24px;
  font-weight: 700;
  font-size: 24px;
  color: #0d96bf;
}
.dropdown {
  width: 80px;
  height: 8px;
  margin: 0 auto;
  background: #cbf3ff;
  border-radius: 4px;
}
</style>
