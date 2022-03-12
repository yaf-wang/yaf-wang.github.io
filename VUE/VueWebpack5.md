

#### Vue2.6+Webpack5

#### 初始化项目

```sh
mkdir webpack-demo
cd webpack-demo
npm init -y
npm install webpack webpack-cli --save-dev
```

#### Css/Sass

##### CSS

参考地址：https://webpack.docschina.org/loaders/css-loader/

首先，你需要先安装 `css-loader`：

```sh
npm install --save-dev css-loader
```

webpack.config.js配置

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/i,
        use: [
          "handlebars-loader", // handlebars loader 需要原始资源字符串
          "extract-loader", // 提取CSS，将CSS 提取为纯粹的 字符串资源
          "css-loader",
        ],
      },
    ],
  },
};

```

##### SASS

文档地址：https://webpack.docschina.org/loaders/sass-loader/

首先，你需要安装 `sass-loader`：

```sh
npm install sass-loader sass --save-dev
```

`sass-loader` 需要预先安装 [Dart Sass](https://github.com/sass/dart-sass) 或 [Node Sass](https://github.com/sass/node-sass)，*推荐使用* [Dart Sass](https://github.com/sass/dart-sass)。

然后，配置webpack.config.js

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.s[ac]ss$/i,
        use: [
          // 将 JS 字符串生成为 style 节点
          'style-loader',
          // 将 CSS 转化成 CommonJS 模块
          'css-loader',
          // 将 Sass 编译成 CSS
          'sass-loader',
        ],
      },
    ],
  },
};
```

#### ESlint







引入方式两种：

- @import '~/src/style/mixin.scss';
- @import './mixin.scss';

**重要的是，只在前面加上 `~`，因为`~/` 将会解析到用户的主目录（home directory）。 因为 CSS 和 Sass 文件没有用于导入相关文件的特殊语法，所以 Webpack 需要区分 `bootstrap` 和 `~bootstrap`。 `@import "style.scss"` 和 `@import "./style.scss";` 两种写法是相同的。**

#### webpack升级

##### webpack.NamedModulesPlugin

> TypeError: webpack.NamedModulesPlugin is not a constructor

**报错原因**

由于webpack版本问题，已经不支持`NamedModulesPlugin`插件，所以该插件配置需要去掉，改为配置`optimization`

**解决办法**

删除`NamedModulesPlugin`插件配置

添加`optimization`配置如下

```
module.exports = {
  optimization: {
    moduleIds: 'named',
  },
}
```









