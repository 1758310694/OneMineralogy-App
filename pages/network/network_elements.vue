<template>
  <view class="container">
    <web-view :src="htmlUrl"></web-view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      htmlUrl: ''
    };
  },
  onLoad() {
    // 调用后端接口获取 HTML 文件的 URL
    this.htmlUrl = 'https://jewelry-change-establish-tabs.trycloudflare.com/minerals_elements_network.html';
  }
};
</script>

<style>
.container {
  width: 100%;
  height: 100%;
}
</style>
