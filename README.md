# 字蛛（font-spider）

[![NPM Version][npm-image]][npm-url]
[![NPM Downloads][downloads-image]][downloads-url]
[![Node.js Version][node-version-image]][node-version-url]
[![Build Status][travis-ci-image]][travis-ci-url]

字蛛是一个智能 WebFont 压缩工具，它能自动分析出页面使用的 WebFont 并进行按需压缩。

<img alt="font-spider 命令行界面" width="670" src="https://cloud.githubusercontent.com/assets/1791748/15415184/8bc574ac-1e73-11e6-92b9-515281620e9d.png">

## 特性

1. 压缩字体：智能删除没有被使用的字形数据，大幅度减少字体体积
2. 生成字体：支持 woff2、woff、eot、svg 字体格式生成


## 安装

安装好 [nodejs](http://nodejs.org)，然后执行：

``` shell
npm install font-spider -g
```

## 使用范例

### 一、书写 CSS

``` css
/*声明 WebFont*/
@font-face {
  font-family: 'source';
  src: url('../font/source.eot');
  src:
    url('../font/source.eot?#font-spider') format('embedded-opentype'),
    url('../font/source.woff2') format('woff2'),
    url('../font/source.woff') format('woff'),
    url('../font/source.ttf') format('truetype'),
    url('../font/source.svg') format('svg');
  font-weight: normal;
  font-style: normal;
}

/*使用指定字体*/
.home h1, .demo > .test {
    font-family: 'source';
}
```

> 特别说明： `@font-face` 中的 `src` 定义的 .ttf 文件必须存在，其余的格式将由工具自动生成

### 二、压缩 WebFont

``` shell
font-spider [options] <htmlFile1 htmlFile2 ...>
```

#### htmlFiles

一个或多个页面地址，支持 http 形式。

例如：

``` shell
font-spider dest/news.html dest/index.html dest/about.html
```

#### options

```
-h, --help                    输出帮助信息
-V, --version                 输出当前版本号
--info                        输出 WebFont 的 JSON 描述信息，不压缩与转码
--ignore <pattern>            忽略的文件配置（支持正则表达式）
--map <remotePath,localPath>  映射 CSS 内部 HTTP 路径到本地（支持正则表达式）
--no-backup                   关闭字体备份功能
--debug                       调试模式，打开它可以显示 CSS 解析错误
```

#### 参数使用示例

使用通配符压缩多个 HTML 文件关联的 WebFont：

``` shell
font-spider dest/*.html
```

`--info` 查看网站所应用的 WebFont：

``` shell
font-spider --info http://fontawesome.io
```

`--ignore` 忽略文件：

``` shell
font-spider --ignore "icon\\.css$" dest/*.html
```

`--map` 参数将线上的页面的 WebFont 映射到本地来进行压缩（本地路径必须使用绝对路径）：

``` shell
font-spider --map "http://font-spider.org/font,/Website/font" http://font-spider.org/index.html
```

## API

font-spider 包括爬虫与压缩器模块，接口文档：[API.md](./API.md)

## 限制

- 不支持 javascript 动态插入的元素与样式
- .otf 字体需要转换成 .ttf 格式才能被压缩（[免费 ttf 字体资源](#免费字体)）
- 仅支持 `utf-8` 编码的 HTML 与 CSS 文件
- CSS `content` 仅支持 `content: 'prefix'` 和 `content: attr(value)` 这两种形式

## 字体兼容性参考

| 格式      | IE   | Edge | Firefox | Chrome | Safari | Opera | iOS Safari | Android Browser | Chrome for Android |
| -------  | ---- | ---- | ------- | ------ | ------ | ----- | ---------- | --------------- | ------------------ |
| `.eot`   | 6    | \-\- | \-\-    | \-\-   | \-\-   | \-\-  | \-\-       | \-\-            | \-\-               |
| `.woff`  | 9    | 13   | 3.6     | 5      | 5.1    | 11.1  | 5.1        | 4.4             | 36                 |
| `.woff2` | \-\- | 14   | 39      | 36     | \-\-   | 23    | \-\-       | 50              | 50                 |
| `.ttf`   | \-\- | 13   | 3.5     | 4      | 3.1    | 10.1  | 4.3        | 2.2             | 36                 |
| `.svg`   | \-\- | \-\- | \-\-    | 4      | 3.2    | 9.6   | 3.2        | 3               | 36                 |

来源：<http://caniuse.com/#feat=fontface>


