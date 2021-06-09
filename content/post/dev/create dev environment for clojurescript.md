---
title: '搭建 Clojurescript 开发环境'
date: 2021-06-06T13:06:57+08:00
tags: [clojurescript]
categories: [dev]
---

以`re-frame`的 template 为例，搭建 clojurescript 开发环境。

基础环境: Ubuntu 20.04 + jdk 11

### 安装`clj`命令行工具

按照[clojure](https://clojure.org/guides/getting_started)官网的方法安装 clj.

{{% tip %}}

不要使用 ubuntu 系统自带的`clojure`包，其不支持某些特性，无法在接下来的项目中配合`shadow-cljs`使用。

{{% /tip %}}

### 安装 Idea Intellij + Cursive

可以使用 Idea Intellij 的 community 版本。
安装完成后，安装 cursive 插件，其 license 可以去[官网](https://cursive-ide.com/)获取免费的个人版。

### 创建项目

以`reframe`的模板项目为基础创建一个新的 cljs 项目:

```bash
lein new re-frame my-app
```

创建完成后，打开项目目录，会发现默认使用 shadow 来管理依赖，配置记录在`shadow-cljs.edn`中。
现在我们改为使用 deps 来管理。
创建`deps.edn`文件，内容为：

```clojure
{
 :paths   ["dev"]
 :deps    {thheller/shadow-cljs {:mvn/version "2.12.5"}}

 :aliases {:cljs
           {:extra-deps  {reagent            {:mvn/version "1.0.0"}
                          re-frame           {:mvn/version "1.2.0"}
                          binaryage/devtools {:mvn/version "1.0.3"}
                          }
            :extra-paths ["src/main" "src/test"]
            }
           }
 }

```

而`shadow-cljs.edn` 中，添加：

```clojure
 :deps  {:aliases [:cljs]}
```

shadow-cljs 将使用 deps 来管理依赖。

在 intellij 中打开项目时，选择 打开-> 从已有资源打开，然后选择`deps.edn`文件，以项目形式打开。

在`clojure deps`面板中，选中我们设定的别名配置`cljs`:

![clojure deps](../assets/clojure_deps.png)

然后在外部库上右键，选择**Refresh Clojure Deps Project**，即可添加外部库。

![refresh deps](../assets/refresh_deps.png)

### 启动项目，添加 repl

在 intellij 中添加一个运行配置，选择本地 clojure repl：

![clojure repl](../assets/clojure_repl.png)

启动项目：

```bash
yarn && yarn watch
```

之后，运行`repl`，在其中即可运行命令。
默认连接的是 clojure 的 repl，可以运行 `(shadow/node-repl)`来切换到 node 的 repl。

![node repl](../assets/node_repl.png)
