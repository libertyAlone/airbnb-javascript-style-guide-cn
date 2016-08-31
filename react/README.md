# Airbnb React/JSX Style Guide

*合理书写React和JSX*

## 内容

  1. [基本规则](#基本规则)
  1. [类(Class) vs `React.createClass` vs 无状态(stateless)](#类(Class)vs`React.createClass`vs无状态(stateless))
  1. [命名](#命名)
  1. [声明](#声明)
  1. [对齐](#对齐)
  1. [引号](#引号)
  1. [空格](#空格)
  1. [属性](#属性)
  1. [引用(Refs)](#引用(Refs))
  1. [括号](#括号)
  1. [标签](#标签)
  1. [方法](#方法)
  1. [顺序](#顺序)
  1. [`isMounted`](#ismounted)

## 基本规则

  - 每个文件只包含一个React组件.
    - 但是多个[无状态组件](https://facebook.github.io/react/docs/reusable-components.html#stateless-functions)可以放到一个文件中. eslint: [`react/no-multi-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-multi-comp.md#ignorestateless).
  - 使用JSX语法.
  - 除非是在非JSX文件中初始化应用，否则不要使用`React.createElement`.

## Class vs `React.createClass` vs stateless

  - 如果组件有内部状态或引用, 建议使用 `class extends React.Component` 而不是 `React.createClass` 除非有充足的理由使用mixin. eslint: [`react/prefer-es6-class`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-es6-class.md) [`react/prefer-stateless-function`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-stateless-function.md)

    ```jsx
    // bad
    const Listing = React.createClass({
      // ...
      render() {
        return <div>{this.state.hello}</div>;
      }
    });

    // good
    class Listing extends React.Component {
      // ...
      render() {
        return <div>{this.state.hello}</div>;
      }
    }
    ```

    如果没有状态或引用, 建议使用普通函数 (非箭头函数) 而不是类(class):

    ```jsx
    // bad
    class Listing extends React.Component {
      render() {
        return <div>{this.props.hello}</div>;
      }
    }

    // bad (relying on function name inference is discouraged)
    const Listing = ({ hello }) => (
      <div>{hello}</div>
    );

    // good
    function Listing({ hello }) {
      return <div>{hello}</div>;
    }
    ```

## 命名

  - **文件后缀**: React组件使用`.jsx`后缀.
  - **文件名**: 文件名使用帕斯卡风格. 如 `ReservationCard.jsx`.
  - **引用命名**: React组件使用帕斯卡风格，组件实例使用驼峰风格. eslint: [`react/jsx-pascal-case`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-pascal-case.md)

    ```jsx
    // bad
    import reservationCard from './ReservationCard';

    // good
    import ReservationCard from './ReservationCard';

    // bad
    const ReservationItem = <ReservationCard />;

    // good
    const reservationItem = <ReservationCard />;
    ```

  - **组件命名**: 使用文件名作为组件名字. 例如, `ReservationCard.jsx` 应该包含名为 `ReservationCard`的引用. 然而对于文件夹中的根组件, 使用 `index.jsx` 作为文件名，使用文件夹的名字作为组件的名字:

    ```jsx
    // bad
    import Footer from './Footer/Footer';

    // bad
    import Footer from './Footer/index';

    // good
    import Footer from './Footer';
    ```
  - **高阶组件命名**: 对于生成的组件使用高阶组件的名字和传入组件的名字的组合作为`displayName`. 例如高阶组件 `withFoo()`, 当传入组件 `Bar`时应该生成叫`withFoo(Bar)`作为`displayName`的组件.

  > 原因? 组件的 `displayName` 名字能够用于开发工具或者错误信息, 有一个能够明显表达这种关系的值可以帮助我们理解发生了什么.

    ```jsx
    // bad
    export default function withFoo(WrappedComponent) {
      return function WithFoo(props) {
        return <WrappedComponent {...props} foo />;
      }
    }

    // good
    export default function withFoo(WrappedComponent) {
      function WithFoo(props) {
        return <WrappedComponent {...props} foo />;
      }

      const wrappedComponentName = WrappedComponent.displayName
        || WrappedComponent.name
        || 'Component';

      WithFoo.displayName = `withFoo(${wrappedComponentName})`;
      return WithFoo;
    }
    ```
## 声明

  - 不要使用 `displayName` 命名模块. 要通过引用来命名.

    ```jsx
    // bad
    export default React.createClass({
      displayName: 'ReservationCard',
      // stuff goes here
    });

    // good
    export default class ReservationCard extends React.Component {
    }
    ```

## 代码对齐

  - 遵循以下JSX语法的对齐风格. eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

    ```jsx
    // bad
    <Foo superLongParam="bar"
         anotherSuperLongParam="baz" />

    // good
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    />

    // if props fit in one line then keep it on the same line
    <Foo bar="bar" />

    // children get indented normally
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    >
      <Quux />
    </Foo>
    ```

## 引号

  - JSX属性要使用双引号 (`"`), 但是其他JS部分要使用单引号. eslint: [`jsx-quotes`](http://eslint.org/docs/rules/jsx-quotes)

  > 原因? JSX属性 [不能包含转译的引号](http://eslint.org/docs/rules/jsx-quotes), 所以双引号使得像`don't`一样的缩写更容易输入.
  > 一般HTML属性也是使用双引号而不是单引号,所以JSX属性同样适用.

    ```jsx
    // bad
    <Foo bar='bar' />

    // good
    <Foo bar="bar" />

    // bad
    <Foo style={{ left: "20px" }} />

    // good
    <Foo style={{ left: '20px' }} />
    ```

## 空格

  - 自闭和的标签前要加一个空格.

    ```jsx
    // bad
    <Foo/>

    // very bad
    <Foo                 />

    // bad
    <Foo
     />

    // good
    <Foo />
    ```

  - 不要再JSX的花括号里边加空格. eslint: [`react/jsx-curly-spacing`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-curly-spacing.md)

    ```jsx
    // bad
    <Foo bar={ baz } />

    // good
    <Foo bar={baz} />
    ```

## 属性

  - 属性名使用驼峰风格.

    ```jsx
    // bad
    <Foo
      UserName="hello"
      phone_number={12345678}
    />

    // good
    <Foo
      userName="hello"
      phoneNumber={12345678}
    />
    ```

  - 当属性值为`true`时可以省略. eslint: [`react/jsx-boolean-value`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-boolean-value.md)

    ```jsx
    // bad
    <Foo
      hidden={true}
    />

    // good
    <Foo
      hidden
    />
    ```

  - `<img>`标签中要包含`alt`属性. 如果图片是presentational(?不明), `alt`可以为空字符串或者`<img>`要有`role="presentation"`. eslint: [`jsx-a11y/img-has-alt`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/img-has-alt.md)

    ```jsx
    // bad
    <img src="hello.jpg" />

    // good
    <img src="hello.jpg" alt="Me waving hello" />

    // good
    <img src="hello.jpg" alt="" />

    // good
    <img src="hello.jpg" role="presentation" />
    ```

  - 不要在`<img>`的`alt`属性中使用诸如"image", "photo", 或"picture"之类的词. eslint: [`jsx-a11y/img-redundant-alt`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/img-redundant-alt.md)

  > 原因? 屏幕阅读器已经声明`img`元素就是图片了，所以没必要在alt文本中包含这些信息.

    ```jsx
    // bad
    <img src="hello.jpg" alt="Picture of me waving hello" />

    // good
    <img src="hello.jpg" alt="Me waving hello" />
    ```

  - 使用合法的非抽象的 [ARIA roles](https://www.w3.org/TR/wai-aria/roles#role_definitions). eslint: [`jsx-a11y/aria-role`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/aria-role.md)

    ```jsx
    // bad - not an ARIA role
    <div role="datepicker" />

    // bad - abstract ARIA role
    <div role="range" />

    // good
    <div role="button" />
    ```

  - 不要在元素上使用 `accessKey`. eslint: [`jsx-a11y/no-access-key`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/no-access-key.md)

  > 原因：使用的屏幕阅读器器在键盘快捷键和命令间的不一致会导致难以阅读.

  ```jsx
  // bad
  <div accessKey="h" />

  // good
  <div />
  ```

  - 避免使用数组的索引作为 `key` 属性值, 建议使用唯一的ID. ([为什么?](https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318))

  ```jsx
  // bad
  {todos.map((todo, index) =>
    <Todo
      {...todo}
      key={index}
    />
  )}

  // good
  {todos.map(todo => (
    <Todo
      {...todo}
      key={todo.id}
    />
  ))}
  ```

## 引用(Refs)

  - 要使用引用回调函数. eslint: [`react/no-string-refs`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-string-refs.md)

    ```jsx
    // bad
    <Foo
      ref="myRef"
    />

    // good
    <Foo
      ref={ref => { this.myRef = ref; }}
    />
    ```

## 圆括号

  - 当JSX标签超过一行时使用圆括号包裹. eslint: [`react/wrap-multilines`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/wrap-multilines.md)

    ```jsx
    // bad
    render() {
      return <MyComponent className="long body" foo="bar">
               <MyChild />
             </MyComponent>;
    }

    // good
    render() {
      return (
        <MyComponent className="long body" foo="bar">
          <MyChild />
        </MyComponent>
      );
    }

    // good, when single line
    render() {
      const body = <div>hello</div>;
      return <MyComponent>{body}</MyComponent>;
    }
    ```

## 标签

  - 对没有子元素的标签进行自闭和. eslint: [`react/self-closing-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/self-closing-comp.md)

    ```jsx
    // bad
    <Foo className="stuff"></Foo>

    // good
    <Foo className="stuff" />
    ```

  - 如果组件包含多行属性,在新的一行闭合标签. eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

    ```jsx
    // bad
    <Foo
      bar="bar"
      baz="baz" />

    // good
    <Foo
      bar="bar"
      baz="baz"
    />
    ```

## 方法

  - 使用箭头函数返回本地变量.

    ```jsx
    function ItemList(props) {
      return (
        <ul>
          {props.items.map((item, index) => (
            <Item
              key={item.key}
              onClick={() => doSomethingWith(item.name, index)}
            />
          ))}
        </ul>
      );
    }
    ```

  - 对于render方法，在构造函数中绑定事件处理函数. eslint: [`react/jsx-no-bind`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-no-bind.md)

  > 原因：每次进行render调用`bind`时都会创建新的函数.

    ```jsx
    // bad
    class extends React.Component {
      onClickDiv() {
        // do stuff
      }

      render() {
        return <div onClick={this.onClickDiv.bind(this)} />
      }
    }

    // good
    class extends React.Component {
      constructor(props) {
        super(props);

        this.onClickDiv = this.onClickDiv.bind(this);
      }

      onClickDiv() {
        // do stuff
      }

      render() {
        return <div onClick={this.onClickDiv} />
      }
    }
    ```

  - React组件的内部方法不要使用下划线前缀.
  > 原因：下划线前缀有时被其他语言用来表示私有性.但是JavaScript不一样，它并不支持私有性(priviate)，所有的都是公开的(public). 不管你怎么想, 属性添加下划线前缀并不会使其真正私有化, 任何属性 (无论是否有下划线前缀) 都应被视为公开的. 参考issues #1024和#490更多的讨论.

    ```jsx
    // bad
    React.createClass({
      _onClickSubmit() {
        // do stuff
      },

      // other stuff
    });

    // good
    class extends React.Component {
      onClickSubmit() {
        // do stuff
      }

      // other stuff
    }
    ```

  - 确保在`render`方法中返回值. eslint: [`require-render-return`](https://github.com/yannickcr/eslint-plugin-react/pull/502)

    ```jsx
    // bad
    render() {
      (<div />);
    }

    // good
    render() {
      return (<div />);
    }
    ```

## 顺序

  - `class extends React.Component`的顺序:

  1. 可选的 `static` 方法
  1. `constructor`
  1. `getChildContext`
  1. `componentWillMount`
  1. `componentDidMount`
  1. `componentWillReceiveProps`
  1. `shouldComponentUpdate`
  1. `componentWillUpdate`
  1. `componentDidUpdate`
  1. `componentWillUnmount`
  1. *事件处理函数* 例如`onClickSubmit()`或`onChangeDescription()`
  1. *`render`的getter方法* 如`getSelectReason()` or `getFooterContent()`
  1. *可选的render方法* 如 `renderNavigation()` or `renderProfilePicture()`
  1. `render`

  - 如何定义 `propTypes`, `defaultProps`, `contextTypes` 等...

    ```jsx
    import React, { PropTypes } from 'react';

    const propTypes = {
      id: PropTypes.number.isRequired,
      url: PropTypes.string.isRequired,
      text: PropTypes.string,
    };

    const defaultProps = {
      text: 'Hello World',
    };

    class Link extends React.Component {
      static methodsAreOk() {
        return true;
      }

      render() {
        return <a href={this.props.url} data-id={this.props.id}>{this.props.text}</a>
      }
    }

    Link.propTypes = propTypes;
    Link.defaultProps = defaultProps;

    export default Link;
    ```

  - `React.createClass`的顺序: eslint: [`react/sort-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/sort-comp.md)

  1. `displayName`
  1. `propTypes`
  1. `contextTypes`
  1. `childContextTypes`
  1. `mixins`
  1. `statics`
  1. `defaultProps`
  1. `getDefaultProps`
  1. `getInitialState`
  1. `getChildContext`
  1. `componentWillMount`
  1. `componentDidMount`
  1. `componentWillReceiveProps`
  1. `shouldComponentUpdate`
  1. `componentWillUpdate`
  1. `componentDidUpdate`
  1. `componentWillUnmount`
  1. *事件处理函数* 如 `onClickSubmit()` 或 `onChangeDescription()`
  1. *`render`的getter方法* 如 `getSelectReason()` 或 `getFooterContent()`
  1. *可选的render方法* 如 `renderNavigation()` 或 `renderProfilePicture()`
  1. `render`

## `isMounted`

  - 不要使用 `isMounted`. eslint: [`react/no-is-mounted`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-is-mounted.md)

  > 原因： [`isMounted`是反模式][反模式], 当使用ES6的时候不可用,正逐渐被官方废弃.

  [anti-pattern]: https://facebook.github.io/react/blog/2015/12/16/ismounted-antipattern.html
I
## 翻译

  其他语言的JSX/React编码规范翻译:

  - ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Chinese (Simplified)**: [JasonBoy/javascript](https://github.com/JasonBoy/javascript/tree/master/react)
  - ![pl](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Poland.png) **Polish**: [pietraszekl/javascript](https://github.com/pietraszekl/javascript/tree/master/react)
  - ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Korean**: [apple77y/javascript](https://github.com/apple77y/javascript/tree/master/react)
  - ![Br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Portuguese**: [ronal2do/javascript](https://github.com/ronal2do/airbnb-react-styleguide)
  - ![jp](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japanese**: [mitsuruog/javascript-style-guide](https://github.com/mitsuruog/javascript-style-guide)

**[⬆ back to top](#table-of-contents)**
