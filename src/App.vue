<template>
  <div>
    <HeaderComponent />
    <ComparsionTable />
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

<style lang="scss"></style>
