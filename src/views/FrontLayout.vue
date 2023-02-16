<template>
<div class="container">
    你現在在前台頁面
      <router-link to="/">Home</router-link> |
      <router-link to="/products">前台產品列表</router-link> |
      <router-link to="/cart">購物車</router-link> |
    <hr>
    <RouterView></RouterView>
    </div>
</template>

<script>
import { RouterView } from 'vue-router'

export default {
  components: {
    RouterView
  },
  methods: {
    logout () {
      document.cookie = `hexschoolToken=; expires=${new Date()};`
      this.$$route.push('login')
    }
  }
}
</script>
