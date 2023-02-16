<template>
    這是產品頁面
    <div class="container">
        <div class="mt-4">
          <table class="table align-middle">
            <thead>
              <tr>
                <th>圖片</th>
                <th>商品名稱</th>
                <th>價格</th>
                <th></th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="item in products" :key="item.id">
                <td style="width: 200px">
                  <div style="height: 100px; background-size: cover; background-position: center"
                  :style="{backgroundImage: `url(${item.imageUrl})`}"></div>
                </td>
                <td>
                  {{ item.title }}
                </td>
                <td>
                  <div class="h5">{{ item.origin_price }} 元</div>
                  <del class="h6">原價 {{ item.origin_price }} 元</del>
                  <div class="h5">現在只要 {{ item.price }} 元</div>
                </td>
                <td>
                  <div class="btn-group btn-group-sm">
                    <RouterLink :to="`/product/${item.id}`" class="btn btn-outline-secondary">查看產品</RouterLink>
                    <button type="button" class="btn btn-outline-danger"
                    @click="addCart(item.id)">
                      <i class="fas fa-spinner fa-pulse"></i>
                      加到購物車
                    </button>
                  </div>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
    </div>
</template>

<script>
import { RouterLink } from 'vue-router'
const { VITE_APP_URL, VITE_APP_PATH } = import.meta.env

export default {
  data () {
    return {
      products: [],
      carts: [],
      cartData: {
        product_id: '',
        qty: 1
      },
      isLoading: false
    }
  },
  components: {
    RouterLink
  },
  methods: {
    getAllProducts () {
      // this.isLoading = true,
      this.$http.get(`${VITE_APP_URL}/api/${VITE_APP_PATH}/products/all`)
        .then((res) => {
          this.products = res.data.products
          this.isLoading = false
        })
        .catch(error => {
          console.log(error)
        })
    },
    getCarts () {
      this.$http.get(`${VITE_APP_URL}/api/${VITE_APP_PATH}/cart`)
        .then((res) => {
          this.carts = res.data.data
        })
        .catch(error => {
          console.log(error)
        })
    },
    addCart (productId, qty = 1) {
      this.cartData.product_id = productId
      this.cartData.qty = qty
      this.$http.post(`${VITE_APP_URL}/api/${VITE_APP_PATH}/cart`, { data: this.cartData })
        .then((res) => {
          alert(res.data.message)
          this.getCarts()
        })
        .catch(error => {
          console.log(error)
        })
    },
    updateCart (item) {
      this.cartData.product_id = item.id
      this.cartData.qty = item.qty
      this.$http.put(`${VITE_APP_URL}/api/${VITE_APP_PATH}/cart/${item.id}`, { data: this.cartData })
        .then((res) => {
          this.getCarts()
          alert(res.data.message)
        })
        .catch(error => {
          console.log(error)
        })
    }
  },
  mounted () {
    // const token = document.cookie.replace(/(?:(?:^|.*;\s*)hexschoolToken\s*\=\s*([^;]*).*$)|^.*$/, '$1')

    // this.$http.defaults.headers.common.Authorization = token

    this.getAllProducts()

    //   this.getCarts();
    // this.productModal = new bootstrap.Modal(this.$refs.productModal)
    //   this.isLoading = true;
    //   this.createProduct();
  }
}
</script>
