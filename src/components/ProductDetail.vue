<template>
  <div class="detail-container">
    <div class="nav-header">
      <router-link to="/" class="back-link">
        &larr; Back to Products
      </router-link>
    </div>

    <div class="product-card-detail" v-if="productExists">
      
      <div class="image-section">
        <div class="image-wrapper">
          <img :src="product.image" :alt="product.name">
        </div>
      </div>

      <div class="info-section">
        <div class="header-group">
          <span class="product-id">SKU: {{ product.id }}</span>
          <h1>{{ product.name }}</h1>
          <div class="price-tag">{{ product.price }}</div>
        </div>

        <div class="divider"></div>

        <div class="description-group">
          <h3>Overview</h3>
          <p>{{ product.description }}</p>
        </div>

        <div class="actions-group">
          <router-link :to="`/product/${this.$route.params.id}/edit`" class="action-link">
            <button class="button secondary-btn">Edit Product Details</button>
          </router-link>
        </div>
      </div>
    </div>

    <div class="not-found-card" v-else>
      <h3>Product not found</h3>
      <p>The product you are looking for does not exist or has been removed.</p>
      <router-link to="/">
        <button class="button">Return to Store</button>
      </router-link>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'ProductDetail',
    props: ['products'],
    computed: {
      product() {
        return this.products.find(product => product.id == this.$route.params.id)
      },
      productExists() {
        return !!this.product
      }
    },
  }
</script>

<style scoped>
/* --- LAYOUT --- */
.detail-container {
  max-width: 1000px;
  margin: 0 auto;
  padding: 0 1rem;
}

.nav-header {
  margin-bottom: 1.5rem;
  text-align: left;
}

.back-link {
  color: #0046be; /* Best Buy Blue */
  text-decoration: none;
  font-weight: 600;
  font-size: 0.95rem;
  transition: color 0.2s;
}

.back-link:hover {
  color: #001e73;
  text-decoration: underline;
}

/* --- CARD DESIGN --- */
.product-card-detail {
  background-color: white;
  border-radius: 12px;
  box-shadow: 0 8px 24px rgba(0,0,0,0.08);
  display: flex;
  overflow: hidden;
  border: 1px solid rgba(0,0,0,0.05);
}

/* --- IMAGE SECTION --- */
.image-section {
  flex: 1;
  background-color: #f9f9f9;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 3rem;
  border-right: 1px solid #eee;
}

.image-wrapper img {
  max-width: 100%;
  max-height: 400px;
  object-fit: contain;
  transition: transform 0.3s ease;
}

.image-wrapper img:hover {
  transform: scale(1.05);
}

/* --- INFO SECTION --- */
.info-section {
  flex: 1;
  padding: 3rem;
  text-align: left;
  display: flex;
  flex-direction: column;
}

.header-group h1 {
  font-size: 2rem;
  margin: 0.5rem 0;
  color: #1d252c;
  line-height: 1.2;
}

.product-id {
  color: #888;
  font-size: 0.85rem;
  font-weight: 600;
  letter-spacing: 0.5px;
  text-transform: uppercase;
}

.price-tag {
  font-size: 2rem;
  font-weight: 800;
  color: #0046be; /* Best Buy Blue */
  margin-top: 1rem;
}

.divider {
  height: 1px;
  background-color: #eee;
  margin: 2rem 0;
}

.description-group h3 {
  font-size: 1.1rem;
  margin-bottom: 0.5rem;
  color: #1d252c;
}

.description-group p {
  color: #555;
  line-height: 1.6;
  font-size: 1rem;
  margin-top: 0;
}

/* --- BUTTONS --- */
.actions-group {
  margin-top: auto;
  padding-top: 2rem;
}

.button {
  padding: 12px 24px;
  border-radius: 6px;
  font-weight: 600;
  font-size: 1rem;
  cursor: pointer;
  border: none;
  transition: all 0.2s;
}

.secondary-btn {
  background-color: white;
  color: #0046be;
  border: 2px solid #0046be;
  width: 100%;
}

.secondary-btn:hover {
  background-color: #f0f4ff;
}

.action-link {
  text-decoration: none;
  display: block;
}

/* --- NOT FOUND STATE --- */
.not-found-card {
  text-align: center;
  padding: 4rem;
  background: white;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.05);
}

.not-found-card h3 {
  font-size: 1.5rem;
  color: #333;
}

/* --- RESPONSIVE --- */
@media (max-width: 768px) {
  .product-card-detail {
    flex-direction: column;
  }

  .image-section {
    border-right: none;
    border-bottom: 1px solid #eee;
    padding: 2rem;
  }

  .info-section {
    padding: 2rem;
  }
}
</style>
