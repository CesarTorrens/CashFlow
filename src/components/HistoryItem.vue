<template>
  <section :class="removeEffect ? 'item-remove' : ''" class="item">
    <div class="item-text one">
      <p v-text="title" />
      <button @click="removeItem(id)">
        <Trash class="trash" />
      </button>
    </div>
    <div class="item-text two">
      <div>
        <p v-text="description" />
        <p class="text-gray" v-text="time" />
      </div>
      <span
        :class="movementType === 'Ingreso' ? 'green' : 'red'"
        v-text="currencyFormat.format(amount)"
      />
    </div>
  </section>
</template>

<script setup>
import { ref, toRefs, defineProps, defineEmits } from "vue";
import Trash from "@/assets/TrashIcon.vue";

const props = defineProps({
  title: {
    type: String,
  },
  description: {
    type: String,
  },
  amount: {
    type: Number,
  },
  id: {
    type: String,
  },
  movementType: {
    type: String,
  },
  time: {
    type: String,
  },
});

const { title, description, amount, movementType, id, time } = toRefs(props);
const currencyFormat = new Intl.NumberFormat("en-US", {
  style: "currency",
  currency: "USD",
  maximumFractionDigits: 2,
  roundingIncrement: 5,
});
const removeEffect = ref(false);
const emit = defineEmits(["remove"]);
const removeItem = () => {
  removeEffect.value = true;
  emit("remove", id.value);
};
</script>

<style scoped>
.trash {
  width: 30px;
}
.item {
  background: #e6f9ff;
  border-radius: 8px;
  padding: 18px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-bottom: 8px;
  transform: scale(1);
  transition: all 1s;
}

.item-remove {
  transform: scale(0.9);
  transition: all 1s;
}
.item-text {
  display: flex;
  justify-content: space-between;
}
p {
  text-overflow: ellipsis;
  overflow: hidden;
}
.one {
  margin-bottom: 11px;
  color: #5a5a5a;
  font-size: 18px;
  font-weight: 600;
  line-height: 21px;
}
.two {
  font-weight: 300;
  font-size: 14px;
  line-height: 16px;
}
span {
  display: flex;
  align-items: flex-end;
}
.green {
  color: green;
}
.red {
  color: red;
}
.text-gray {
  color: rgb(128, 128, 128);
  margin-top: 5px;
}
button {
  background: transparent;
  border: none;
  display: flex;
}
</style>
