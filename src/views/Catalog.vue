<script setup>
import { computed, ref } from 'vue';
import { onMounted } from 'vue';
import { productsStore } from "../stores/products";
import { useRouter } from 'vue-router';

const store = productsStore();
const routers = useRouter();
const searchTerm = ref('');

const searchResults = computed(() => {
  const term = searchTerm.value.toLowerCase();
  if (term !== ' ') {
    return store.products.filter(product =>
      product.title.toLowerCase().includes(term)
    );
  } else {
    return store.products;
  }
});

const noResultsFound = computed(() => !searchResults.value.length);

onMounted(async () => {
  await store.fetchProductsFromDB();
});

const goToProductPage = (id) => {
  routers.push({ name: 'ProductView', params: { id } });
};

const searchingProduct = (e) => {
  searchTerm.value = e.target.value;
};
</script>

<template>
  <div class="container">
    <input @input="searchingProduct" class="search-item" type="text" placeholder="Search">
    <div class="products-list">
      <div v-for="product in searchResults" :key="product.id" class="product" @click="goToProductPage(product.id)">
        <img :src="product.thumbnail" alt="" srcset="">
        <h2>
          {{ product.title }}
        </h2>
        <h3>
          Brand: {{ product.brand }}
        </h3>
        <p>
          {{ product.description }}
        </p>
        <p>
          {{ product.price }}$
        </p>
      </div>
      <template v-if="noResultsFound">
        <h1 class="no-results">No products found matching your search</h1>
      </template>
    </div>
  </div>
</template>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 20px;
}

.search-item {
  margin: 5px;
  width: 20%;
  height: 20px;
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 10px;
}

.search-item:focus {
  outline: none;
  border: 2px solid #007bff;
  width: 50%;
}

.products-list {
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
}

.product {
  flex-basis: 28%;
  margin: 8px;
  padding: 16px;
  box-shadow: 0px 0px 14px 1px #d8d5d5;
  cursor: pointer;
  border-radius: 5px;
}

.product img {
  display: flex;
  justify-content: center;
  width: 70%;
}

.no-results {
  color: #999;
  text-align: center;
  margin-top: 20px;
}
</style>
