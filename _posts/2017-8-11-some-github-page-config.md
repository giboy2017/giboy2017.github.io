---
layout: post
title:  "一些 github page 设置及查看"
categories: github
tags:  github confi setting
author: leon
---

* content
{:toc}

# 一 首页分隔符
Jekyll has an option excerpt_separator, which is suitable for you. Things go like this:

In _config.yml:

```html
excerpt_separator: <!--more-->  # you can specify your own separator, of course.
```
<!--more-->

In you post:
```html
---
layout: post
title: Foo
---

This appears in your `index.html`

This appears, too.

<!--more-->
```

This doesn't appear. It is separated.
Note you must type exactly <!--more-->, not <!--More--> or <!-- more -->.

In your index.html:
```html

<!-- Loop in you posts -->
{% for post in site.posts %}
  <!-- Here's the header -->
  <header>
    <h2 class="title"><a href="{{ post.url }}">{{ post.title }}</a></h2>
  </header>

  <!-- Your post's summary goes here -->
  <article>{{ post.excerpt }}</article> 
{% endfor %}
The output is like this:

<header>
  <h2 class="title"><a href="Your post URL">Foo</a></h2>
</header>

<article>

This appears in your `index.html`

This appears, too.

</article>
```
# 二 使用代码高亮
![image](https://gss0.baidu.com/9vo3dSag_xI4khGko9WTAnF6hhy/zhidao/wh%3D600%2C800/sign=3cce828a0df41bd5da06e0f261eaadf3/f2deb48f8c5494eeb1cf6ab82ef5e0fe98257ea5.jpg)

# 三 站内搜索
首先在自己的项目中引入 jekyll-search.js

在项目根目录下新建 search.json，该文件的数据参考 这里，如果设置全文搜索可参考 这里；

在相关的 html 中加入如下代码，并自行编写 css,主要是results-container 搜索结果框的样式展现；
```html
<!-- Html Elements for Search -->
<div id="search-container">
    <input type="text" id="search-input" placeholder="search...">
    <ul id="results-container"></ul>
</div>
```
在项目的主要 js 文件中加入如下配置：
```html
SimpleJekyllSearch({
    searchInput: document.getElementById('search-input'),
    resultsContainer: document.getElementById('results-container'),
    json: '/search.json',
    searchResultTemplate: '<li><a href="{url}">{title}</a></li>',
    noResultsText: '',
    limit: 6,
    fuzzy: true,
    exclude: ['Welcome']
});
　```
css文件参考
```css
#results-container a,.color-title:hover,.tooltip-inner,a{text-decoration:none}
#results-container,#search-input{width:100%;background-color:#fafafa}
#results-container
{   position:absolute;
    left:0;
    top:40px;
    list-style:none;
    margin:0;
    z-index:998;
    padding-left:0;
    border-radius:0 0 5px 5px;
    box-shadow:0 0 1px #404040 inset;
    counter-reset:li;
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow: hidden;
    widht:95%;

}
#results-container li{padding:0 6px 0 16px;text-align:left;line-height:24px;position:relative}
#results-container li:before{content:counter(li);counter-increment:li;position:absolute;left:6px;top:0}
#results-container a,.color-title{color:inherit;transition:all .3s}
#results-container li:hover{color:#252dff}
#search-input {
    display: block;
    padding: 6px 6px 6px 2px;
    border: none;
    border-bottom: 1px solid #a09090;
    font-size: 12px;
    outline: 0;
    line-height: 1;
    box-sizing: border-box;
}
#search-container {
    position: relative;
    width: 100%;
    
}
```
# 四 分页插件

　　在 Jekyll 3 中，需要在 gems 中安装 jekyll-paginate 插件，并添加到 _config.yml 中，然后在 _config.yml 里边开启分页功能，格式如下：
```html
paginate:5
paginate_path: "page:num"
gems: [jekyll-paginate]
```
　　博客在 username..github.io 目录下的话，在其根目录的 index.html 下（Jekyll 的分页功能不支持 Jekyll site 中的 Markdown 或 Textile 文件。分页功能从名为 index.html 的 HTML 文件中被调用时，才能工作）添加如下代码：
```html
<!--遍历分页后的文章-->
{% for post in paginator.posts %}
  <h1><a href="{{ post.url }}">{{ post.title }}</a></h1>
  <p class="author">
    <span class="date">{{ post.date }}</span>
  </p>
  <div class="content">
    {{ post.content }}
  </div>
{% endfor %}

<!-- 分页链接 -->
<div class="pagination">
  {% if paginator.previous_page %}
    <a href="/page{{ paginator.previous_page }}" class="previous">Previous</a>
  {% else %}
    <span class="previous">Previous</span>
  {% endif %}
  <span class="page_number ">Page: {{ paginator.page }} of {{ paginator.total_pages }}</span>
  {% if paginator.next_page %}
    <a href="/page{{ paginator.next_page }}" class="next">Next</a>
  {% else %}
    <span class="next ">Next</span>
  {% endif %}
</div>
```
