---
title: "创建你的第一个应用"
---

# 创建你的第一个应用

在这个章节中, 你将会利用 "Hello World" 这个应用来学习 Angular.dart 应用的基本结构, 如何绑定 (bind) model 到 view, 以及如何使用 Angular 内置的一些指令 (directive).

### 运行示例应用

本章节的代码可以在示例代码的 *[Chapter_01](https://github.com/angular/angular.dart.tutorial/tree/master/Chapter_01)* 目录下找到. 在 Dart Editor 里通过 **File >> Open Existing Folder...** 打开 **Chapter_01** 来查看它.

现在开始运行这个应用. 在 Dart Editor 的 Files 面板内, 选中 **Chapter_01/web/index.html** 文件, 右键, 然后点击 **Run in Dartium**.

> **提示:** 尽管 Angular.dart 的应用可以在任何现代浏览器内运行, 但是现在, 我们推荐你暂时使用 Dartium. [稍后](./09-ch07-deploying-your-app.html), 你将会学习到如何去生成现代浏览器可运行的 JavaScript 文件.

Dartium 启动后, 应用成功的运行了. 当你在输入框打字的时候, 输入的所有字符都会显示在标题上.

![ch01-1.png](./img/ch01-1.png) ![ch01-2.png](./img/ch01-2.png)

> **提示:** 你可能注意到了, 在应用第一次启动的时候, *{% raw %}{{name}}{% endraw %}* 显示了出来然后又消失了. 别担心, [稍后](./07-ch05-filter-service.html)你将会学习到如何使用 `ng-cloak` 去隐藏未编译过的 DOM 中的值.

### 让 Angular 的库在应用中可用

Dart 通过 **pubspec.yaml** 文件来定义应用的依赖包. 这里是当前示例中最简单的 pubspec.yaml 文件定义:

``` yaml
name: tutorial
version: 1.0.0
dependencies:
  angular: "1.0.0"
  web_components: ">=0.8.0 <0.9.0"
  browser: ">=0.10.0+2 <0.11.0"
transformers:
- angular
```

要在应用里使用 Angular.dart, 你需要将 `angular` 加入进依赖列表. 同时, 你也需要添加 `angular` 变换器 (transformer), 在之后将你的应用转换成 JavaScript 时候 (如 [部署你的应用](./09-ch07-deploying-your-app.html) 中所描述的), 这是非常必要的. `web_components` 这个包为不支持 [Shadow DOM](http://www.w3.org/TR/shadow-dom/) 的旧版浏览器提供了 polyfills. `browser` 这个包帮助那些不支持 Dart 的浏览器切换到应用编译后的 JavaSript 版本.

Dart Editor 会自动下载你的应用依赖的所有包. 如果下载失败了, 或者你更喜欢使用命令行, 你可以手动去安装那些包. 通过 Dart Editor, 你可以使用 **Tools >> Pub Get** 下载. 通过命令行 (在你的应用的根目录下, 并且确保 Dart SDK 在你的 PATH 中), 你可以直接运行 `pub get`.

### 应用的代码

应用的代码被包含在两个文件中:

[index.html](https://github.com/angular/angular.dart.tutorial/blob/master/Chapter_01/web/index.html)
: 定义了应用的 UI, 并且包含了应用的脚本.

[main.dart](https://github.com/angular/angular.dart.tutorial/blob/master/Chapter_01/web/main.dart)
: 定义了一个 main() 函数用于初始化和启动应用.

两个文件都在名为 **web** 的子目录下, 与 [pub 包布局规定](http://pub.dartlang.org/doc/package-layout.html)相符.

首先, 让我们来看一下 **index.html**. 注意一下在 `<html>` 元素上额外添加的 `ng-app` 指令.

``` html
<html ng-app>
```

这个 `ng-app` 指令告诉了 Angular 哪个元素是应用的根元素. 这个元素内的所有东西都是由 Angular 管理的页面模板的一部分. 除非你有其他的原因只能让 Angular 管理应用的一部分, 否则我们推荐你将 `ng-app` 指令放到 `<html>` 元素上, 因为它是最外层的标签. 这也是当页面上没有找到 `ng-app` 指令时的默认行为.

下一步, 让我们看一下 HTML 里 `<head>` 内的两个 script 标签.

``` html
<script src="packages/web_components/platform.js"></script>
<script src="packages/web_components/dart_support.js"></script>
```

这两个 script 标签会让旧版浏览器支持 Shadow DOM (一个新的 Web 平台特性).

再下一步, 让我们看一下结尾处的两个 script 标签.

``` html
<script type="application/dart" src="main.dart"></script>
<script type="text/javascript" src="packages/browser/dart.js"></script>
```

> **译者注:** 第二个标签上的 `type="text/javascript"` 并不是必须的.
