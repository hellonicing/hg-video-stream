<script setup>
  import NavBar from "./components/NavBar.vue";
  import Tabs from "./components/Tabs.vue";
  import Category from "./components/Category.vue";
</script>

<template>
  <div class="App">
    <NavBar> </NavBar>
    <Tabs />
    <h2>热门</h2>
    <Category />
    <h2>直播</h2>
    <Category />
    <h2>推荐</h2>
    <Category />
  </div>
</template>

<style scoped></style>
