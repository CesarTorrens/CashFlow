<template>
  <LayoutTemplate>
    <template #header>
      <HeaderTemplate />
    </template>
    <template #resume>
      <ResumeTemplate
        :chartData="chartData"
        :total="totalAmount"
        @create="saveItem"
      />
    </template>
    <template #movements>
      <MovementsTemplate @remove="deleteItem" :item="dataItem" />
    </template>
  </LayoutTemplate>
</template>

<script>
import LayoutTemplate from "./LayoutTemplate.vue";
import HeaderTemplate from "./HeaderTemplate.vue";
import ResumeTemplate from "./ResumeTemplate.vue";
import MovementsTemplate from "./MovementsTemplate.vue";

export default {
  components: {
    LayoutTemplate,
    HeaderTemplate,
    ResumeTemplate,
    MovementsTemplate,
  },
  data() {
    return {
      dataItem: [],
    };
  },
  mounted() {
    if (localStorage.getItem("dataItem"))
      this.dataItem = JSON.parse(localStorage.getItem("dataItem"));
  },
  computed: {
    totalAmount() {
      return this.dataItem.reduce((suma, m) => {
        return suma + m.amount;
      }, 0);
    },
    chartData() {
      const dataTime = this.dataItem.map((movement) =>
        movement.time.split(",")
      );
      const data = this.dataItem.map((movement) => movement.amount);

      return {
        labels: dataTime?.reverse(),
        datasets: [
          {
            label: "Ultimos Movimientos",
            data: data?.reverse(),
          },
        ],
      };
    },
  },
  methods: {
    saveItem(movement) {
      this.dataItem.unshift(movement);
      this.save();
    },
    deleteItem(id) {
      setTimeout(() => {
        const idDelete = this.dataItem.findIndex((m) => m.id === id);
        this.dataItem.splice(idDelete, 1);
        this.save();
      }, 500);
    },
    save() {
      localStorage.setItem("dataItem", JSON.stringify(this.dataItem));
    },
  },
};
</script>
