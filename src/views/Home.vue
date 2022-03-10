<template>
  <div class="home">
    <div id="nav">
      <ul class="nav nav-pills justify-content-end">
      <li class="nav-item pe-4">
        <router-link to="/">首頁 </router-link> 
      </li>
      <li class="nav-item pe-4">
        <router-link to="/products">前台產品列表 </router-link>
      </li>
      <li class="nav-item pe-4">
        <router-link to="/cart">前台購物車 </router-link>
      </li>
      <li class="nav-item pe-4">
        <router-link to="/admin/products">後台購物車列表 </router-link>
      </li>
    </ul>
    </div>
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: 'Home',
  data() {
    return {
    };
  },
};
</script>