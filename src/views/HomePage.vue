<template>
  <div class="bg-gray-100 min-h-screen">
    <!-- Top Navbar -->
    <nav
      class="bg-blue-600 px-6 py-4 flex justify-between items-center text-white sticky top-0 z-10"
    >
      <!-- Logo -->
      <div class="text-2xl font-bold">E-Shop</div>

      <!-- Search Bar + Button -->
      <div class="flex w-full md:w-1/2 lg:w-1/3 gap-2">
        <input
          type="text"
          v-model="searchQuery"
          @input="debouncedSearch"
          placeholder="Search products..."
          class="flex-1 p-2 rounded-lg text-black focus:outline-none focus:ring-2 focus:ring-blue-300"
        />
        <button
          @click="manualSearch"
          class="bg-white text-blue-600 font-semibold px-4 py-2 rounded-lg hover:bg-blue-100 transition"
        >
          Search
        </button>
      </div>
    </nav>
  </div>

  <!-- Sidebar + Main Content -->
  <div class="flex">
    <!-- Sidebar -->
    <aside
      class="bg-indigo-700 w-60 min-h-screen p-4 text-white hidden md:block"
    >
      <h2 class="text-2xl mb-4">Categories</h2>
      <ul>
        <li
          v-for="category in categories"
          :key="category"
          @click="filterByCategory(category)"
          class="cursor-pointer hover:underline mb-2"
        >
          {{ category }}
        </li>
        <li
          @click="clearCategory"
          class="cursor-pointer hover:underline mt-6 text-gray-300 text-sm"
        >
          Clear Filter
        </li>
      </ul>
    </aside>

    <!-- Main Content -->
    <main class="flex-1 p-6">
      <!-- Loading Spinner -->
      <div v-if="loading" class="flex justify-center items-center h-64">
        <div
          class="animate-spin rounded-full h-12 w-12 border-b-2 border-blue-600"
        ></div>
      </div>

      <!-- Error Message -->
      <div v-if="error" class="text-center text-red-500 text-xl">
        Oh! Seems something went wrong on the page.
      </div>

      <!-- Products Grid -->
      <div
        v-if="filteredProducts.length && !loading"
        class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6"
      >
        <div
          v-for="product in filteredProducts"
          :key="product.id"
          class="bg-white rounded-lg shadow-md p-4"
        >
          <img
            :src="backendUrl + product.image"
            alt="Product"
            class="w-full h-40 object-cover rounded mb-3"
          />
          <h3 class="font-semibold text-lg">{{ product.title }}</h3>
          <p class="text-gray-600">${{ product.price }}</p>
          <p class="text-gray-400 text-sm">{{ product.category }}</p>
          <span
            class="inline-block mt-2 px-2 py-1 rounded text-white text-xs"
            :class="product.is_sale ? 'bg-green-500' : 'bg-red-500'"
          >
            {{ product.is_sale ? "Free" : "Sale" }}
          </span>
          <button
            @click="addToCart(product.id)"
            class="mt-4 bg-teal-500 hover:bg-teal-600 text-white px-4 py-2 rounded w-full transition"
          >
            Add to Cart
          </button>
        </div>
      </div>

      <!-- No Products Found -->
      <div
        v-if="!filteredProducts.length && !loading && !error"
        class="text-center text-gray-500 text-xl"
      >
        No Products Found
      </div>

      <!-- View All Products Button -->
      <div class="flex justify-center mt-10">
        <button
          @click="goToAllProducts"
          class="bg-blue-600 hover:bg-blue-700 text-white px-6 py-3 rounded-lg transition"
        >
          View All Products
        </button>
      </div>
    </main>
  </div>
</template>

<!-- <script>
import axios from "axios";
import _ from "lodash";

export default {
  name: "HomePage",
  data() {
    return {
      products: [],
      loading: false,
      error: false,
      backendUrl: "https://aayushchoudhary.pythonanywhere.com/",
      searchQuery: "",
      selectedCategory: "",
      categories: [
        "mobile",
        "electronics",
        "headphones",
        "clothings",
        "kids wear",
        "home & decor",
        "sports wear",
        "sports",
        "softwares",
        "laptops",
        "musical instruments",
        "toys",
        "bags",
        "beauty products",
        "furniture",
      ],
    };
  },
  computed: {
    filteredProducts() {
      let filtered = this.products;

      if (this.searchQuery.trim() !== "") {
        const q = this.searchQuery.trim().toLowerCase();
        filtered = filtered.filter(
          (product) =>
            product.title.toLowerCase().includes(q) ||
            product.category.toLowerCase().includes(q)
        );
      }

      if (this.selectedCategory !== "") {
        filtered = filtered.filter(
          (product) => product.category === this.selectedCategory
        );
      }

      return filtered;
    },
  },
  methods: {
    async fetchProducts() {
      this.loading = true;
      this.error = false;
      try {
        const response = await axios.get(
          `${this.backendUrl}/api/products/most-bought/`
        );
        this.products = response.data;
      } catch (err) {
        console.error("API Error:", err);
        this.error = true;
        setTimeout(() => this.$router.push("/"), 3000);
      } finally {
        this.loading = false;
      }
    },
    async addToCart(productId) {
      try {
        const token = localStorage.getItem("token");
        await axios.post(
          `${this.backendUrl}/api/cart/add/`,
          { product_id: productId, quantity: 1 },
          { headers: { Authorization: `Bearer ${token}` } }
        );
        await this.fetchProducts(); // Optional refresh
      } catch (err) {
        console.error("Add to Cart Error:", err);
      }
    },
    debouncedSearch: _.debounce(function () {
      // Triggers computed property refresh
    }, 500),
    goToAllProducts() {
      this.$router.push("/products");
    },
    filterByCategory(category) {
      this.selectedCategory = category;
    },
    clearCategory() {
      this.selectedCategory = "";
    },
  },
  created() {
    this.fetchProducts();
  },
};
</script> -->
<script>
import axios from "axios";
import _ from "lodash";

export default {
  name: "HomePage",
  data() {
    return {
      products: [],
      loading: false,
      error: false,
      backendUrl: "https://aayushchoudhary.pythonanywhere.com",
      searchQuery: "",
      selectedCategory: "",
      categories: [
        "mobile",
        "electronics",
        "headphones",
        "clothings",
        "kids wear",
        "home & decor",
        "sports wear",
        "sports",
        "softwares",
        "laptops",
        "musical instruments",
        "toys",
        "bags",
        "beauty products",
        "furniture",
      ],
    };
  },
  computed: {
    filteredProducts() {
      let filtered = this.products;

      if (this.searchQuery.trim() !== "") {
        const q = this.searchQuery.trim().toLowerCase();
        filtered = filtered.filter(
          (product) =>
            product.title.toLowerCase().includes(q) ||
            product.category.toLowerCase().includes(q)
        );
      }

      return filtered;
    },
  },
  methods: {
    async fetchProducts(category = "") {
      this.loading = true;
      this.error = false;
      try {
        let url = `${this.backendUrl}/api/products/most-bought/`;
        if (category) {
          url = `${this.backendUrl}/api/products/?category=${encodeURIComponent(
            category
          )}`;
        }
        const response = await axios.get(url);
        this.products = response.data;
      } catch (err) {
        console.error("API Error:", err);
        this.error = true;
        setTimeout(() => this.$router.push("/"), 3000);
      } finally {
        this.loading = false;
      }
    },
    async addToCart(productId) {
      try {
        const token = localStorage.getItem("token");
        await axios.post(
          `${this.backendUrl}/api/cart/add/`,
          { product_id: productId, quantity: 1 },
          { headers: { Authorization: `Bearer ${token}` } }
        );
        await this.fetchProducts(); // Optional refresh
      } catch (err) {
        console.error("Add to Cart Error:", err);
      }
    },
    debouncedSearch: _.debounce(function () {
      // Triggers computed property refresh
    }, 500),
    goToAllProducts() {
      this.$router.push("/products");
    },
    async filterByCategory(category) {
      this.selectedCategory = category;
      await this.fetchProducts(category);
      window.scrollTo(0, 0);
    },
    async clearCategory() {
      this.selectedCategory = "";
      await this.fetchProducts();
      window.scrollTo(0, 0);
    },
  },
  created() {
    this.fetchProducts();
  },
};
</script>

<style scoped>
/* Sidebar links hover */
aside ul li {
  @apply mb-2 cursor-pointer hover:text-yellow-300 transition-colors duration-300;
}

/* Search bar style */
input[type="text"] {
  @apply w-full p-2 rounded-lg border text-black focus:outline-none focus:ring-2 focus:ring-blue-500;
}

/* Buttons - general transition */
button {
  @apply transition duration-300;
}

/* Specific Search Button Style */
button.search-btn {
  @apply bg-white text-blue-600 font-semibold px-4 py-2 rounded-lg hover:bg-blue-100 transition duration-300 shadow-sm;
}

/* Navbar sticky and layout */
nav {
  @apply bg-blue-600 px-6 py-4 flex justify-between items-center text-white sticky top-0 z-10;
}
</style>
