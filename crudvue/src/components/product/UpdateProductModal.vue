<template>
  <div class="modal-container" v-if="show">
    <div class="modal-content bg-white w-1/3 rounded-lg p-10">
      <form @submit.prevent="submitForm" class="grid grid-cols-2 gap-4 item-center p-5">
        <div>
          <label class="modal-label" for="name">Product Name</label>
          <input required class="modal-input" type="text" id="name" v-model="name" />
        </div>
        <div>
          <label class="modal-label" for="bname">Product Description</label>
          <input required class="modal-input" type="text" id="bname" v-model="description" />
        </div>
        <div>
          <label class="modal-label" for="price">Price</label>
          <input required class="modal-input" type="number" id="price" v-model="price" />
        </div>
        <div>
          <label class="modal-label" for="quantity">Quantity in stock</label>
          <input required class="modal-input" type="number" id="quantity" v-model="quantity_in_stock" />
        </div>
        <div class="col-span-2">
          <div class="btn flex justify-end mt-4">
            <button type="submit" class="modal-button add-merchant-button">Update</button>
            <button class="modal-button cancel-button" @click="change()">cancel</button>
          </div>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  props: {
    show: {
      type: Boolean,
      default: false,
    },
    close_update_modal: {
      type: Function,
      default: () => { },
    },
    product: {
      type: Object,
      required: true,
    },

  },
  data() {
    return {
      name: '',
      description: '',
      price: 0,
      quantity_in_stock: 0,
    }
  },
  mounted() {
      this.name = this.product?.name,
      this.description = this.product?.description,
      this.price = this.product?.price,
      this.quantity_in_stock = this.product?.quantity_in_stock
  },
  methods: {
    submitForm() {

      const newProduct = {
        name: this.name,
        description: this.description,
        price: this.price,
        quantity_in_stock: this.quantity_in_stock,
      };

      axios.put(`http://localhost:8000/api/products/${this.product.id}`, newProduct)
        .then(response => {
          console.log('Product updated:', response.data);
          // Handle any additional actions after the update
        })
        .catch(error => {
          console.error('Error updating product:', error);
        });;
        this.close_update_modal();
        location.reload();
    },
    change() {
      this.close_update_modal();
    },
  },
};
</script>

<style scoped>
.modal-container {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 50;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  overflow: auto;
  background-color: rgba(128, 128, 128, 0.5);
}

/* Styles for the modal content */
.modal-content {
  background-color: #fff;
  width: 43.33%;
  height: 55%;
  border-radius: 8px;
}

/* Styles for the form within the modal */
form {
  padding: 40px;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 16px;
}

/* Styles for the form input fields */
.modal-input {
  width: 100%;
  padding: 6px;
  border: 1px solid #ccc;
  border-radius: 4px;
  background-color: #f3f1f1;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05);
  outline: none;
  transition: border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
}

/* Styles for the form labels */
.modal-label {
  font-weight: bold;
  margin-bottom: 8px;
}

/* Styles for the error message */
.modal-error {
  color: #e53e3e;
  margin-top: 20px;
}

/* Styles for the buttons */
.modal-button {
  padding: 6px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  outline: none;
}

.btn {
  display: flex;
  flex-direction: row;
  justify-content: space-evenly;
}

.add-merchant-button {
  background-color: #1a202c;
  color: #fff;
  width: 100%;
}

.cancel-button {
  background-color: #e2e8f0;
  color: #4a5568;
  margin-left: 8px;
  width: 100%;
}
</style>
