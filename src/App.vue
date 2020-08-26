<template>
  <body>
    <div id="app2">
      <div class="product">

        <div class="product-image">
          <img v-bind:src="image">
        </div>

        <div class="product-info">
          <h1>{{ title }}</h1>
          <span v-if="onSale">{{ isOnSale }}</span>
          <p v-if="inStock > 10"> In Stock</p>
          <p v-else-if="inStock <= 10 && inStock > 0">Almost Gone</p>
          <p v-else :class="{ outOfStock: !inStock}"> Out of Stock</p>

          <ul>
            <li v-for="detail in details">{{ detail }}</li>
          </ul>

          <div v-for="(variant, index) in variants" 
               :key="variant.variantID"
               class="color-box"
               :style="{ backgroundColor: variant.variantColor }"
               @mouseover="updateProduct(index)">
          </div>

          <ul>
            <li v-for="size in sizes">{{ size }}</li>
          </ul>

          <button v-on:click="addToCart" 
                  :disabled="!inStock"
                  :class="{ disabledButton: !inStock}">Add to Cart</button>

          <button v-if="cart > 0" v-on:click="removeFromCart">Remove 1</button>

          <div class="cart">
            <p>Cart({{cart}})</p>
          </div>

        </div>

      </div>

      <div>
        <h2>Reviews</h2>
        <p v-if="!reviews.length">There are no reviews yet.</p>
        <ul>
          <li v-for="review in reviews">
            <p>{{ review.name }}</p>
            <p>Rating: {{ review.rating }}</p>
            <p>{{ review.review }}</p>
          </li>
        </ul>
      </div>

    </div>
    <product-review @review-submitted="addReview"></product-review>
  </body>
</template>

<script>
import ProductReview from "./components/ProductReview";
export default {
  name: 'App',
  components: {
    ProductReview
  },
  data: function() {
    return {
      brand: 'Apple',
      product: 'Socks',
      selectedVariant: 0,
      onSale: true,
      details: ["80% cotton", "20% polyester", "Gender-neutral"],
      variants: [
        {
          variantID: 2234,
          variantColor: "green",
          variantImage: require("./assets/vmSocks-green-onWhite.jpg"),
          variantQuantity: 20
        },
        {
          variantID: 2235,
          variantColor: "blue",
          variantImage: require("./assets/vmSocks-blue-onWhite.jpg"),
          variantQuantity: 0
        }
      ],
      sizes: ["L", "M", "S"],
      cart: 0,
      reviews: []
    }
  },
  methods: {
    addToCart: function () {
      this.cart += 1
    },
    removeFromCart: function () {
      this.cart -= 1
    },
    updateProduct: function (index) {
      this.selectedVariant = index
      console.log(index)
    },
    addReview: function (ProductReview) {
      this.reviews.push(ProductReview)
    }
  },
  computed: {
    title() {
      return this.brand + ' ' + this.product
    },
    image() {
      return this.variants[this.selectedVariant].variantImage
    },
    inStock() {
      return this.variants[this.selectedVariant].variantQuantity
    },
    isOnSale() {
      return this.brand + ' ' + this.product + ' is on sale!'
    }
  }
}

</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
 body {
    font-family: tahoma;
    color:#282828;
    margin: 0px;
  }
  
  .nav-bar {
    background: linear-gradient(-90deg, #84CF6A, #16C0B0);
    height: 60px;
    margin-bottom: 15px;
  }

  .product {
    display: flex;
    flex-flow: wrap;
    padding: 1rem;
  }

  img {
    border: 1px solid #d8d8d8;
    width: 70%;
    margin: 40px;
    box-shadow: 0px .5px 1px #d8d8d8;
  }
  
  .product-image {
    width: 80%;
  }
  
  .product-image,
  .product-info {
    margin-top: 10px;
    width: 50%;
  }
  
  .color-box {
    width: 40px;
    height: 40px;
    margin-top: 5px;
  }
  
  .cart {
    margin-right: 25px;
    float: right;
    border: 1px solid #d8d8d8;
    padding: 5px 20px;
  }
  
  button {
    margin-top: 30px;
    border: none;
    background-color: #1E95EA;
    color: white;
    height: 40px;
    width: 100px;
    font-size: 14px;
  } 
  
  .disabledButton {
    background-color: #d8d8d8;
  }
  
  .review-form {
    width: 400px;
    padding: 20px;
    margin: 40px;
    border: 1px solid #d8d8d8;
  }
  
  input {
    width: 100%;  
    height: 25px;
    margin-bottom: 20px;
  }
  
  textarea {
    width: 100%;
    height: 60px;
  }

  .tab {
    margin-left: 20px;
    cursor: pointer;
  }

  .activeTab {
    color: #16C0B0;
    text-decoration: underline;
  }

  .outOfStock {
    text-decoration: line-through;
  }
</style>
