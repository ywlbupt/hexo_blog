title: hexo-jacman-多级目录的具体实现.md
date: 2016-08-15 23:18:21
tags: [hexo]
category: [markdown,hexo]
description:
---

[toc]

## 问题

Hexo基本主题模板自带的分类显示都是一级的，如何显示多级?

## 解决方案

参考了seay的[Hexo主题实现多级分类显示]，Git clone了jacman的主题，基本上能够照搬Seay的实现方式，方法如下:

1. 利用Hexo 3 更新的`list_categories([categories], [options])`**辅助函数**生成分类列表;
2. 利用css实现样式.

### 具体实现

<!-- more -->

以`jacman`主题为例(ps:`EJS node.js模板引擎`)

1. 在主题文件夹下找到layout/_widget/category.ejs文件,内容如下:
    ```html
    <% if (site.categories.length){ %>
    <div class="categorieslist">
        <p class="asidetitle"><%= __('categories') %></p>
            <ul>
            <% site.categories.sort('name').each(function(item){ %>
              <% if(item.posts.length){ %>
                <li><a href="<%- config.root %><%- item.path %>" title="<%= item.name %>"><%= item.name %><sup><%= item.posts.length %></sup></a></li>
              <% } %>
            <% }); %>
            </ul>
    </div>
    <% } %>
    ```
2. 修改内容,利用上面提到的list_categories([categories], [options])辅助函数:
    ```html
    <% if (site.categories.length){ %>
    <div class="category-block">
      <h3 class="asidetitle"><%= __('categories') %></h3>
         <%- list_categories(site.categories) %>
    </div>
    <% } %>
    ```
3. 修改样式文件
   在主题文件夹下找到`source/css/_partial/aside.styl`(jacman)文件，其他主题也可能是`source/css/_partial/sidebar.styl`
    ```html
    //categories
    .category-block>ul>li
      border-bottom 1px solid #ccc
    .category-block li
      margin-bottom 8px
    .category-list
      @media mini
        width 45%
        float left
        margin 0 5% 0 0
      @media tablet
        width 100%
        float none
        margin .5em 0 0
      .categoriy-list-item
        padding .5em 5%
      .category-list-count
        top -.5em
        padding-left .3em
        font-size 75%
        line-height 0
        position relative
        vertical-align baseline
      ul, ol, dl
        list-style none
      ul, ol, dl
        background-color #f9f9fa
        margin-left 20px
        li
          border-bottom 1px dashed #ccc
      .category-list-child
        border-top 1px dashed #ccc
        margin-bottom 8px
    ```


## Hexo的`category`的语法

在`front-matter`中，`yaml`格式如下所示：
```
title: python-返回函数
date: 2016-01-16 10:31:49
tags: python
category: [First,second,Third]
```

## 实现的效果如下所示：  

![多级目录效果图](images/markdown/hexo-multi-catalog-display.jpg)

## 参考链接：

1. [Hexo主题实现多级分类显示——Seay](https://segmentfault.com/a/1190000004359502)

<!-- 内嵌参考链接　-->
[Hexo主题实现多级分类显示]: https://segmentfault.com/a/1190000004359502

