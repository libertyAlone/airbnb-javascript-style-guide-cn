# Airbnb React/JSX Style Guide

*合理书写React和JSX*
该规范基于目前流行的JavaScript标准.尽管一些习惯要视情况来允许或禁止(如async/await或者静态的类字段).现阶段,任何超过stage 3的写法均不在此规范中且不推荐使用.
## 内容

  1. [基本规则](#基本规则)
  2. [类(Class) vs `React.createClass` vs 无状态(stateless)](#类vs`React.createClass`vs无状态)
  3. [混入(Mixin)](#混入)
  4. [命名](#命名)
  5. [声明](#声明)
  6. [对齐](#对齐)
  7. [引号](#引号)
  8. [空格](#空格)
  9. [属性(Props)](#属性)
  10. [引用(Refs)](#引用)
  11. [括号](#括号)
  12. [标签](#标签)
  13. [方法](#方法)
  14. [顺序](#顺序)
  15. [`isMounted`](#isMounted)

## 基本规则

  - 每个文件只包含一个React组件.
    - 但是多个[无状态组件](https://facebook.github.io/react/docs/reusable-components.html#stateless-functions)可以放到一个文件中. eslint: [`react/no-multi-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-multi-comp.md#ignorestateless).
  - 使用JSX语法.
  - 除非是在非JSX文件中初始化应用，否则不要使用`React.createElement`.

## Class vs `React.createClass` vs stateless

  - 如果组件有内部状态或引用, 建议使用 `class extends React.Component` 而不是 `React.createClass`. eslint: [`react/prefer-es6-class`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-es6-class.md) [`react/prefer-stateless-function`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-stateless-function.md)

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
## 混入
  - [不要使用mixin](#https://facebook.github.io/react/blog/2016/07/13/mixins-considered-harmful.html)
  > 原因: Mixin引入了隐式的依赖,导致命名冲突并且复杂度会滚雪球式的增加.多数mixin的情形都可以通过组件,高阶组件或工具模块等更好的方式实现.
  
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
   - **属性命名**: 避免出于不同的目的使用DOM组件的属性名.
  > 原因? 人们期望像`style`和`className`这样的属性只代表具体的事情.为你的应用去改变这种API会使代码难以阅读和维护，可能会产生bug.

    ```jsx
    // bad
    <MyComponent style="fancy" />
    
	// bad
    <MyComponent className="fancy" />
    
    // good
    <MyComponent variant="fancy" />
    ```
## 声明

  - 不要使用 `displayName` 命名组件. 要通过引用来命名.

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

## 对齐

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
    
    // bad
    {showButton &&
      <Button />
    }

    // bad
    {
      showButton &&
        <Button />
    }

    // good
    {showButton && (
      <Button />
    )}

    // good
    {showButton && <Button />}

    ```

## 引号

  - JSX属性要使用双引号 (`"`), 但是其他JS部分要使用单引号. eslint: [`jsx-quotes`](http://eslint.org/docs/rules/jsx-quotes)

  > 原因? 一般HTML属性也是使用双引号而不是单引号,所以JSX属性同样适用.

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

  - 自闭合的标签前要加一个空格. eslint: [`no-multi-spaces`](https://eslint.org/docs/rules/no-multi-spaces), [`react/jsx-tag-spacing`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-tag-spacing.md)

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

  - 不要在JSX的花括号里边加空格. eslint: [`react/jsx-curly-spacing`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-curly-spacing.md)

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
    
    // good
    <Foo hidden />
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
  - 为所有的非必需属性定义明确的默认值(defaultProps).

  > 原因? propTypes是一种文档形式，提供defaultProps意味着阅读你代码的人不需要假想太多.此外,这也可以让你的代码忽略具体的类型检查.

  ```jsx
  // bad
  function SFC({ foo, bar, children }) {
    return <div>{foo}{bar}{children}</div>;
  }
  SFC.propTypes = {
    foo: PropTypes.number.isRequired,
    bar: PropTypes.string,
    children: PropTypes.node,
  };

  // good
  function SFC({ foo, bar, children }) {
    return <div>{foo}{bar}{children}</div>;
  }
  SFC.propTypes = {
    foo: PropTypes.number.isRequired,
    bar: PropTypes.string,
    children: PropTypes.node,
  };
  SFC.defaultProps = {
    bar: '',
    children: null,
  };
  ```

  - 谨慎地使用扩展属性.
  > 原因? 除非你很大可能向下传递给组件不必要的属性. 对于React v15.6.1和更老的版本, 你可以 [给DOM传递非法的HTML属性](https://reactjs.org/blog/2017/09/08/dom-attributes-in-react-16.html).

  例外:

  - 向下代理属性并且提升propType的HOC

  ```jsx
  function HOC(WrappedComponent) {
    return class Proxy extends React.Component {
      Proxy.propTypes = {
        text: PropTypes.string,
        isLoading: PropTypes.bool
      };

      render() {
        return <WrappedComponent {...this.props} />
      }
    }
  }
  ```

  - 对于已知且确定的属性来扩展对象. 这对于用MoCha的beforeEach构造方法来测试React组件会非常有用.

  ```jsx
  export default function Foo {
    const props = {
      text: '',
      isPublished: false
    }

    return (<div {...props} />);
  }
  ```

  使用注意事项:
  可能的话过滤掉不必要的属性. 使用 [prop-types-exact](https://www.npmjs.com/package/prop-types-exact) 来避免bug.

  ```jsx
  // good
  render() {
    const { irrelevantProp, ...relevantProps  } = this.props;
    return <WrappedComponent {...relevantProps} />
  }

  // bad
  render() {
    const { irrelevantProp, ...relevantProps  } = this.props;
    return <WrappedComponent {...this.props} />
  }
  ```

## 引用

  - 要使用回调函数作为引用. eslint: [`react/no-string-refs`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-string-refs.md)

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

  - 使用箭头函数包裹本地变量.

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
  > 原因：下划线前缀有时被其他语言用来表示私有性.但是JavaScript不一样，它并不支持私有性(priviate)，所有的都是公开的(public). 不管你怎么想, 属性添加下划线前缀并不会使其真正私有化, 任何属性 (无论是否有下划线前缀) 都应被视为公开的. 参考[#1024](https://github.com/airbnb/javascript/issues/1024)和[#490](https://github.com/airbnb/javascript/issues/490)更多的讨论.

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

  > 原因： [`isMounted` 是反模式][anti-pattern], 当使用ES6的时候不可用,正逐渐被官方废弃.

  [anti-pattern]: https://facebook.github.io/react/blog/2015/12/16/ismounted-antipattern.html
I
## 翻译

  其他语言的JSX/React编码规范翻译:

  - ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Chinese (Simplified)**: [JasonBoy/javascript](https://github.com/JasonBoy/javascript/tree/master/react)
  - ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **Chinese (Traditional)**: [jigsawye/javascript](https://github.com/jigsawye/javascript/tree/master/react)
  - ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Español**: [agrcrobles/javascript](https://github.com/agrcrobles/javascript/tree/master/react)
  - ![jp](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japanese**: [mitsuruog/javascript-style-guide](https://github.com/mitsuruog/javascript-style-guide/tree/master/react)
  - ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Korean**: [apple77y/javascript](https://github.com/apple77y/javascript/tree/master/react)
  - ![pl](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Poland.png) **Polish**: [pietraszekl/javascript](https://github.com/pietraszekl/javascript/tree/master/react)
  - ![Br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Portuguese**: [ronal2do/javascript](https://github.com/ronal2do/airbnb-react-styleguide)
  - ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Russian**: [leonidlebedev/javascript-airbnb](https://github.com/leonidlebedev/javascript-airbnb/tree/master/react)
  - ![th](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Thailand.png) **Thai**: [lvarayut/javascript-style-guide](https://github.com/lvarayut/javascript-style-guide/tree/master/react)
  - ![ua](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Ukraine.png) **Ukrainian**: [ivanzusko/javascript](https://github.com/ivanzusko/javascript/tree/master/react)

**[⬆ back to top](#table-of-contents)**
