<template>
  <main class="main">
    <nav class="header">
      <h1 class="header__primary">Online Shopping</h1>
      <div class="header__cart">
        <span class="material-icons"> shopping_cart </span>
        <p>
          Cart <span class="header__cart--count">{{ cartItemCount }}</span>
        </p>
      </div>
    </nav>

    <!-- products and cart section-->
    <section class="row">
      <div class="product-listing">
        <div class="row__icons">
          <span class="material-icons">apps</span>
          <span class="material-icons">view_list</span>
        </div>
        <div class="product-listing__row">
          <div
            class="product-listing__card"
            v-for="product in products"
            :key="product.id"
          >
            <div class="img-container">
              <img
                class="img"
                :src="`/img/${product.productImage}`"
                alt="Image"
              />
            </div>
            <div class="product-listing__details">
              <h1 class="product-name">
                {{ product.name
                }}<span
                  class="product-availability"
                  v-if="product.stock === 'IN STOCK'"
                  >{{ product.stock }}</span
                >
                <span
                  class="product-availability product-availability--out"
                  v-else
                  >{{ product.stock }}</span
                >
              </h1>
              <p class="product-price">₹{{ product.price }}</p>

              <button
                class="btn"
                @click="addToCart(product)"
                :disabled="checkCart(product.id)"
                :class="{ 'btn--out': checkCart(product.id) }"
              >
                Add to cart
              </button>
            </div>
          </div>
        </div>
      </div>

      <!-- Cart Section -->
      <div class="cart">
        <h1 cart__heading>YOUR SHOPPING CART</h1>
        <div class="cart__row" v-if="cartItems.length">
          <div
            class="cart__block"
            v-for="cartItem in cartItems"
            :key="cartItem.id"
          >
            <h2 class="cart__product-name">{{ cartItem.name }}</h2>
            <div>
              <p class="cart__product-price">
                ₹{{ cartItem.price }}
                <button class="cart__delete" @click="removeItem(cartItem.id)">
                  &times;
                </button>
              </p>
            </div>
          </div>

          <div class="cart__block">
            <p class="cart__total cart__product-name">Total</p>
            <!-- <p class="cart__total-value">₹{{ total }}</p> -->
            <p class="cart__total-value">₹{{ addTotalCost }}</p>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>

<script>
export default {
  name: "App",

  data() {
    return {
      // product details
      products: [
        {
          id: 1,
          name: "Nike Sport",
          subtitle: "Unisex sport can",
          price: 3500,
          stock: "IN STOCK",
          productImage: "1.jpg",
        },
        {
          id: 2,
          name: "Long Sleeve",
          subtitle: "Men office shirt",
          price: 3600,
          stock: "IN STOCK",
          productImage: "2.jpg",
        },
        {
          id: 3,
          name: "Smart Watch",
          subtitle: "Smart watch for smart people",
          price: 300,
          stock: "OUT OF STOCK",
          productImage: "3.jpg",
        },
        {
          id: 4,
          name: "Gold Chain",
          subtitle: "Women gold chain",
          price: 3500,
          stock: "IN STOCK",
          productImage: "4.jpg",
        },
        {
          id: 5,
          name: "Polo T-Shirt",
          subtitle: "White polo t-shirt for men",
          price: 3500,
          stock: "OUT OF STOCK",
          productImage: "5.jpg",
        },
        {
          id: 6,
          name: "Bridal Gown",
          subtitle: "White bridal gown",
          price: 3500,
          stock: "IN STOCK",
          productImage: "6.jpg",
        },
        {
          id: 7,
          name: "Necklace",
          subtitle: "Gold neckalce for women",
          price: 3500,
          stock: "IN STOCK",
          productImage: "7.jpg",
        },
        {
          id: 8,
          name: "Leather Shoe",
          subtitle: "Pure leather mocassin for men",
          price: 3500,
          stock: "OUT OF STOCK",
          productImage: "8.jpg",
        },
        {
          id: 9,
          name: "Leather Purse",
          subtitle: "Simple female purse",
          price: 3500,
          stock: "IN STOCK",
          productImage: "9.jpg",
        },
      ],
      cartItemCount: 0,
      cartItems: [],
      total: 0,
    };
  },

  methods: {
    // Check if Item already exist in the cart. If it has been added, do not addd.
    checkCart(id) {
      const check = this.cartItems;
      return check.find((item) => item.id === id);

    },

    // add items to cart when clicked
    addToCart(product) {
      this.cartItems.push(product);
      this.cartItemCount++;
    },

    // Removing items from cart
    removeItem(id) {
      this.cartItems = this.cartItems.filter((cartItem) => {
        return cartItem.id !== id;
      });
      this.cartItemCount--;
    },
  },

  // Computing the total cost of items in the cart
  computed: {
    addTotalCost() {
      const totalPrice = this.cartItems.map((el) => el.price);
      return totalPrice.reduce((acc, price) => acc + price, 0);
    },
  },
};
</script>

<style lang="scss">
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #66c57a;
  color: #fff;
  padding: 1.5rem;
  font-family: "Lato", sans-serif;

  &__primary {
    font-size: 1.8rem;
  }

  &__cart {
    display: flex;
    justify-content: space-between;
    // align-items: center;

    p {
      font-size: 1.6rem;
    }

    &--count {
      background: red;
      padding: 0.5rem 0.8rem;
      border-radius: 100rem;
      font-size: 1.2rem;
      font-weight: 600;
      // display: inline-block;
    }
  }
}

// row section
.row {
  display: flex;

  @media only screen and (max-width: 37.5em) {
    flex-direction: column;
  }

  &__icons {
    display: flex;
    justify-content: flex-end;
    position: relative;
    top: 1rem;
    right: 2.5rem;
  }

  .product-listing {
    flex-basis: 70%;
    background-color: #ddd;
    // height: 100vh;

    &__row {
      display: flex;
      flex-direction: column;
      align-items: center;
      margin: 2rem 2rem;

      @media only screen and (max-width: 56.25em) {
        display: grid;
        grid-template-columns: repeat(2, 1fr);
        gap: 2rem;
      }

      @media only screen and (max-width: 35em) {
        display: flex;
        flex-direction: column;
      }
    }

    &__card {
      display: flex;
      background: var(--white);
      width: 100%;
      box-shadow: 0 15px 20px rgba(0, 0, 0, 0.3);

      @media only screen and (max-width: 56.25em) {
        display: grid;
        grid-template-columns: repeat(2, 1fr);
        gap: 2rem;
      }

      .img-container {
        // border-radius: 1rem;
        // background-color: black;

        img {
          object-fit: cover;
          width: 100%;
          display: block;
        }
      }
    }

    &__details {
      display: flex;
      flex-direction: column;
      position: relative;
      top: 2rem;
      left: 5rem;

      @media only screen and (max-width: 56.25em) {
        left: 2rem;
        height: 20rem;
      }

      .product-name {
        font-size: 1.5rem;
        margin-left: 2rem;
      }

      .product-price {
        margin-top: 1.3rem;
        margin-left: 2rem;
        font-size: 1.5rem;
        font-weight: 600;

        @media only screen and (max-width: 56.25em) {
          margin: 1rem;
        }
      }

      .product-availability {
        font-size: 1rem;
        font-weight: 600;
        padding: 0.4rem 1rem;
        border-radius: 1rem;
        background-color: rgba(255, 255, 0, 0.35);
        position: relative;
        left: 1rem;

        @media only screen and (max-width: 56.25em) {
          left: -8rem;
          top: 2rem;
          display: none;
        }

        &--out {
          background: #ddd;
          padding: 0.4rem;
        }
      }
    }
  }

  .cart {
    flex-basis: 30%;
    background: var(--white);
    padding: 2rem;

    h1 {
      color: #333fff;
    }

    &__row {
      display: flex;
      flex-direction: column;
    }

    &__block {
      display: flex;
      align-items: center;
      gap: 10rem;
      padding-top: 1.4rem;

      @media only screen and (max-width: 56.25em) {
        gap: 3rem;
      }

      .cart__product-name {
        font-size: 1.5rem;
        font-weight: 600;
        color: #777;
      }

      .cart__total-value {
        font-size: 2rem;
        font-weight: 900;
        color: #333fff;
        position: relative;
        left: 5rem;

        @media only screen and (max-width: 56.25em) {
          left: 1rem;
        }

        @media only screen and (max-width: 56.25em) {
          left: 2rem;
        }
      }
      div {
        .cart__product-price {
          font-size: 1.5rem;
          font-weight: 600;
        }

        .cart__delete {
          display: inline-block;
          font-size: 1.6rem;
          // background: #777;
          position: relative;
          left: 1.6rem;
          cursor: pointer;
          outline: none;
          border: none;
          padding: 0.1rem 0.4rem;
        }
      }
    }
  }
}

.btn {
  font-size: 1.3rem;
  font-weight: 600;
  text-decoration: none;
  color: var(--white);
  background-color: var(--color-sec);
  padding: 1rem 5rem;
  border-radius: 3rem;
  text-align: center;
  cursor: pointer;
  position: relative;
  left: 45rem;
  bottom: 4rem;
  border: none;
  outline: none;

  @media only screen and (max-width: 56.25em) {
    left: -3rem;
    bottom: -4rem;
    padding: 1rem;
  }

  &--out {
    background: #777;
  }
}

.line {
  margin: 1rem 0;
  width: 75%;
  height: 1px;
  background-color: #e0e0e0;
}
</style>
