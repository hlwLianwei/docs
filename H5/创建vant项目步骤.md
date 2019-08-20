## 1.创建引用 vant 项目步骤

### 1.1 创建项目

```
vue init webpack charger_h5_new
```

### 1.2.填写项目名称

```
Project name: charger_h5_new
Project description: ttkc
Author: ttkc
Vue build: (这个直接敲回车键选默认)
Install vue-router: y
Use ESLint to lint your code: n (这个是各种代码风格，各个种样式的检查, 代码多一个空格少一个空格都会报错，最好选no)
Set up unit tests: n
setup e2e tests with Nightwatch: n
Should we run 'npm install' for you after the project has been created: (这个直接敲回车键选默认)
```

### 1.3.创建好项目后，切换到项目文件夹，运行项目看是否成功

```
cd charger_h5_new
npm run dev
```

### 1.4.修改配置使用使用 ip 及 localhost 同时能访问

在项目根目录的 config 文件下的 index.js 文件，找到

```
module.exports = {
  dev: {
    // ......
    // Various Dev Server settings
    host: 'localhost', // can be overwritten by process.env.HOST
    port: 8080, // can be overwritten by process.env.PORT, if port is in use, a free one will be determined
    // ......
  },
}
```

将 host: 'localhost'修改成 host: '0.0.0.0', 然后重新启动项目会出现：

```
  Your application is running here: http://0.0.0.0:8080
```

### 1.5.安装 vw 布局插件

```
npm i postcss-aspect-ratio-mini postcss-px-to-viewport-opt postcss-write-svg postcss-cssnext postcss-viewport-units cssnano --save
```

安装成功之后，在项目根目录下的 package.json 文件中，可以看到新安装的依赖包  
接下来在项目根目录的 .postcssrc.js 文件对新安装的 PostCSS 插件进行配置：

```
module.exports = {
  "plugins": {
    "postcss-import": {},
    "postcss-url": {},
    "postcss-aspect-ratio-mini": {},
    "postcss-write-svg": {
      utf8: false
    },
    "postcss-cssnext": {},
    "postcss-px-to-viewport-opt": {
      viewportWidth: 750,   // 视窗的宽度，对应的是我们设计稿的宽度，一般是750
      viewportHeight: 1334, // 视窗的高度，根据750设备的宽度来指定，一般指定1334，也可以不配置
      unitPrecision: 3,     // 指定`px`转换为视窗单位值的小数位数
      viewportUnit: "vw",   //指定需要转换成的视窗单位，建议使用vw
      selectorBlackList: ['.ignore', '.hairlines'],// 指定不转换为视窗单位的类，可以自定义，可以无限添加,建议定义一至两个通用的类名
      minPixelValue: 1,     // 小于或等于`1px`不转换为视窗单位，你也可以设置为你想要的值
      mediaQuery: false,     // 允许在媒体查询中转换`px`
      exclude: /(\/|\\)(node_modules)(\/|\\)/
    },
    "postcss-viewport-units": {},
    "cssnano": {
      preset: "advanced",
      autoprefixer: false,
      "postcss-zindex": false // 只要启用了这个插件，z-index的值就会重置为1。这是一个天坑，千万记得将postcss-zindex设置为false。
    }
  }
}
```

简单介绍上面几个插件的作用：

postcss-cssnext：其实就是 cssnext。该插件可以让我们使用 CSS 未来的特性，其会对这些特性做相关的兼容性处理。

cssnano：主要用来压缩和清理 CSS 代码。在 Webpack 中，cssnano 和 css-loader 捆绑在一起，所以不需要自己加载它。在 cssnano 的配置中，使用了 preset: "advanced"，所以我们需要另外安装：

```
npm i cssnano-preset-advanced --save-dev
```

postcss-px-to-viewport-opt：要用来把 px 单位转换为 vw、vh、vmin 或者 vmax 这样的视窗单位，也是 vw 适配方案的核心插件之一。是 postcss-px-to-viewport 的改良版本，主要是处理与第三方组件::before, ::after 等冲突。
postcss-aspect-ratio-mini：主要用来处理元素容器宽高比。
postcss-write-svg：插件主要用来处理移动端 1px 的解决方案。该插件主要使用的是 border-image 和 background 来做 1px 的相关处理。

### 1.6.安装 vant

代码在这里: [https://github.com/youzan/vant](https://github.com/youzan/vant)

```
npm i vant -S
```

### 1.7.安装 babel-plugin-import 插件, 即按需引入 vant 组件

babel-plugin-import 是一款 babel 插件，它会在编译过程中将 import 的写法自动转换为按需引入的方式。

```
npm i babel-plugin-import -D
```

在项目根目录下的 .babelrc 文件中配置 plugins（插件）：

```
"plugins": [
    "transform-vue-jsx",
    "transform-runtime",
    ["import", [{ "libraryName": "vant", "style": true }]]
  ],
```

按需要引入 vant 组件，比如我们要在 HelloWord.vue 组件中使用 Loading 组件，我们可以先 在组件中引入:

```
<script>
import { Loading } from 'vant'
export default {
  components: {
    [Loading.name]: Loading
  }
}
</script>
```

然后在页面中加入组件代码:

```
<template>
   <div>
      <van-loading color="black" />
      <van-loading color="white" />
      <van-loading type="spinner" color="black" />
      <van-loading type="spinner" color="white" />
  </div>
</template>
```

这样就可以看到效果了.
完整的 HelloWord.vue 代码如下:

```
<template>
  <div>
    <van-loading color="black" />
    <van-loading color="white" />
    <van-loading type="spinner" color="black" />
    <van-loading type="spinner" color="white" />
  </div>
</template>

<script>
import { Loading } from "vant";
export default {
  components: {
    [Loading.name]: Loading
  },
  name: "HelloWorld",
  data() {
    return {
      msg: "Welcome to Your Vue.js App"
    };
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
```

## 2.以下是额外的代码

### 2.1.创建组件

在项目根目录的 src\components 目录创建 NavigationBar.vue 文件, 内容如下:

```
<template>
  <div class="cp-navbar">
    <div class="cp-title-text">{{data}}</div>
    <img class="cp-title-back-image" src="/static/icon/navbar_back.png" />
  </div>
</template>
<script>
export default {
  name: "NavigationBar",
  data: function() {
    return {
      data: "这个是标题栏"
    };
  }
};
</script>

<style scoped>
/*样式代码*/
.cp-navbar {
  width: 100%;
  height: 100px;
  display: flex;
  flex-direction: row;
  align-items: center;
  background-color: #009bf8;
  position: fixed;
  top: 0;
  left: 0;
  z-index: 999999;
}

.cp-title-back-image {
  width: 22px;
  height: 38px;
  position: fixed;
  left: 30px;
  top: 31px;
}

.cp-title-text {
  height: 100px;
  line-height: 100px;
  color: #fff;
  font-size: 38px;
  text-align: center;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  margin-left: 30px;
  margin-right: 30px;
  flex-grow: 1;
  flex-shrink: 1;
}
</style>

```

引用的图片放在项目根目录 static\icon 目录下。
放在 static 目录下，在引用的时候可以写绝对路径：

```
<img class="cp-title-back-image" src="/static/icon/navbar_back.png" />
```

### 2.2.创建页面

在项目根目录的 src 目录下 pages 目录，关在 pages 目录创建 Login.vue 文件, 内容如下:

```
<template>
  <div class="ui-page">
    <navigationBar />
    <div class="ui-divider" />
    <div class="ui-fragment">
      <div>我的中国</div>
    </div>
  </div>
</template>

<script>
// 引入loading组件,在数据加载时显示,他的显示隐藏由 showLoading的值决定
import navigationBar from "../components/NavigationBar.vue";
export default {
  name: "home",
  components: {
    navigationBar
  },
  data: function() {
    return {
      msg: "Welcome to one demo",
      showLoading: true,
      homeDesc: {}
    };
  },
  created: function() {
    this.getDatas();
  },
  methods: {
    getDatas: function() {}
  }
};
</script>

<style>
/*样式代码*/
.ui-page {
  margin: 0px;
}

.ui-divider {
  width: 100%;
  height: 1px;
  background: #ff0000;
  position: fixed;
  z-index: 999;
}

.ui-fragment {
  margin-top: 100px;
  margin-bottom: 0px;
  background: #00ff00;
  flex-grow: 1;
  flex-shrink: 1;
}
</style>

```

修改项目根目录 src\router 目录下的 index.js 文件, 去掉 HelloWord 组件，添加我们自己的 Login 页面:

```
import Vue from 'vue'
import Router from 'vue-router'
// import HelloWorld from '@/components/HelloWorld'
import Login from '@/pages/Login'

var routes = [
  {
    path: '/',
    component: Login
  }
]

Vue.use(Router)

export default new Router({
  routes
})

```

### 2.3.修改路由

打开项目根目录 src 目录下的 App.vue 文件，把 vue 的 logo 去掉:

```
<img src="./assets/logo.png">
```

去掉 logo 后变成这样子:

```
<template>
  <div id="app">
    <router-view/>
  </div>
</template>
```

### 2.4.修改页面边距

新建的 vue 程序页面默认左、右、下等有一定的边距。要消除这些边距，可以在项目根目录 src 目录下的 App.vue 文件里添加样式:

```
body {
  margin: 0;
  padding: 0;
  background-color: #fff;
}
```

### 2.5.设置全局样式

部分样式在多个页面同时引用时，可以设置成全局样式。
在项目根目录 src\assets 目录下创建样式文件 css, 如 ResetUi.css。把需要定义成全局样式的代码写在该文件中。
然后在项目根目录 src 目录的 main.js 文件中引入该 css 文件:

```
import '../src/assets/ResetUi.css'
```
