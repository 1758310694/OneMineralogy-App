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
    this.htmlUrl = 'http://172.31.152.31:80//minerals_network.html';
  }
};
</script>

<style>
.container {
  width: 100%;
  height: 100%;
}
</style>
