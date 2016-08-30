# Airbnb CSS / Sass 编码规范

*合理使用 CSS 和 Sass*

## 内容

  1. [术语](#术语)
    - [规则声明](#规则声明)
    - [选择器](#选择器)
    - [属性](#属性)
  1. [CSS](#css)
    - [格式化](#格式化)
    - [注释](#注释)
    - [面对对象CSS和BEM](#面对对象CSS和BEM)
    - [ID选择器](#ID选择器)
    - [JavaScript钩子](#JavaScript钩子)
    - [边框](#边框)
  1. [Sass](#sass)
    - [语法](#语法)
    - [顺序](#顺序)
    - [变量](#变量)
    - [Mixins](#mixins)
    - [扩展指令](#扩展指令)
    - [嵌套选择器](#嵌套选择器)
  1. [翻译](#翻译)

## 术语

### 规则声明

“规则声明”是给有一组属性的选择器的名字. 例子:

```css
.listing {
  font-size: 18px;
  line-height: 1.2;
}
```

### 选择器

在规则声明中, “选择器”是决定DOM树中哪些元素会被已定义的属性来描述的东西. 选择器还可以匹配HTML元素，元素的类名和ID或者其属性. 下边是选择器的一些例子:

```css
.my-element-class {
  /* ... */
}

[aria-hidden] {
  /* ... */
}
```

### 属性

最后，属性是给予被规则声明选中的元素样式的东西. 属性是键值对，一条规则声明可以包括一个或多个属性声明. 属性声明看上去是这个样子:

```css
/* some selector */ {
  background: #f1f1f1;
  color: #333;
}
```

## CSS

### 格式化

* 使用软TAB缩进 (2个空格)
* 建议类名使用中划线而不是驼峰命名.
  - 如果你使用BEM，下划线和帕斯卡风格也可以 (参见下边的 [OOCSS and BEM](#oocss-and-bem)).
* 不用使用ID选择器
* 一条规则声明中使用多个选择器时，每个选择器要占一行.
* 在规则声明的花括号`{`前使用空格
* 在属性中，在`:`后边而不是前边加一个空格.
* 在规则声明结束后新的一行使用闭合花括号 `}` 
* 在规则声明间加空行

**Bad**

```css
.avatar{
    border-radius:50%;
    border:2px solid white; }
.no, .nope, .not_good {
    // ...
}
#lol-no {
  // ...
}
```

**Good**

```css
.avatar {
  border-radius: 50%;
  border: 2px solid white;
}

.one,
.selector,
.per-line {
  // ...
}
```

### 注释

* 建议使用行注释 (在Sass中是`//`) 而不是块级注释.
* 建议注释独占一行.避免注释在行末
* 给无法自说明的代码写详细注释:
  - z-index的使用
  - 浏览器具体实现的兼容性hack

### 面对对象CSS和BEM

出于以下原因我们鼓励结合使用 面对对象CSS 和 BEM:

  * 有助于在HTML和CSS间创建明确且严格的关系
  * 有助于创建可复用且可组合的组件
  * 允许更少的嵌套和更小的特异性
  * 有助于构建可扩展的样式表

**OOCSS**, 或 “面对对象CSS”, 是鼓励你将样式表作为'对象'集合来考虑的书写CSS的方法: 整个网站可以独立使用可重用且可重复的代码片段.

  * Nicole Sullivan's [OOCSS wiki](https://github.com/stubbornella/oocss/wiki)
  * Smashing Magazine's [Introduction to OOCSS](http://www.smashingmagazine.com/2011/12/12/an-introduction-to-object-oriented-css-oocss/)

**BEM**, 或者 “块级元素修饰符”, 是一个HTML和CSS中类名的_命名规范_. 由拥有很大的代码库和心中的可扩展性的Yandex开发, 可以为实现OOCSS提供一系列规范指南.

  * CSS Trick's [BEM 101](https://css-tricks.com/bem-101/)
  * Harry Roberts' [introduction to BEM](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)

我们建议使用BEM的变种-帕斯卡风格的'块',当和组件（例如React）结合时尤其有用.下划线和中划线仍用于修饰符和子类

**例子**

```jsx
// ListingCard.jsx
function ListingCard() {
  return (
    <article class="ListingCard ListingCard--featured">

      <h1 class="ListingCard__title">Adorable 2BR in the sunny Mission</h1>

      <div class="ListingCard__content">
        <p>Vestibulum id ligula porta felis euismod semper.</p>
      </div>

    </article>
  );
}
```

```css
/* ListingCard.css */
.ListingCard { }
.ListingCard--featured { }
.ListingCard__title { }
.ListingCard__content { }
```

  * `.ListingCard` 是 “块” 代表更高级的组件
  * `.ListingCard__title` 是 “元素”， 代表 `.ListingCard` 类的后代，有助于形成块的整体.
  * `.ListingCard--featured` 是 “修饰符”，代表 `.ListingCard` 块的不同差异.

### ID选择器

由于可以通过CSS中的ID选择器选择元素, 通常被认为是反模式. ID选择器给你的规则声明引入了不必要的更高优先级的 [特异性](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity) , 并且是不可重用的.

了解更多处理特异性的信息请阅读 [CSS Wizardry's article](http://csswizardry.com/2014/07/hacks-for-dealing-with-specificity/) .

### JavaScript钩子

避免在CSS中和JavaScript中绑定相同的类名. 否则开发者在重构时通常会出现以下情况：轻则浪费时间在对照查找每个要改变的类，重则因为害怕破坏功能而不敢作出更改.

我们建议创建以`js-`开头的用于给JavaScript绑定的类名:

```html
<button class="btn btn-primary js-request-to-book">Request to Book</button>
```

### 边框

指定样式没有border时使用`0`而不是`none`.

**Bad**

```css
.foo {
  border: none;
}
```

**Good**

```css
.foo {
  border: 0;
}
```

## Sass

### 语法

* 使用 `.scss` 语法, 而不是原始的 `.sass` 语法
* 将常规CSS和 `@include` 声明按照以下逻辑排序

### 属性声明排序

1. 属性声明

    列出所有的非`@include`或嵌套选择器外标准属性声明.

    ```scss
    .btn-green {
      background: green;
      font-weight: bold;
      // ...
    }
    ```

2. `@include` 声明

    将全部`@include`放到末尾以便更易阅读整个选择器.

    ```scss
    .btn-green {
      background: green;
      font-weight: bold;
      @include transition(background 0.5s ease);
      // ...
    }
    ```

3. 嵌套选择器

    _如果必要_,将嵌套选择器放到最后. 在规则声明与嵌套选择器和相邻嵌套选择器间添加空格. 在嵌套选择的上边同样适用此规范.

    ```scss
    .btn {
      background: green;
      font-weight: bold;
      @include transition(background 0.5s ease);

      .icon {
        margin-right: 10px;
      }
    }
    ```

### 变量

建议使用中划线风格的变量名字（例如`$my-variable`）而不是驼峰式或者蛇式风格.如果只是想在同一个文件中使用，也可以在变量前加上下划线 (如`$_my-variable`).

### Mixins

Mixins 用来 DRY 你的代码, 增加复杂度或将复杂度抽象--和命名良好的函数很像. Mixins不接受参数，但是注意如果不压缩载荷（如gzip）可能会在生成的样式中导致不必要的重复代码.

### 扩展指令

应该避免`@extend`，因为很可能会引发潜在的危险行为, 尤其是当使用嵌套选择器的时候. 即便是在顶层占位符选择器使用扩展，如果选择器的顺序最终会改变，也可能会导致问题。（比如，如果它们存在于其他文件，而加载顺序发生了变化）。其实，使用 @extend 所获得的大部分优化效果，gzip 压缩已经帮助你做到了，因此你只需要通过 mixin 让样式表更符合 DRY 原则就足够了.

### 嵌套选择器

**不要嵌套超过三层深的选择器!**

```scss
.page-container {
  .content {
    .profile {
      // STOP!
    }
  }
}
```

当选择器这么长的时候，你的CSS可能会是如下这样:

* 与HTML强耦合 (脆弱) *—或者—*
* 太过具体 (强大) *—或者—*
* 不可重用


再次强调: **永远不要嵌套ID选择器！**

如果必须在开始使用ID选择器 (不应该这么做), 也不要嵌套它们. 如果你正这么做,需要再检查你的标签或指出需要如此强的具体性的原因. 如果书写格式良好的HTML和CSS，应该**永远**不要这么做.

## 翻译

  其他语言版本的编码规范:

  - ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **Chinese (Traditional)**: [ArvinH/css-style-guide](https://github.com/ArvinH/css-style-guide)
  - ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Chinese (Simplified)**: [Zhangjd/css-style-guide](https://github.com/Zhangjd/css-style-guide)
  - ![ja](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japanese**: [nao215/css-style-guide](https://github.com/nao215/css-style-guide)
  - ![ko](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Korean**: [CodeMakeBros/css-style-guide](https://github.com/CodeMakeBros/css-style-guide)
  - ![PT-BR](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Portuguese**: [felipevolpatto/css-style-guide](https://github.com/felipevolpatto/css-style-guide)  
  - ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Russian**: [Nekorsis/css-style-guide](https://github.com/Nekorsis/css-style-guide)
  - ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Spanish**: [ismamz/guia-de-estilo-css](https://github.com/ismamz/guia-de-estilo-css)
  - ![vn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Vietnam.png) **Vietnamese**: [trungk18/css-style-guide](https://github.com/trungk18/css-style-guide)
