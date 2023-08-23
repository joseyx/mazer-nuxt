<script lang="ts" setup>
import LayoutSidebar from "~~/components/Layouts/Sidebar/LayoutSidebar.vue";
import VerticalHeader from "~~/components/Layouts/VerticalHeader.vue";
import LayoutFooter from "~~/components/Layouts/LayoutFooter.vue";
</script>
<template>
  <div id="app">
    <LayoutSidebar></LayoutSidebar>
    <div id="main" class="layout-navbar navbar-fixed">
      <VerticalHeader></VerticalHeader>
      <div id="main-content">
        <slot />
        <LayoutFooter></LayoutFooter>
      </div>
    </div>
  </div>
</template>
<style lang="scss">
@import "assets/scss/app.scss";
@import "assets/scss/themes/dark/app-dark.scss";
@import "assets/scss/iconly.scss";
</style>
