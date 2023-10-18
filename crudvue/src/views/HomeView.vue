<template>
  <div class="home">
    <HelloWorld msg="Product Managment System"/>
    <ProductList />
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue';
import ProductList from '@/components/product/ProductList.vue';

export default {
  name: 'HomeView',
  components: {
    HelloWorld,
    ProductList,

  },
};
</script>
