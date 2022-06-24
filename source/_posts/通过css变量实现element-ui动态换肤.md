---
title: 通过css变量实现element-ui动态换肤
date: 2022-06-24 22:39:15
tags: [前端, css]
---

## 写在前面

element ui换肤是一个老生常谈的问题，从其发布之时就一直有讨论，本人所知的的主流换肤方案有以下几种(如有错误欢迎指正):

- ### 通过全局class样式覆盖

   定义一个class将其挂载在body上，之后为该class单独写一份样式文件

  ```scss
  .dark-theme{
      .el-button--success{
          background-color: #000000;
          border-color: #000000;
      }
  }
  ```

  ```js
  document.body.className('dark-theme')
  ```

- ### 通过 [element-theme主题生成工具生](https://github.com/ElementUI/element-theme)生成一套新的样式文件

   [element-theme](https://github.com/ElementUI/element-theme)是element ui官方提供的一套主题生成工具，可以通过替换scss变量值生成一套新的样式文件，这种方法每个主题都有一套样式文件，体积过大



## 我的思路

通过css变量与属性选择器实现

### 将element ui主题中的关键色提取为css变量

```css
// before
.el-button--success {
  color: #FFF;
  background-color: #67C23A;
  border-color: #67C23A
}
```

```css
// after
.el-button--success {
  color: #FFF;
  background-color: var(--color-success-primary);
  border-color: var(--color-success-primary)
}
```

### 为不同主题定义不同的变量值

假设我们现在有两套主题：light与dark，我们只需要分别为其定义一套变量文件，之后通过在body上挂在一个自定义属性theme，属性值为主题名，之后通过标签属性选择器加载对应的变量即可

```typescript
// 为body挂载自定义属性
const changeTheme=(theme:'light'|'dark')=>{
    document.body.setAttribute('theme',theme)
}
```

```css
// light.css
body[theme='light']{
  --color-success-primary:green
}

// dark.css
body[theme='dark']{
  --color-success-primary:black
}
```

