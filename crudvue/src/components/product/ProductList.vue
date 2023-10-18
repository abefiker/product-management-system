<template>
  <div>
    <AddButton @onClick()="openModal" text="Add Product" />
    <AddProductModal :show="showModal" :close_modal="closeModal" />
    <div v-if="mount">
      <UpdateProductModal :show="showUpdateModal"
       :close_update_modal="closeUpdateModal"
       :product="selectedProduct" />
    </div>
    <table class="table-auto">
      <thead class="bg-gray-200">
        <tr>
          <th>Product Name</th>
          <th>Description</th>
          <th>Price</th>
          <th>Quantity in Stock</th>
          <th>Action</th>
        </tr>
      </thead>
      <tbody>

        <tr v-for="product in products" :key="product.id">
          <td>{{ product.name }}</td>
          <td class="desctd">{{ product.description }}</td>
          <td>{{ product.price }}</td>
          <td>{{ product.quantity_in_stock }}</td>
          <td class="btntd">
            <button class="btn btn-primary" @click="updateModal(product)" >update</button>
          <button @click="onDelete(product.id)" class="btn btn-danger">delete</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
<script>
import AddProductModal from '@/components/product/AddProductModal.vue';
import AddButton from './AddButton.vue';
import UpdateProductModal from './UpdateProductModal.vue';
import axios from 'axios';

export default {
  name: 'ProductList',
  components: {
    AddButton,
    AddProductModal,
    UpdateProductModal,
  },
  data() {
    return {
      products: [],
      showModal: false,
      showUpdateModal: false,
      selectedProduct: null,
      mount : false,
    };
  },
  async mounted() {
    await axios.get('http://localhost:8000/api/products')
      .then(response => {
        this.products = response.data
      })
      .catch(error => {
        console.error('Error reading product:', error);
      });
  },
  methods: {
    async onDelete(id) {
      await axios.delete(`http://localhost:8000/api/products/${id}`)
        .then(response => {
          console.log('Product deleted:', response.data);
          // Handle any additional actions after the delete
        })
        .catch(error => {
          console.error('Error deleting product:', error);
        });
        location.reload();
    },
    openModal() {
      this.showModal = !this.showModal;
      this.name = '';
      this.description = '';
      this.price = 0;
      this.quantity_in_stock = 0;
    },
    closeModal() {
      this.showModal = false;
    },
    updateModal(product) {
      this.showUpdateModal = !this.showUpdateModal;
      console.log(product);
      this.selectedProduct = product;
      this.mount = true;
    },
    closeUpdateModal() {
      this.showUpdateModal = false;
    },
  },
};
</script>
<style scoped>
table {
  margin: 0 auto;
}

th,
td {
  padding: 6px 14px;
}
.desctd{
  width: 25%;
}
.btntd{
  display: flex;
  flex-direction: row;
  justify-content: space-evenly;

}
</style>
