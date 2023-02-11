<script setup>
  import NavBar from "./components/NavBar.vue";
  import Tabs from "./components/Tabs.vue";
  import Category from "./components/Category.vue";
  import { dataSource } from "./constants/data.js";
  import { ref } from "vue";

  const BannerImgSrc = new URL("./assets/banner.png", import.meta.url).href;
  const FoooterImgSrc = new URL("./assets/footer.jpg", import.meta.url).href;

  const contentRef = ref(null);
  const oldYRef = ref(0);
  const hiddenSate = ref(false);
  const useHidden = (bool) => {
    return (hiddenSate.value = bool);
  };
  const scroll = () => {
    if (contentRef.value) {
      const { top: newY } = contentRef.value.getBoundingClientRect();
      // console.log(contentRef.value.getBoundingClientRect());
      const delta = newY - oldYRef.value;
      oldYRef.value = newY;
      // console.log(delta);
      if (delta < 0) {
        // 向下滚动 隐藏
        useHidden(true);
        // console.log("隐藏", hiddenSate.value);
      } else {
        useHidden(false);
        // 向上滚动   显示
        // console.log("显示", hiddenSate.value);
      }
    }
  };
</script>

<template>
  <div class="App">
    <header class="header" :class="{ hidden: hiddenSate }">
      <NavBar title="首页" />
      <Tabs />
    </header>

    <div class="line"></div>

    <div class="scrollView" @scroll="scroll">
      <img class="banner" :src="BannerImgSrc" alt="Banner" />
      <div class="content" ref="contentRef">
        <h2>{{ dataSource.hot.title }}</h2>
        <Category :list="dataSource.hot.list" />
        <h2>{{ dataSource.recommend.title }}</h2>
        <Category :list="dataSource.recommend.list" />
        <h2>{{ dataSource.live.title }}</h2>
        <Category :list="dataSource.live.list" />
      </div>
      <img class="footerImg" :src="FoooterImgSrc" alt="Foooter" />
      <footer class="footer"><span> @Bolobili </span></footer>
    </div>
  </div>
</template>

<style scoped lang="scss">
  .App {
    background-color: #fb7299;
    color: #fff;

    .header {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      transform: translateY(0);
      transition: transform 250ms;
      &.hidden {
        transform: translateY(-56px);
      }
    }
    .line {
      position: fixed;
      top: 50%;
      left: 0;
      width: 100%;
      height: 4px;
      background-color: red;
    }
    .scrollView {
      height: 100vh;
      overflow: auto;

      h2 {
        margin-top: 100px;
        margin-left: 16px;
      }
      .banner {
        width: 100%;
      }

      .footerImg {
        width: 100%;
      }
      .footer {
        display: flex;
        justify-content: center;
        align-items: center;
      }
    }
  }
</style>
