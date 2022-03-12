微服务架构中的BFF到底是啥？

#### VUE起步安装

##### Vue-cli

##### webpack模版

#### VUE中常被忽略的知识

1、自动过滤收尾的空白，可以用<input v-model.trim="msg">

2、自定义事件，只触发一次：this.$once('xx')

3、异步使用组件，只有在需要时才去从服务器中拿

```javascript
'my-component': () => import('./my-async-component')
```

4、边界处理

```javascript
this.$root
this.$parent
this.$refs
```

5、依赖注入：父亲provide方法，子组件进行inject，适合于很多子组件都需要用到父组件的某个方法

```javascript
// 父组件
provide: function () {
  return {
    getMap: this.getMap
  }
}

// 子组件
inject: ['getMap']
```

6、使用程序化进行清理第三方的插件

```javascript
// 建议
mounted(){
    var picker = new Pikaday({
        field: this.$refs.input,
        format: 'YYYY-MM-DD'
    })
    this.$once('hook:beforeDestroy', () => {
        picker.destroy();
    })
}

// 不建议
mounted(){
    var picker = new Pikaday({
   field: this.$refs.input,
   format: 'YYYY-MM-DD'
 })
},
beforeDestroy(){
    this.picker.destroy();
}
```



7、混入mixin，是组件数据和方法优先（vue3中使用）

8、自定义事件async，可用于子组件快速改变父组件的值

```javascript
// 父
<child v-on:isShow.async="isShow" v-show="isShow"/>
// 本质上是 v-on:update:isShow="v => isShow = v" 的语法糖

// 子组件
this.$emit('update:isShow', true);
  
```

9、在输入input的时候，可以用<input v-model.lazy="msg">，只有在change的时候才去更新值

10、页导入多个组件的写法

```javascript
// 原始写法
import titleCom from '@/components/home/titleCom';
import bannerCom from '@/components/home/bannerCom';
import cellCom from '@/components/home/cellCom';


// 利用require.context()的写法
const path = require('path');
const files = require.context('@/components/home',false, /\.vue$/);
const modules = {}
files.keys().forEach(key =>{
   const name = path.basename(key,'.vue')
   modules[name] = files(key).default || files(key)
})
....
//组件内的这样写
export default{
  components: modules
}
```

##### 11、watch侦听属性

- 立即执行 选项handle,immediate

- 深度侦听 选项handle, deep

  受JavaScript的限制，Vue不能检测对象属性的添加和删除，因为Vue在初始化实例时执行getter/setter执行转化，所以属性必须在data对象上才能让Vue执行转换。这样他才是响应的。可以这样改写

  ```javascript
  watch:{
     obj:{
        handle(newValue, oldValue){
           console.log('obj.a is changed')
        },
        immediate: true,
        deep: true  // 加上deep:true后，就可以监听obj属性的变化了
     }
  }
  // 缺点:开销比较大，因为会向下一层一层进行遍历，给obj所有的属性都加上了监听器。
  // 直接监听对象的属性
  watch:{
     obj.a:{
        handle(newValue, oldValue){
           console.log('obj.a is changed')
        },
        immediate: true,
        // deep: true  
     }
  }
  this.$set(obj, key, vaule)
  ```

- attrs和listeners
  - attrs获取父传子组件中，未在props定义的属性
  - listeners: 可以通过v-on="$listeners" 将父组件的事件传入内部子组件，实现多层级通信

##### 12、数组对象更新视图渲染问题

​	同布局数据

```javascript
// 方式1
this.items.splice(feedDataIndex, 1, res.data)
// 方式2
this.$set(this.items, feedDataIndex, res.data)
// 方式3
this.items = this.items.map(item => {
	if (item.id === feedDataId) {
		return res.data
	}
	return item
})
```

##### 13、**Vue.js 使用v-cloak后仍显示变量的解决方法**

v-cloak  这个指令是防止页面加载时出现 vuejs 的变量名而设计的，但有时候添加了这个指令仍会显示变量，这是怎么回事呢？。

**v-cloak 用法：**

HTML代码：

```vue
<div v-cloak> {{ message }}</div>
```

CSS代码：

```css
[v-cloak] { display: none;}
```

这样直至div内变量编译完毕后才会显示。

但有时添加完毕后仍有部分变量会显示，这是怎么回事呢？通过控制台查看，原来是 v-cloak 的display属性被优先级别高的样式覆盖所导致，我的处理方案是添加 !important ，简单粗暴。新css样式如下：

```css
[v-cloak] { display:none !important;}
```

##### 14、VUE同一个组件被一个页面调用两次，数据冲突

```vue
// 复现前代码，其中filelist有冲突
<UploadImage v-if="showIndex === 1" key="1" v-model="form.gccczhk" />
<UploadImage v-if="showIndex === 2"  key="2" v-model="form.rclcyz" />

//原因：VUE识别的同一组件，不会created()方法只执行一次

//方法1: 对每个组件绑定Key
```

##### 15、自定义组件传参问题

```vue
// 父子组件传参到方法中
<my-component  @change="handleChange($event, params" />
```

##### 16、自定义组件插槽问题

目的：当插槽没有使用禁止渲染

```vue
<div v-if="$slots.footer">
  <slot name="footer" />
</div>
```



##### 17、数组函数

###### 会更新视图渲染

- push：最后位置添加
- pop：最后位置删除
- shift：第一个位置删除
- unshift：第一个位置添加
- splice：切片
- sort：排序
- reverse：反转

###### 检测不到变动的数组操作

- filter()：过滤
- concat()：追加另一个数组
- slice()：
- map()：

---------------------------------------------------------------无与伦比的分割线-----------------------------------------------------------------------

#### 附录：

##### Vue.config.js完整配置

```json

const path = require("path");
const resolve = dir => path.join(__dirname, dir);
//用于生产环境去除多余的css
const PurgecssPlugin = require("purgecss-webpack-plugin");
//全局文件路径
const glob = require("glob-all");
//压缩代码并去掉console
const UglifyJsPlugin = require("uglifyjs-webpack-plugin");
//代码打包zip
const CompressionWebpackPlugin = require("compression-webpack-plugin");
const productionGzipExtensions = /\.(js|css|json|txt|html|ico|svg)(\?.*)?$/i;
module.exports = {
  // 废弃baseUrl  一般运维会配置好的
  publicPath: process.env.NODE_ENV === "production" ? "/configtest/" : "/",
  //打包的输出目录
  outputDir: "dist/configtest",
  //保存时是否校验
  lintOnSave: true,
  //如果文件等设置
  pages: {
    index: {
      entry: "src/main.js",
      template: "public/index.html",
      filename: "index.html"
    }
  },
  //静态资源打包路径
  assetsDir: "static",
  //默认false 可以加快打包
  productionSourceMap: false,
  //打包后的启动文件
  indexPath: "congfigtest.html",
  //打包文件是否使用hash
  filenameHashing: true,
  runtimeCompiler: false,
  transpileDependencies: [],
  //打包的css路径及命名
  css: {
    modules: false,
    //vue 文件中修改css 不生效 注释掉  extract:true
    extract: {
      filename: "style/[name].[hash:8].css",
      chunkFilename: "style/[name].[hash:8].css"
    },
    sourceMap: false,
    loaderOptions: {
      css: {},
      less: {
        // 向全局less样式传入共享的全局变量
        // data: `@import "~assets/less/variables.less";$src: "${process.env.VUE_APP_SRC}";`
      },
      // postcss 设置
      postcss: {
        plugins: [
          require("postcss-px-to-viewport")({
            viewportWidth: 750, // 视窗的宽度，对应的是我们设计稿的宽度，一般是750
            viewportHeight: 1334, // 视窗的高度，根据750设备的宽度来指定，一般指定1334，也可以不配置
            unitPrecision: 3, // 指定`px`转换为视窗单位值的小数位数（很多时候无法整除）
            viewportUnit: "vw", // 指定需要转换成的视窗单位，建议使用vw
            selectorBlackList: [".ignore", ".hairlines"], // 指定不转换为视窗单位的类，可以自定义，可以无限添加,建议定义一至两个通用的类名
            minPixelValue: 1, // 小于或等于`1px`不转换为视窗单位，你也可以设置为你想要的值
            mediaQuery: false // 允许在媒体查询中转换`px`
          })
        ]
      }
    }
  },
  //webpack 链式配置   默认已经配置好了  node_moudles/@vue
  chainWebpack: config => {
    // 修复HMR
    config.resolve.symlinks(true);
    // 修复Lazy loading routes  按需加载的问题，如果没有配置按需加载不需要写，会报错
    // config.plugin("html").tap(args => {
    //   args[0].chunksSortMode = "none";
    //   return args;
    // });
    //添加别名
    config.resolve.alias
      .set("@", resolve("src"))
      .set("assets", resolve("src/assets"))
      .set("components", resolve("src/components"))
      .set("layout", resolve("src/layout"))
      .set("base", resolve("src/base"))
      .set("static", resolve("src/static"));
    // 压缩图片
    config.module
      .rule("images")
      .use("image-webpack-loader")
      .loader("image-webpack-loader")
      .options({
        mozjpeg: { progressive: true, quality: 65 },
        optipng: { enabled: false },
        pngquant: { quality: "65-90", speed: 4 },
        gifsicle: { interlaced: false },
        webp: { quality: 75 }
      });
  },
  //webpack 配置
  configureWebpack: config => {
    const plugins = [];
    //去掉不用的css 多余的css
    plugins.push(
      new PurgecssPlugin({
        paths: glob.sync([path.join(__dirname, "./**/*.vue")]),
        extractors: [
          {
            extractor: class Extractor {
              static extract(content) {
                const validSection = content.replace(
                  /<style([\s\S]*?)<\/style>+/gim,
                  ""
                );
                return validSection.match(/[A-Za-z0-9-_:/]+/g) || [];
              }
            },
            extensions: ["html", "vue"]
          }
        ],
        whitelist: ["html", "body"],
        whitelistPatterns: [/el-.*/],
        whitelistPatternsChildren: [/^token/, /^pre/, /^code/]
      })
    );
    //启用代码压缩
    plugins.push(
      new UglifyJsPlugin({
        uglifyOptions: {
          compress: {
            warnings: false,
            drop_console: true,
            drop_debugger: false,
            pure_funcs: ["console.log"] //移除console
          }
        },
        sourceMap: false,
        parallel: true
      })
    ),
      //代码压缩打包
      plugins.push(
        new CompressionWebpackPlugin({
          filename: "[path].gz[query]",
          algorithm: "gzip",
          test: productionGzipExtensions,
          threshold: 10240,
          minRatio: 0.8
        })
      );
    config.plugins = [...config.plugins, ...plugins];
  },
  parallel: require("os").cpus().length > 1,
  pluginOptions: {},
  pwa: {},
  //设置代理
  devServer: {
    port: 8080,
    host: "0.0.0.0",
    https: false,
    open: true,
    openPage: "about",
    hot: true,
    disableHostCheck: true,
    proxy: {
      "/api": {
        target: "https://cdn.awenliang.cn",
        ws: true,
        changeOrigin: true
      },
      "/foo": {
        target: "https://cdn.awenliang.cn",
        ws: true,
        changeOrigin: true
      }
    }
  }
};
```

