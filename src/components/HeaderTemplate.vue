<template>
  <header>
    <img @click="reload" src="@/assets/logo.png" alt="logo" />
    <h1><span>Cash</span>Flow</h1>
  </header>
</template>

<script>
export default {
  methods: {
    reload() {
      location.reload();
    },
  },
};
</script>

<style scoped>
header {
  width: 100%;
  height: 100px;
  padding: 0 20px;
  display: flex;
  column-gap: 60px;
  align-items: center;
}
img {
  width: 60px;
}
h1 {
  color: darkcyan;
}
span {
  color: lightseagreen;
}
</style>
