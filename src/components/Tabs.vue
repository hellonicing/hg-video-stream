<script setup>
  import { ref } from "vue";
</script>

<template>
  <div class="tabs">
    <ul>
      <li>大会员</li>
      <li>消息</li>
      <li>动态</li>
    </ul>
  </div>
</template>

<style scoped lang="scss">
  .tabs {
    display: flex;
    align-items: center;
    background-color: #fff;
    width: 100%;
    height: 56px;
    padding: 16px;
    ul {
      li {
        margin-right: 8px;
        padding: 8px 16px;
        height: 32px;
        display: inline-flex;
        justify-content: center;
        align-items: center;
        border-radius: 16px;
        color: gray;
        background-color: #ddd;
        list-style: none;
      }
    }
  }
</style>
