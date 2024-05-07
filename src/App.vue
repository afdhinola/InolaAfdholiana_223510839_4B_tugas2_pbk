<template>
  <div class="background">
    <div class="container">
      <h1>Product Catalog</h1>

      <!-- Form untuk menambahkan produk -->
      <form @submit.prevent="addProduct">
        <div class="form-group">
          <label for="name">Product Name:</label>
          <input type="text" id="name" v-model="newProduct.name" placeholder="Enter product name">
        </div>

        <div class="form-group">
          <label for="category">Category:</label>
          <input type="text" id="category" v-model="newProduct.category" placeholder="Enter category">
        </div>

        <button type="submit">Add Product</button>
      </form>

      <!-- Input teks untuk pencarian -->
      <div class="form-group">
        <label for="search">Search:</label>
        <input type="text" id="search" v-model="searchTerm" placeholder="Search products">
        
        <label for="filter">Filter by Category:</label>
        <select id="filter" v-model="selectedCategory">
          <option value="">All</option>
          <option v-for="category in categories" :value="category" :key="category">{{ category }}</option>
        </select>
      </div>

      <!-- Daftar produk -->
      <ul v-if="filteredProducts.length > 0">
        <li v-for="(product, index) in filteredProducts" :key="index" class="product-item">
          <!-- Tampilkan nama produk dan kategori -->
          <div v-if="index !== editedIndex">
            <span>{{ product.name }}</span>
            <span>( {{ product.category }} )</span>
            <br>
            <button @click="editProduct(index)">Edit</button>
            <button @click="removeProduct(index)">Remove</button>
          </div>
          <!-- Saat di-edit, tampilkan input teks -->
          <div v-else>
    <input type="text" v-model="editedProduct.name" placeholder="Edit product name">
    <div>{{ editedProduct.name }}</div> <!-- Menampilkan editan teks di bawah input -->
    
    <input type="text" v-model="editedProduct.category" placeholder="Edit category">
    <div>{{ editedProduct.category }}</div> <!-- Menampilkan editan teks di bawah input -->
    
    <button @click="saveProduct(index)">Save</button>
    <button @click="cancelEdit">Cancel</button>
</div>

        </li>
      </ul>

      <!-- Tampilkan pesan jika tidak ada produk yang ditemukan -->
      <div v-else class="no-products">No products found.</div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      products: [],
      newProduct: { name: '', category: '' },
      editedIndex: -1,
      editedProduct: { name: '', category: '' },
      searchTerm: '',
      selectedCategory: '',
      categories: []
    };
  },
  computed: {
    filteredProducts() {
      let filtered = this.products.filter(product =>
        product.name.toLowerCase().includes(this.searchTerm.toLowerCase()) &&
        (this.selectedCategory === '' || product.category === this.selectedCategory)
      );
      return filtered;
    }
  },
  methods: {
    addProduct() {
      if (this.newProduct.name.trim() !== '' && this.newProduct.category.trim() !== '') {
        this.products.push({ ...this.newProduct });
        this.newProduct.name = '';
        this.newProduct.category = '';
        this.categories = [...new Set(this.products.map(product => product.category))];
      }
    },
    removeProduct(index) {
      this.products.splice(index, 1);
    },
    editProduct(index) {
      this.editedIndex = index;
      this.editedProduct = { ...this.products[index] };
    },
    saveProduct(index) {
      this.products[index] = { ...this.editedProduct };
      this.cancelEdit();
    },
    cancelEdit() {
      this.editedIndex = -1;
      this.editedProduct = { name: '', category: '' };
    }
  }
};
</script>

<style scoped>
.background {
  background-image: url('./assets/product.png') ;
  background-size: 800px 800px; 
  background-position: right -50px; 
  background-repeat: no-repeat;
  background-color: transparent;
}

.container {
  max-width: 400px;
  height: 650px;
  margin-top: 25px;
  margin-left: 100px ;
  padding: 20px;
  border: 1px solid rgb(223, 134, 134);
  border-radius: 15px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  background-color: rgb(223, 134, 134);
  font-family:system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
}

h1 {
  font-size: 28px;
  margin-bottom: 30px;
  font-weight: bold;
  text-align: center;
  color: #5B1F3D;
}

.form-group {
  margin-bottom: 20px;
  color: #5B1F3D;
}

label {
  display: block;
  margin-bottom: 10px;
  margin-top: 10px;
}

input[type="text"],
select {
  width: calc(100% - 10px);
  padding: 8px;
  border-radius: 4px;
  border: 1px solid #5B1F3D;
}

button[type="submit"],
button {
  padding: 8px 16px;
  border-radius: 4px;
  border: none;
  background-color: #A7586D;
  color: #210111;
  cursor: pointer;
  margin-bottom: 10px;
  margin-right: 10px;
  margin-top: 5px;
}

button[type="submit"]:hover,
button:hover {
  background-color: #742F4C;
}

.product-item span {
  margin-right: 3px;
  color: #5B1F3D;
}

ul {
  padding: 0;
}

li {
  margin-bottom: 20px;
  padding: 10px;
  border: 1px solid #5B1F3D;
  border-radius: 4px;
}

li:hover {
  box-shadow: 0 2px 4px rgba(33, 226, 40, 0.1);
}

.no-products {
  font-style: italic;
  color: #c40000;
}
</style>
