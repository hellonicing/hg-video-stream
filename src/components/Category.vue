<script setup>
  import { ref, toRefs } from "vue";
  const props = defineProps({
    list: Array,
  });
  const { list } = toRefs(props);
</script>

<template>
  <div class="category">
    <ul>
      <li v-for="item in list" :key="item.id">
        <video :data-video-id="item.id" loop muted :src="item.src"></video>
      </li>
    </ul>
  </div>
</template>

<style scoped lang="scss">
  .category {
    overflow: auto; // 滚动
    ul {
      display: flex;
      padding: 16px 0;
      li {
        list-style: none;
        margin-right: 12px;

        video {
          width: 136px;
          height: 240px;
          border-radius: 6px;
          background-color: #000;
        }
      }
    }
  }
</style>
