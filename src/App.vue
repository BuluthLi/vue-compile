// 以下针对本地运行，线上运行，注意build之后线上运行需要调整的地方，主要是各种路径
// 一般图片放在assets里面，路径会被webpack解析，而static的路径不会被解析，是固定的,(即表示assets被webpack当做模块解析，而static是看成静态资源，里面的资源一旦被引用，是不会在解析的)
// html的相对路径都会解析，用static的图片该路径不会解析；js的路径不会解析，需要先import图片资源；css的路径会解析出错，一般在build/util.js里面的ExtractTextPlugin.extract进行配置相对路径
// 注意本地代理的使用方式
<template>
  <div id="app">
    <Main/>
  </div>
</template>

<script>
import Main from "./components/main/Main";

export default {
  name: "App",
  components: {
    Main
  }
};
</script>

<style>
</style>
