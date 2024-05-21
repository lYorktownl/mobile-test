<template>
  <div class="app-container">
    <HeaderComponent />
    <ComparsionTable />
    <div class="fon"></div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import ComparsionTable from "./components/ComparsionTable.vue";
import HeaderComponent from "./components/HeaderComponent.vue";

export default defineComponent({
  name: "App",
  components: {
    ComparsionTable,
    HeaderComponent,
  },
});
</script>

<style lang="scss">
.app-container {
  position: relative;
  min-height: 100vh; // Устанавливаем минимальную высоту на 100% высоты окна браузера
}

.fon {
  z-index: -1;
  position: absolute;
  width: 100%;
  top: 428px;
  bottom: 0;
  background-color: #f4f9fc;
  min-height: calc(100vh - 465px); // Делаем высоту равной 100vh минус 465px
  padding-bottom: 91px;
}
</style>
