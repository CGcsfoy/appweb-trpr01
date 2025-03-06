<script setup lang="ts">
import "bootstrap/dist/css/bootstrap.min.css"
import "bootstrap"
import ProductList from "./components/ProductList.vue";
import Header from "./components/Header.vue";
import Footer from "./components/Footer.vue";
</script>

<template>
  <Header />
  <ProductList />
  <Footer />
</template>
<style>
body {
  background-color: #121212;
}
</style>