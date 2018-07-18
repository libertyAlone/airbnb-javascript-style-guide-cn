# Airbnb CSS-in-JavaScript编码规范

*合理的方式书写CSS-in-JavaScript*

## 内容

1. [命名](#命名)
1. [O顺序](#顺序)
1. [嵌套](#嵌套)
1. [内联](#内联)
1. [主题](#主题)

## 命名

  - 对象键值使用驼峰式风格(如"selectors").

    > 原因? 我们通过组件中的 `styles` 属性来访问这些键值，所以使用驼峰式是最方便的.

    ```js
    // bad
    {
      'bermuda-triangle': {
        display: 'none',
      },
    }

    // good
    {
      bermudaTriangle: {
        display: 'none',
      },
    }
    ```

  - 对于其他样式的修饰符使用下划线.

    > 原因? 类似BEM,这个命名约定清楚地表明:这些样式是用来修改下划线前面的元素. 下划线不需要加引号,因此比其他字符，如中划线，使用更多.

    ```js
    // bad
    {
      bruceBanner: {
        color: 'pink',
        transition: 'color 10s',
      },

      bruceBannerTheHulk: {
        color: 'green',
      },
    }

    // good
    {
      bruceBanner: {
        color: 'pink',
        transition: 'color 10s',
      },

      bruceBanner_theHulk: {
        color: 'green',
      },
    }
    ```

  - 给降级样式使用 `selectorName_fallback`.

    > 原因? 类似修饰符,保持命名一致有助于表明这些风格与浏览器中覆写的样式的关系.

    ```js
    // bad
    {
      muscles: {
        display: 'flex',
      },

      muscles_sadBears: {
        width: '100%',
      },
    }

    // good
    {
      muscles: {
        display: 'flex',
      },

      muscles_fallback: {
        width: '100%',
      },
    }
    ```

  - 给降级样式使用分隔符选择器.

    > 原因? 保证独立的对象中包含降级样式可以明确目标,提高可读性.

    ```js
    // bad
    {
      muscles: {
        display: 'flex',
      },

      left: {
        flexGrow: 1,
        display: 'inline-block',
      },

      right: {
        display: 'inline-block',
      },
    }

    // good
    {
      muscles: {
        display: 'flex',
      },

      left: {
        flexGrow: 1,
      },

      left_fallback: {
        display: 'inline-block',
      },

      right_fallback: {
        display: 'inline-block',
      },
    }
    ```

  - 使用设备无关名字来命名媒体查询断点 (如 "small", "medium", 和 "large").

    > 原因? 通常像 "phone", "tablet", 和 "desktop" 这样的名字不符合现实世界中设备的特点. 使用这些名字无法到达预期.

    ```js
    // bad
    const breakpoints = {
      mobile: '@media (max-width: 639px)',
      tablet: '@media (max-width: 1047px)',
      desktop: '@media (min-width: 1048px)',
    };

    // good
    const breakpoints = {
      small: '@media (max-width: 639px)',
      medium: '@media (max-width: 1047px)',
      large: '@media (min-width: 1048px)',
    };
    ```

## 顺序

  - 在组件后面定义样式.

    > 原因? 我们使用高阶组件来定义我们的样式, 这在组件定义后使用是很自然的. 将样式对象直接传给函数减少了间接性.

    ```jsx
    // bad
    const styles = {
      container: {
        display: 'inline-block',
      },
    };

    function MyComponent({ styles }) {
      return (
        <div {...css(styles.container)}>
          Never doubt that a small group of thoughtful, committed citizens can
          change the world. Indeed, it’s the only thing that ever has.
        </div>
      );
    }

    export default withStyles(() => styles)(MyComponent);

    // good
    function MyComponent({ styles }) {
      return (
        <div {...css(styles.container)}>
          Never doubt that a small group of thoughtful, committed citizens can
          change the world. Indeed, it’s the only thing that ever has.
        </div>
      );
    }

    export default withStyles(() => ({
      container: {
        display: 'inline-block',
      },
    }))(MyComponent);
    ```

## 嵌套

  - 在同一缩进级别的两个相邻的块间留一个空行.

    > 原因? 空格增加可读性并且减少了合并冲突的可能性.

    ```js
    // bad
    {
      bigBang: {
        display: 'inline-block',
        '::before': {
          content: "''",
        },
      },
      universe: {
        border: 'none',
      },
    }

    // good
    {
      bigBang: {
        display: 'inline-block',

        '::before': {
          content: "''",
        },
      },

      universe: {
        border: 'none',
      },
    }
    ```

## 内联

  - 对较高优先级的样式使用内联样式(如使用prop的值).

    > 原因? 产生样式表是有开销的，所以它们最适合独立的样式.

    ```jsx
    // bad
    export default function MyComponent({ spacing }) {
      return (
        <div style={{ display: 'table', margin: spacing }} />
      );
    }

    // good
    function MyComponent({ styles, spacing }) {
      return (
        <div {...css(styles.periodic, { margin: spacing })} />
      );
    }
    export default withStyles(() => ({
      periodic: {
        display: 'table',
      },
    }))(MyComponent);
    ```

## 主题

  - 使用像 [react-with-styles](https://github.com/airbnb/react-with-styles) 这样的抽象层来自定义主题样式. *react-with-styles 给了我们文档中例子使用的 `withStyles()`, `ThemedStyleSheet`, 和 `css()` .*

  > 原因? 有一组共享变量来定义组件样式是很有用的. 使用抽象层会让这更方便. 此外,这有助于防止组件和特定的底层实现紧密耦合,这会给予你更多自由.

  - 只在主题中定义颜色.

    ```js
    // bad
    export default withStyles(() => ({
      chuckNorris: {
        color: '#bada55',
      },
    }))(MyComponent);

    // good
    export default withStyles(({ color }) => ({
      chuckNorris: {
        color: color.badass,
      },
    }))(MyComponent);
    ```

  - 只在主题中定义字体.

    ```js
    // bad
    export default withStyles(() => ({
      towerOfPisa: {
        fontStyle: 'italic',
      },
    }))(MyComponent);

    // good
    export default withStyles(({ font }) => ({
      towerOfPisa: {
        fontStyle: font.italic,
      },
    }))(MyComponent);
    ```

  - 定义字体作为一系列相关的样式.

    ```js
    // bad
    export default withStyles(() => ({
      towerOfPisa: {
        fontFamily: 'Italiana, "Times New Roman", serif',
        fontSize: '2em',
        fontStyle: 'italic',
        lineHeight: 1.5,
      },
    }))(MyComponent);

    // good
    export default withStyles(({ font }) => ({
      towerOfPisa: {
        ...font.italian,
      },
    }))(MyComponent);
    ```

  - 在主题中定义基本的栅格单元 (返回值或者计算乘法的函数).

    ```js
    // bad
    export default withStyles(() => ({
      rip: {
        bottom: '-6912px', // 6 feet
      },
    }))(MyComponent);

    // good
    export default withStyles(({ units }) => ({
      rip: {
        bottom: units(864), // 6 feet, assuming our unit is 8px
      },
    }))(MyComponent);

    // good
    export default withStyles(({ unit }) => ({
      rip: {
        bottom: 864 * unit, // 6 feet, assuming our unit is 8px
      },
    }))(MyComponent);
    ```

  - 只在主题中定义媒体查询.

    ```js
    // bad
    export default withStyles(() => ({
      container: {
        width: '100%',

        '@media (max-width: 1047px)': {
          width: '50%',
        },
      },
    }))(MyComponent);

    // good
    export default withStyles(({ breakpoint }) => ({
      container: {
        width: '100%',

        [breakpoint.medium]: {
          width: '50%',
        },
      },
    }))(MyComponent);
    ```

  - 在主题中定义tricky的降级属性.

    > 原因? 许多 CSS-in-JavaScript 实现会把对象合并到一起,这会使得为相同属性指定具体的降级属性(如 `display`)有点棘手(tricky). 为了统一方法,将这些降级放到主题中.

    ```js
    // bad
    export default withStyles(() => ({
      .muscles {
        display: 'flex',
      },

      .muscles_fallback {
        'display ': 'table',
      },
    }))(MyComponent);

    // good
    export default withStyles(({ fallbacks }) => ({
      .muscles {
        display: 'flex',
      },

      .muscles_fallback {
        [fallbacks.display]: 'table',
      },
    }))(MyComponent);

    // good
    export default withStyles(({ fallback }) => ({
      .muscles {
        display: 'flex',
      },

      .muscles_fallback {
        [fallback('display')]: 'table',
      },
    }))(MyComponent);
    ```

  - 尽量少地创建自定义主题.许多应用可能只有一个主题.

  - 在拥有唯一的和描述性的嵌套对象下来定义主题设置的命名空间.

    ```js
    // bad
    ThemedStyleSheet.registerTheme('mySection', {
      mySectionPrimaryColor: 'green',
    });

    // good
    ThemedStyleSheet.registerTheme('mySection', {
      mySection: {
        primaryColor: 'green',
      },
    });
    ```

---

CSS双关语(?) [Saijo George](https://saijogeorge.com/css-puns/).
