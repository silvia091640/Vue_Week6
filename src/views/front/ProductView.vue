<template>
    這是單一產品頁面
</template>

<script>
const { VITE_APP_URL, VITE_APP_PATH } = import.meta.env

export default {
  data () {
    return {
      prodcut: {}
    }
  },
  methods: {
    getProduct () {
      const { id } = this.$route.params
      this.$http.get(`${VITE_APP_URL}/api/${VITE_APP_PATH}/product/${id}`)
        .then((res) => {
          console.log(res)
        })
    }

  },
  mounted () {
    this.getProduct()
  }
}
</script>
