<template>
  <div class="admin-app">
    <TopNav />
    <div class="main-content">
      <router-view
        :orders="orders"
        :products="products"
        @fetchOrders="fetchOrders"
        @completeOrder="completeOrder"
        @addProductsToList="addProductsToList"
        @updateProductInList="updateProductInList"
        @getProduct="getProduct"
        @getProducts="getProducts"
      ></router-view>
    </div>
  </div>
</template>

<script>
import TopNav from './components/TopNav.vue';

const productServiceUrl = "/products/";
const singleProductServiceUrl = "/product/";
const makelineServiceUrl = "/makeline/";

export default {
  name: 'App',
  components: {
    TopNav
  },
  data() {
    return {
      orders: [],
      products: [],
      product: {}
    }
  },
  mounted() {
    this.getProducts();
  },
  methods: {
    async addProductsToList(newProduct) {
      this.products.push(newProduct);
    },
    async updateProductInList(updatedProduct) {
      const index = this.products.findIndex(product => product.id === updatedProduct.id);
      this.products[index] = updatedProduct;
    },
    async getProduct(id) {
      fetch(`${singleProductServiceUrl}${id}`)
        .then(response => response.json())
        .then(product => {
          this.product.id = product.id
          this.product.name = product.name
          this.product.image = product.image
          this.product.description = product.description
          this.product.price = product.price
        })
        .catch(error => {
          console.log(error)
          alert('Error occurred while fetching product')
        })
    },
    async getProducts() {
      fetch(`${productServiceUrl}`)
        .then(response => response.json())
        .then(products => {
          this.products = products
        })
        .catch(error => {
          console.log(error)
          alert('Error occurred while fetching products')
        })
    },
    async fetchOrders() {
      await fetch(`${makelineServiceUrl}order/fetch`)
        .then(response => response.json())
        .then(data => {
          console.log(data)
          if (data) {
            this.orders = data;
          } else {
            console.log('No orders from server');
          }
        })
        .catch(error => console.error(error));
    },
    async completeOrder(orderId) {      
      // get the order and update the status
      let order = this.orders.find(order => order.orderId === orderId);
      order.status = 1;

      let orderObject = JSON.stringify(order)
      console.log(orderObject);

      await fetch(`${makelineServiceUrl}order`, {
        method: 'PUT',
        headers: {
          'Content-Type': 'application/json'
        },
        body: orderObject
      })
        .then(response => {
          if (!response.ok) {
            alert('Error occurred while processing order')
          } else {
            alert('Order successfully processed')
            // remove the order from the list
            this.orders = this.orders.filter(order => order.orderId !== orderId);
            this.$router.go(-1);
          }
        })
        .catch(error => {
          console.log(error)
          alert('Error occurred while processing order')
        })
    }
  },
}
</script>

<style>
/* --- GLOBAL VARIABLES (Best Buy Theme) --- */
:root {
  --primary-blue: #0046be;    /* Best Buy Blue */
  --accent-yellow: #ffce00;   /* Best Buy Yellow */
  --bg-color: #f0f2f5;        /* Light Gray Background */
  --card-bg: #ffffff;
  --text-dark: #1d252c;
  --text-light: #555;
  --border-color: #e0e0e0;
  --font-main: 'Inter', -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
}

/* --- RESET & BODY --- */
body {
  margin: 0;
  padding: 0;
  font-family: var(--font-main);
  background-color: var(--bg-color);
  color: var(--text-dark);
  -webkit-font-smoothing: antialiased;
}

#app {
  text-align: center;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

.main-content {
  margin-top: 80px; /* Space for fixed nav */
  padding: 2rem;
  max-width: 1400px;
  width: 100%;
  margin-left: auto;
  margin-right: auto;
  box-sizing: border-box;
}

/* --- TYPOGRAPHY --- */
h1, h2, h3 {
  color: var(--text-dark);
}

a {
  color: var(--primary-blue);
  text-decoration: none;
  font-weight: 600;
}

a:hover {
  text-decoration: underline;
}

/* --- BUTTONS --- */
button {
  background-color: var(--primary-blue);
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 6px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s ease;
  font-size: 0.95rem;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

button:hover {
  background-color: #003da6;
  transform: translateY(-1px);
  box-shadow: 0 4px 8px rgba(0,0,0,0.15);
}

/* --- TABLES (Orders/Products List) --- */
table {
  width: 100%;
  border-collapse: separate;
  border-spacing: 0;
  background-color: white;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 4px 12px rgba(0,0,0,0.05);
  margin-top: 1rem;
}

th {
  background-color: #f8f9fa;
  color: var(--text-light);
  font-weight: 700;
  text-transform: uppercase;
  font-size: 0.85rem;
  padding: 1rem;
  border-bottom: 2px solid var(--border-color);
}

td {
  padding: 1rem;
  border-bottom: 1px solid var(--border-color);
  vertical-align: middle;
}

tr:last-child td {
  border-bottom: none;
}

tr:hover td {
  background-color: #f0f7ff; /* Very light blue on hover */
}

/* --- FORMS (Add/Edit Product) --- */
.product-form {
  background-color: white;
  padding: 2.5rem;
  border-radius: 12px;
  box-shadow: 0 8px 24px rgba(0,0,0,0.08);
  max-width: 800px;
  margin: 2rem auto;
  text-align: left;
}

.form-row {
  display: flex;
  flex-direction: column;
  margin-bottom: 1.5rem;
}

label {
  font-weight: 600;
  margin-bottom: 0.5rem;
  color: var(--text-dark);
  text-align: left;
  width: 100%;
}

input, textarea {
  padding: 12px;
  border: 1px solid var(--border-color);
  border-radius: 6px;
  font-size: 1rem;
  font-family: var(--font-main);
  transition: border-color 0.2s;
  width: 100%;
  box-sizing: border-box;
}

input:focus, textarea:focus {
  outline: none;
  border-color: var(--primary-blue);
  box-shadow: 0 0 0 3px rgba(0, 70, 190, 0.1);
}

textarea {
  min-height: 120px;
  resize: vertical;
}

/* --- UTILITY CLASSES --- */
.action-button {
  display: flex;
  justify-content: flex-end;
  margin-bottom: 1rem;
}

.order-detail {
  text-align: left;
  background: white;
  padding: 2rem;
  border-radius: 8px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.05);
  max-width: 800px;
  margin: 2rem auto;
}

.product-detail {
  background: white;
  padding: 2rem;
  border-radius: 8px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.05);
  display: flex;
  gap: 2rem;
  align-items: flex-start;
  text-align: left;
}

/* AI Button Special Style */
.ai-button {
  background-color: #007acc; /* Lighter blue/cyan */
  margin-top: 10px;
  width: auto;
  align-self: flex-start;
}

.ai-button:hover {
  background-color: #005f8b;
}

/* --- FOOTER --- */
footer {
  background-color: #1d252c;
  color: #8898aa;
  padding: 1.5rem;
  margin-top: auto;
  font-size: 0.9rem;
  position: relative; /* Unfix footer so it sits at bottom of content */
  bottom: auto;
}
</style>
