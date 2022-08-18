<template>
  <main>
    <div class="first-section">
      <h1 class="title"><span class="title-2">Total </span>Ahorrado</h1>
      <span
        :class="String(total).includes('-') ? 'red' : 'green'"
        class="total-title"
        v-text="currencyFormat.format(total)"
      />
    </div>
    <span
      class="title-grafic"
      v-text="'Click en Logo para actualizar la grafica'"
    />
    <GraphicChart />
    <button
      class="button-add button-active"
      @click="showModal = true"
      v-text="'Agregar Movimiento'"
    />
    <teleport to="#app">
      <section class="modal" :class="showModal ? 'transition' : ''">
        <div class="header-modal">
          <p v-text="'Nuevo Movimiento'" />
          <button @click="showModal = false" class="button-close">
            <CloseIcon class="Close" />
          </button>
        </div>
        <form @submit.prevent>
          <div class="field">
            <label v-text="'Titulo'" />
            <input type="text" v-model="title" />
          </div>
          <div class="field">
            <label v-text="'Monto'" />
            <input type="number" v-model="amount" />
          </div>
          <div class="field">
            <label v-text="'Descripción'" />
            <textarea rows="7" v-model="description"></textarea>
          </div>
          <div class="field">
            <label class="radio-label">
              <input type="radio" v-model="movementType" value="Ingreso" />
              <span>Ingreso</span>
            </label>
            <label class="radio-label">
              <input type="radio" v-model="movementType" value="Gasto" />
              <span>Gasto</span>
            </label>
          </div>
          <span class="message-success" v-show="message" v-text="message" />
          <button
            :disabled="validationButton"
            :class="validationButton ? 'button-disabled' : 'button-active'"
            class="button-add"
            @click="loadItem"
            v-text="'Agregar Movimiento'"
          />
        </form>
      </section>
    </teleport>
  </main>
</template>

<script setup>
import { ref, defineEmits, defineProps, toRefs, computed } from "vue";
import { v4 as uuidv4 } from "uuid";
import CloseIcon from "../assets/CloseIcon.vue";
import GraphicChart from "./GraphicChart.vue";

const props = defineProps({
  total: {
    type: Number,
  },
});
const currencyFormat = new Intl.NumberFormat("en-US", {
  style: "currency",
  currency: "USD",
  maximumFractionDigits: 2,
  roundingIncrement: 5,
});

const { total } = toRefs(props);

const showModal = ref(false);
const title = ref(null);
const amount = ref(null);
const description = ref(null);
const movementType = ref("Ingreso");
const message = ref("");

const validationButton = computed(() => !title.value || !amount.value);

const emit = defineEmits(["create"]);

const loadItem = () => {
  if (movementType.value === "Gasto") amount.value *= -1;
  console.log(amount.value);
  emit("create", {
    title: title.value,
    amount: amount.value,
    description: description.value,
    movementType: movementType.value,
    time: new Date().toLocaleString(),
    id: uuidv4(),
  });
  title.value = null;
  amount.value = null;
  description.value = null;
  movementType.value = "Ingreso";
  message.value = "Su movimiento se agregó exitosamente";
};
</script>

<style scoped>
main {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 60px;
  height: calc(100vh - 100px);
}
.first-section {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 10px;
}
.button-disabled {
  background: gray;
}
.button-active {
  background: #0689b0;
}
.button-add {
  border: none;
  padding: 19px 48px;
  border-radius: 30px;
  color: white;
  font-weight: 400;
  font-size: 18px;
}
form {
  display: flex;
  flex-direction: column;
}
.button-close {
  background: transparent;
  border: none;
}
.modal {
  padding: 26px 30px;
  position: fixed;
  width: 100%;
  height: 100vh;
  background: white;
  transform: translateY(0%);
  transition: all 1s;
}
.transition {
  transform: translateY(-100%);
  transition: all 500ms;
}
.Close {
  width: 40px;
}
.title-grafic {
  color: #0689b0;
  font-weight: 600;
}
.header-modal {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 33px;
}
.header-modal p {
  color: #0d96bf;
  font-size: 24px;
  font-weight: 400;
  line-height: 28px;
}
.field {
  display: flex;
  flex-direction: column;
  margin-bottom: 21px;
}
.field label {
  margin-bottom: 8px;
}
input {
  height: 50px;
  border: 2px solid #0689b0;
  border-radius: 4px;
  padding: 10px;
  font-size: 20px;
}

textarea {
  border: 2px solid #0689b0;
  border-radius: 4px;
  padding: 10px;
  font-size: 20px;
}
.radio-label {
  display: flex;
  align-items: center;
  gap: 10px;
}
.total-title {
  font-size: 40px;
  font-weight: 700;
  line-height: 28px;
}
.green {
  color: #04b500;
}
.red {
  color: red;
}
.title {
  color: darkcyan;
}
.title-2 {
  color: lightseagreen;
}
.message-success {
  color: green;
  font-size: 18px;
  font-weight: 600;
  text-align: center;
  margin-bottom: 10px;
}
</style>
