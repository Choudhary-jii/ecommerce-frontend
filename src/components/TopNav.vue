<template>
  <div class="bg-gray-800 text-white p-4 flex justify-between">
    <div class="flex space-x-4">
      <router-link to="/" class="text-xl">Home</router-link>
      <router-link to="/products/category/mobile">Mobile</router-link>
      <router-link to="/products/category/clothings">Clothings</router-link>
      <router-link to="/products/search/Home%20&%20Decor"
        >Home & Decor</router-link
      >
      <router-link to="/products/category/sports">Sports</router-link>
      <router-link to="/products/category/softwares">Softwares</router-link>
    </div>
    <router-link to="/cart" class="text-xl">My Cart</router-link>
  </div>
</template>

<script>
export default {
  name: "TopNav",
};
</script>

<style scoped>
/* Custom styles if needed */
</style>
