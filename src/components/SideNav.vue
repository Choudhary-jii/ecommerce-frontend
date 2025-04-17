<template>
  <div class="bg-gray-200 w-60 h-full p-4 space-y-4">
    <router-link to="/cart" class="block">My Cart</router-link>
    <router-link to="/wishlist" class="block">My Wishlist</router-link>
    <router-link to="/orders" class="block">My Orders</router-link>
  </div>
</template>

<script>
export default {
  name: "SideNav",
};
</script>

<style scoped>
/* Custom styles for the sidebar */
</style>
