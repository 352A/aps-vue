<template>
  <section>
    <h2 class="bg-grey-darken-4 pa-3 text-center">Vue Viewer</h2>

    <div
      class="pt-6"
      style="
        transform: scale(0.85);
        transform-origin: top;
        width: 100%;
        height: 100vh;
      "
    >
      <ApsViewer />
    </div>
  </section>
</template>

<script setup>
import ApsViewer from "./components/ApsViewer.vue";
</script>
