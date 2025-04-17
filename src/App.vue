<template>
  <div class="flex">
    <!-- SideNav Component -->
    <SideNav />

    <div class="flex-1 p-4">
      <!-- TopNav Component -->
      <TopNav />

      <!-- Router View for Dynamic Pages -->
      <router-view />
    </div>
  </div>
</template>

<script>
import TopNav from "./components/TopNav.vue";
import SideNav from "./components/SideNav.vue";

export default {
  components: {
    TopNav,
    SideNav,
  },
};
</script>

<style scoped>
/* Global styling if needed */
</style>
