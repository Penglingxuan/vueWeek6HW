<template>
  <div>
    <h1>好ㄘ的甜點</h1>
    <Loading :active="isLoading"></Loading>
    <div class="d-flex flex-wrap justify-content-center">
      <div v-for="item in products" :key="item.id" class="card me-4 mb-4" style="width: 18rem;">
        <img :src="item.imageUrl" class="card-img-top h-50" alt="">
        <div class="card-body h-50 d-flex flex-column justify-content-center">
          <h5 class="card-title">{{ item.title }}</h5>
          <div class="card-text">
            <div class="h5" v-if="!item.price">{{ item.origin_price }} 元</div>
            <del class="h6" v-if="item.price">原價 {{ item.origin_price }} 元</del>
            <div class="h5" v-if="item.price">現在只要 {{ item.price }} 元</div>
          </div>
          <div class="btn-group btn-group-sm">
            <button
              type="button"
              class="btn btn-outline-secondary"
              @click="getProduct(item.id)"
              :disabled="loadingStatus.loadingItem === item.id">
              <i
                class="fas fa-spinner fa-pulse"
                v-if="loadingStatus.loadingItem === item.id"></i>
              查看更多
            </button>
            <button
              type="button"
              class="btn btn-outline-danger"
              @click="addToCart(item.id)"
              :disabled="loadingStatus.loadingItem === item.id"
            >
              <i
                class="fas fa-spinner fa-pulse"
                v-if="loadingStatus.loadingItem === item.id"
              ></i>
              加到購物車
            </button>
          </div>
        </div>
      </div>
    </div>
    <UserProductModal
      ref="userProductModal"
      :product="product"
      @add-to-cart="addToCart"
    ></UserProductModal>
  </div>
</template>

<script>
import UserProductModal from "@/components/UserProductModal.vue";

export default {
  name: "Products",
  data() {
    return {
      products: [],
      loadingStatus: {
        loadingItem: "",
      },
      isLoading: false,
      product: {},
    };
  },
  components: {
    UserProductModal,
  },
  mounted() {
    this.getProducts();
  },
  methods: {
    addToCart(id, qty = 1) {
      this.isLoading = true;
      const url = `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/cart`;
      this.loadingStatus.loadingItem = id;
      const cart = {
        product_id: id,
        qty,
      };
      this.$http
        .post(url, { data: cart })
        .then((response) => {
          alert(response.data.message);
          this.loadingStatus.loadingItem = "";
          this.$refs.userProductModal.hideModal();
          this.isLoading = false;
        })
        .catch((err) => {
          alert(err.data.message);
        });
    },
    getProducts() {
      this.isLoading = true;
      const url = `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/products`;
      this.$http
        .get(url)
        .then((response) => {
          this.products = response.data.products;
          this.isLoading = false;
        })
        .catch((err) => {
          alert(err.data.message);
        });
    },
    getProduct(id) {
      this.isLoading = true;
      const url = `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/product/${id}`;
      this.loadingStatus.loadingItem = id;
      this.$http
        .get(url)
        .then((response) => {
          this.loadingStatus.loadingItem = "";
          this.product = response.data.product;
          this.isLoading = false;
          this.$refs.userProductModal.openModal();
        })
        .catch((err) => {
          alert(err.data.message);
        });
    },
  },
};
</script>