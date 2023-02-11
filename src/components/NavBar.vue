<script setup>
  import { toRefs } from "vue";
  import { LeftOutlined } from "@ant-design/icons-vue";
  const props = defineProps({
    title: String,
  });
  const { title } = toRefs(props);
</script>

<template>
  <div class="navBar">
    <span class="icon"><LeftOutlined /></span>
    <span>{{ title }}</span>
  </div>
</template>

<style scoped lang="scss">
  .navBar {
    position: relative;
    width: 100%;
    height: 56px;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: rgb(251, 114, 153);
    .icon {
      position: absolute;
      top: 50%;
      left: 16px;
      transform: translateY(-50%);
    }
  }
</style>
