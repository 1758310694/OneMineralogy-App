# OneMineralogy
用于从OpenMindat数据接口(API)访问和分析Mindat开放数据的应用程序(APP)

### 概述
`OneMineralogy`是一个基于Vue和Uni-app框架开发的JavaScript的移动端应用程序，实现多种检索功能获取不同需求的矿物数据，并初步支持交互式查询、探索性分析及推理等任务

### 开始

**编译软件下载**

HBuilderX是通用的前端开发工具，可视化方式简单。HBuilderX内置相关环境，开箱即用，无需配置nodejs。工具获取地址：https://www.dcloud.io/hbuilderx.html

**代码获取**

代码获取有以下两种方式：

1.直接下载源码ZIP文件

2.使用Git克隆仓库

```coffee
git clone https://github.com/1758310694/OneMineralogy-App.git
```

**运行**

1、将源码加载至HBuilderX工具中(可直接将文件拉入)

2、在HBuilderX选择`运行`-->`运行到浏览器`即可在浏览器使用

3、选择`发行`-->`云打包`即可打包为安装包，在移动端使用


**操作使用说明**

<img src="/static/image2/search.png?raw=true" width="20%">

1、①处输入API Token

Token获取：https://www.mindat.org/a/how_to_get_my_mindat_api_key

2、②处选择检索功能，具体说明可见:[检索功能说明](/static/矿物检索功能说明.csv)

3、③处输入检索参数值,详情可见API文档:https://api.mindat.org/schema/redoc/

4、④处点击后进行查询

5、⑤处点击后下载查询文件，支持CSV、TXT、JSON-LD、TTL、RDF格式

**交互式查询、探索性分析、推理等用例分析**

<img src="/static/image2/use case.png?raw=true" width="20%">

上图界面中`查询`对应可支持属性组合过滤出矿物品种，以及使用Mindat ID查询矿物品种具体属性

`探索性分析`支持矿物共生网络分析、矿物-元素共生网络分析及地点密度热图

`推理`支持预测目标地点出现新矿物，及其目标矿物的共生组合

上述用例分析功能初步实现某些具体条件下的分析，后续计划仍需完善通用分析功能


