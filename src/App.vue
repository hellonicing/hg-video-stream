<script setup>
  import NavBar from "./components/NavBar.vue";
  import Tabs from "./components/Tabs.vue";
  import Category from "./components/Category.vue";
  import { dataSource } from "./constants/data.js";
  import { onMounted, ref, watchEffect } from "vue";
  import { debounce } from "lodash";

  const BannerImgSrc = new URL("./assets/banner.png", import.meta.url).href;
  const FoooterImgSrc = new URL("./assets/footer.jpg", import.meta.url).href;

  const contentRef = ref(null);
  const offsetRef = ref(null);

  const oldYRef = ref(0);
  const hiddenSate = ref(false);
  const useHidden = (bool) => {
    return (hiddenSate.value = bool);
  };

  const playingIds = ref([]);
  const isScrolling = ref(false);

  const playAll = (ids) => {
    if (ids.length === 0) return;
    const selector = ids.map((id) => `[data-video-id="${id}"]`).join(",");
    const videoEls = Array.from(document.querySelectorAll(selector));
    videoEls.forEach((el) => {
      el.play();
    });
    playingIds.value = ids;
    // debugger;
  };
  const stopAll = (ids) => {
    if (ids.length === 0) return;
    const selector = ids.map((id) => `[data-video-id="${id}"]`).join(",");
    const videoEls = Array.from(document.querySelectorAll(selector));
    videoEls.forEach((el) => {
      el.pause();
      el.currentTime = 0;
    });
  };
  const pauseAll = (ids) => {
    if (ids.length === 0) return;
    const selector = ids.map((id) => `[data-video-id="${id}"]`).join(",");
    const videoEls = Array.from(document.querySelectorAll(selector));
    videoEls.forEach((el) => {
      el.pause();
    });
  };

  const isInView = (el) => {
    const { top, bottom, left, right } = el.getBoundingClientRect();
    // 水平方向
    const isHorizontalInView = 0 < left && right < window.innerWidth;
    // 垂直方向
    const isVerticalInView =
      top < window.innerHeight / 2 && window.innerHeight / 2 < bottom;
    return isHorizontalInView && isVerticalInView;
  };

  const onScrollEnd = debounce(() => {
    const videoEls = Array.from(document.querySelectorAll("video"));
    // 判断哪些视频在可视范围内
    const inViewVideoEls = videoEls.filter((el) => isInView(el));
    if (inViewVideoEls.length > 0) {
      const ids = inViewVideoEls.map(
        (el) => el.getAttribute("data-video-id") || ""
      );
      // 重置旧视频  不在正在播放视频列表中的就是旧视频
      const stopIds = playingIds.value.filter((id) => !ids.includes(id));
      stopAll(stopIds);
      // 播放新视频
      playAll(ids);
    } else {
      playAll(playingIds.current);
    }
    // console.log("end");
    isScrolling.value = false;
  }, 500);

  const scroll = () => {
    if (!isScrolling.value) {
      pauseAll(playingIds.value);
      isScrolling.value = true;
    }

    if (contentRef.value && offsetRef.value) {
      const { bottom: offsetBottom } = offsetRef.value.getBoundingClientRect();

      // 下滑超过56px 才交互
      if (offsetBottom < 0) {
        const { top: newY } = contentRef.value.getBoundingClientRect();
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
    }
    onScrollEnd();
  };

  onMounted(() => {
    const initVideoIds = dataSource.hot.list.slice(0, 2).map((item) => item.id);
    playAll(initVideoIds);
    // console.log(initVideoIds);
  });
</script>

<template>
  <div class="App">
    <header class="header" :class="{ hidden: hiddenSate }">
      <NavBar title="首页" />
      <Tabs />
    </header>

    <div class="line"></div>

    <div class="scrollView" @scroll="scroll">
      <div :class="{ offset: !hiddenSate }" ref="offsetRef"></div>
      <img class="banner" :src="BannerImgSrc" alt="Banner" />
      <div class="content" ref="contentRef">
        <h2>{{ dataSource.hot.title }}</h2>
        <Category :list="dataSource.hot.list" @scroll="scroll" />
        <h2>{{ dataSource.recommend.title }}</h2>
        <Category :list="dataSource.recommend.list" @scroll="scroll" />
        <h2>{{ dataSource.live.title }}</h2>
        <Category :list="dataSource.live.list" @scroll="scroll" />
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
      .offset {
        height: calc(2 * 56px);
      }

      h2 {
        margin-top: 100px;
      }
      .content {
        padding: 16px;
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
