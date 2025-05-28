<template>
  <view class="container">
	<text>查询</text>
	<button @click="runCode('3.3.1.py')">矿物属性组合过滤</button>
	<button @click="runCode('3.3.2.py')">矿物品种信息查询</button>
	<text>探索性分析</text>
	<button @click="runCode('3.1.1.py')">矿物共生与网络分析</button>
	<button @click="runCode('3.1.2.py')">矿物-元素共生网络分析</button>
    <button @click="runCode('3.1.3.py')">地点热力密度图</button>
	<text>推理</text>
   <button @click="runCode('3.2.1.py')">预测目标地点出现新矿物</button>
   <button @click="runCode('3.2.2.py')">矿物共生关系</button>

    <!-- 筛选表单 -->
    <view v-if="showFilterForm">
      <label>元素组成:</label>
      <input v-model="elements" placeholder="输入元素组成" />
      <label>硬度值:</label>
      <input v-model="hardness" type="number" placeholder="输入硬度值" />
      <label>密度值:</label>
      <input v-model="density" type="number" placeholder="输入密度值" />
      <label>颜色:</label>
      <input v-model="colour" placeholder="输入颜色" />
      <label>晶体结构:</label>
      <input v-model="csystem" placeholder="输入晶体结构" />
      <label>发现年份:</label>
      <input v-model="discoveryYear" type="number" placeholder="输入发现年份" />
      <button @click="applyFilters">查询</button>
    </view>

    <!-- 搜索表单 -->
    <view v-if="showSearchForm">
      <label>输入矿物 ID 或 名称:</label>
      <input v-model="searchValue" placeholder="输入ID或名称" />
      <button @click="applySearch">查询</button>
    </view>

    <!-- 筛选结果 -->
    <view v-if="filteredData.length">
      <h3>匹配的矿物:</h3>
      <ul>
        <li v-for="(item, index) in filteredData" :key="index">
          ID: {{ item.id }}, 矿物名称: {{ item.name }}
        </li>
      </ul>
    </view>

    <!-- 搜索结果 -->
    <view v-if="searchResult">
      <h3>查询到的矿物:</h3>
      <p>ID: {{ searchResult.id }}</p>
      <p>名称: {{ searchResult.name }}</p>
      <p>元素: {{ searchResult.elements }}</p>
      <p>硬度: {{ searchResult.hmin }} - {{ searchResult.hmax }}</p>
      <p>密度: {{ searchResult.dmeas }} - {{ searchResult.dmeas2 }}</p>
      <p>颜色: {{ searchResult.colour }}</p>
      <p>晶体结构: {{ searchResult.csystem }}</p>
      <p>发现年份: {{ searchResult.discovery_year }}</p>
    </view>

    <!-- 输出信息 -->
    <text class="output">{{ output }}</text>
  </view>
</template>

<script>
export default {
  data() {
    return {
      output: "",
      showFilterForm: false,
      showSearchForm: false,
      elements: "",
      hardness: null,
      density: null,
      colour: "",
      csystem: "",
      discoveryYear: null,
      searchValue: "",
      filteredData: [],
      searchResult: null,
      jsonData: []
    };
  },
  methods: {
    // 运行脚本
    async runCode(scriptName) {
      this.elements = "";
      this.hardness = null;
      this.density = null;
      this.colour = "";
      this.csystem = "";
      this.discoveryYear = null;
      this.searchValue = "";
      this.filteredData = [];
      this.searchResult = null;
      this.showFilterForm = false;
      this.showSearchForm = false;

      // 根据脚本名称处理
      if (scriptName === '3.1.1.py') {
        this.output = "";
        try {
          const res = await uni.request({
            url: `http://172.31.152.31:80/run?script=${scriptName}`,
            method: 'GET',
            header: { 'Content-Type': 'application/json' }
          });
          this.output = res.data.output;

          // 跳转到 network 页面
          uni.navigateTo({
            url: '/pages/network/network'
          });
        } catch (err) {
          this.output = '请求失败：' + err.errMsg;
        }
      } else if (scriptName === '3.1.2.py') {
        this.output = "";
        try {
          const res = await uni.request({
            url: `http://172.31.152.31:80/run?script=${scriptName}`,
            method: 'GET',
            header: { 'Content-Type': 'application/json' }
          });
          this.output = res.data.output;

          // 跳转到 network 页面
          uni.navigateTo({
            url: '/pages/network/network_elements'
          });
        } catch (err) {
          this.output = '请求失败：' + err.errMsg;
        }
      } else if (scriptName === '3.1.3.py') {
        this.output = "";
        try {
          const res = await uni.request({
            url: `http://172.31.152.31:80/run?script=${scriptName}`,
            method: 'GET',
            header: { 'Content-Type': 'application/json' }
          });
          this.output = res.data.output;

          // 跳转到 network 页面
          uni.navigateTo({
            url: '/pages/network/map'
          });
        } catch (err) {
          this.output = '请求失败：' + err.errMsg;
        }
      } else if (scriptName === '3.3.1.py') {
        this.output = "";
        this.showFilterForm = true;
        this.loadLocalJson();
      } else if (scriptName === '3.3.2.py') {
        this.output = "";
        this.showSearchForm = true;
        this.loadLocalJson();
      } else {
        try {
          const res = await uni.request({
            url: `http://172.31.152.31:80/run?script=${scriptName}`,
            method: 'GET',
            header: { 'Content-Type': 'application/json' }
          });
          this.output = res.data.output;
        } catch (err) {
          this.output = '请求失败：' + err.errMsg;
        }
      }
    },

    // 从服务器获取 geomaterials.json
    loadLocalJson() {
      const filePath = 'http://172.31.152.31:80/geomaterials';  // 后端服务提供的 API 路径

      uni.request({
        url: filePath,
        method: 'GET',
        success: (res) => {
          if (res.statusCode === 200) {
            this.jsonData = res.data.results || res.data;
          } else {
            this.output = '加载 JSON 文件失败: ' + res.statusCode;
          }
        },
        fail: (err) => {
          this.output = '读取文件失败：' + err.errMsg;
        }
      });
    },

    // 应用筛选器
    applyFilters() {
      const inputElements = this.elements
        ? this.elements.split(/[,，\s]+/).map(e => e.trim()).filter(e => e)
        : [];
    
      this.filteredData = this.jsonData.filter(mineral => {
        const hasAllElements = inputElements.length
          ? inputElements.every(el => mineral.elements.includes(el))
          : true;
    
        return (
          hasAllElements &&
          (this.hardness ? mineral.hmin <= this.hardness && mineral.hmax >= this.hardness : true) &&
          (this.density ? mineral.dmeas <= this.density && mineral.dmeas2 >= this.density : true) &&
          (this.colour ? mineral.colour.includes(this.colour) : true) &&
          (this.csystem ? mineral.csystem.includes(this.csystem) : true) &&
          (this.discoveryYear ? mineral.discovery_year === this.discoveryYear : true)
        );
      });
    },


    // 应用搜索
    applySearch() {
      const value = this.searchValue.trim();
      const isNumeric = /^\d+$/.test(value);
      const result = this.jsonData.find(mineral => {
        return isNumeric
          ? mineral.id === parseInt(value)
          : mineral.name.toLowerCase().includes(value.toLowerCase());
      });
      this.searchResult = result || null;
    }
  }
};
</script>

<style>
.container {
  padding: 20px;
}
button {
  margin: 5px 0;
}
.output {
  margin-top: 10px;
  font-family: monospace;
  white-space: pre-wrap;
}
* {
  user-select: text;
}
</style>
