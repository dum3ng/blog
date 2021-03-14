---
title: "Nuxt_on_ie_not_work"
date: 2020-05-14T12:25:11+08:00
---
# nuxt 在 IE11 上不工作

使用`nuxt.js`编写了一个应用之后，在 IE11 上测试，发现出现错误，于是开始了一段调试之旅。

本来以为可以很快解决，但是最终花了 2 天多的时间，才算基本上解决（仍然有遗留问题）。

## 发现问题

在 IE11 上打开应用后，出现红色的 loading 图标，应用无法正常打开。

初步判断是 IE11 不支持代码中的某些特性。

### 添加 polyfill

在`nuxt.config.js`中添加配置：

```js
module.exports = {
  build: {
    babel: {
      {
          presets({ envName }) {
            const envTargets = {
              client: {
                targets: {
                  ie: 11,
                  browsers: ['>0.25%, last 2 versions, not dead'],
                },
                corejs: {
                  version: 2,
                },
                useBuiltIns: 'usage',
                polyfills: [
                  // defaults: https://github.com/nuxt/nuxt.js/tree/dev/packages/babel-preset-app#options
                  'es6.array.iterator',
                  'es6.promise',
                  'es6.object.assign',
                  'es7.promise.finally',
                  // added
                  'es7.object.values',
                ],
              },
              server: { node: 'current' },
            }

            return [
              [
                '@nuxt/babel-preset-app',
                {
                  ...envTargets[envName],
                },
              ],
              [
                '@vue/babel-preset-jsx',
                {
                  injectH: false,
                },
              ],
            ]
          },
          plugins: [
            '@babel/plugin-syntax-dynamic-import',
            '@babel/plugin-proposal-object-rest-spread',
            '@babel/plugin-transform-arrow-functions',
          ],
          }
    }
  }
}

```

### 代码

到此时， 应用的 loading 图标不再显示红色，但是仍然一直处于 loading 状态，
控制台也没有任何错误信息输出。

尝试将应用部署到测试环境，打开后，控制台显示出了错误信息和错误位置。
错误提示为语法错误。

查看后发现是某个包使用了`es6`的`class`, 但是这个包没有被编译。
由于代码都是 uglify 后的代码，如何发现这段代码属于哪个包呢？
考虑后， 尝试使用 `silversearcher` 去`node_modules` 文件夹里
搜索这个包里的字符串常量。
果然查找到了包名`quick-lru`, 在 github 上查看其源代码后，发现其 publish 没有
compile，导致最终的包里仍然包含`class`关键字。

将这个包也添加到 nuxt.config.js 的 `transpile` 中：

```js
module.exports = {
  build: {
    transpile: [..., 'quick-lru']
  }
}
```

这也反映了 node 生态的一个问题, 很多包并没有很规范的发布流程, 导致一些兼容性问题以及其他问题.
实际上在应用中引入的是`camelcase-keys`, 这个包又引入了`quick-lru`.

