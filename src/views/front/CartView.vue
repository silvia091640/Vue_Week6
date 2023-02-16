<template>
    這是購物車頁面
    <div class="container">
        <div class="mt-4">
          <!-- 購物車列表 -->
          <table class="table align-middle">
            <thead>
              <tr>
                <th></th>
                <th>品名</th>
                <th style="width: 150px">數量/單位</th>
                <th>單價</th>
              </tr>
            </thead>
            <tbody>
              <template v-if="carts">
                <tr v-for="cart in carts.carts" :key="cart.id">
                  <td>
                    <button type="button" class="btn btn-outline-danger btn-sm"  @click="removeCartItem(cart.id)">
                      <i class="fas fa-spinner fa-pulse"></i>
                      x
                    </button>
                  </td>
                  <td>
                    {{ cart.product.title }}
                  </td>
                  <td>
                    <div class="input-group input-group-sm">
                      <div class="input-group mb-3">
                        <input min="1" type="number" class="form-control"
                             v-model.number="cart.qty" @blur="updateCart(cart)">
                        <span class="input-group-text" id="basic-addon2">{{ cart.product.unit }}</span>
                      </div>
                    </div>
                  </td>
                  <td class="text-end">
                    {{ cart.final_total}}
                  </td>
                </tr>
              </template>
            </tbody>
            <tfoot>
              <tr>
                <td colspan="3" class="text-end">總計</td>
                <td class="text-end">{{ carts.total  }}</td>
              </tr>
            </tfoot>
          </table>
        </div>
    </div>
</template>

<script>

const { VITE_APP_URL, VITE_APP_PATH } = import.meta.env

export default {
  data () {
    return {
      carts: [],
      cartData: {
        product_id: '',
        qty: 1
      }
    }
  },
  methods: {
    getCarts () {
      this.$http.get(`${VITE_APP_URL}/api/${VITE_APP_PATH}/cart`)
        .then((res) => {
          this.carts = res.data.data
        })
        .catch(error => {
          console.log(error)
        })
    },
    updateCart (item) {
      console.log(item)
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
    },
    removeCartItem (cartId) {
      this.$http.delete(`${VITE_APP_URL}/api/${VITE_APP_PATH}/cart/${cartId}`)
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
    this.getCarts()
  }
}
</script>
