---
title: react文档review
date: 2023-11-08 23:03:15
categories: 
- web前端
tags:
- React
---
# react文档阅读

Created: November 6, 2023 10:19 PM
Class: 前端
Type: React
Materials: https://react.dev/learn
Reviewed: No

### 使用jsx

1. 一个组件返回一个根元素，可以使用div包裹或者空标签包裹
    
    ```jsx
    <>
      <h1>Hedy Lamarr's Todos</h1>
      <img 
        src="https://i.imgur.com/yXOvdOSs.jpg" 
        alt="Hedy Lamarr" 
        class="photo"
      >
      <ul>
        ...
      </ul>
    </>
    ```
    
    为什么不能返回两个呢？
    
    JSX looks like HTML, but under the hood it is transformed into plain JavaScript objects. You can’t return two objects from a function without wrapping them into an array. This explains why you also can’t return two JSX tags without wrapping them into another tag or a Fragment.
    
2. jsx中的标签都必须闭合
    
    `<img>` 必须是`<img />` 
    
3. 在jsx中大部分时候使用驼峰命名法，在做迁移的时候可以使用转黄工具诸如：[https://transform.tools/html-to-jsx](https://transform.tools/html-to-jsx)
    
    例如class写成className，
    
    但是也有例外
    
    For historical reasons, `[aria-*](https://developer.mozilla.org/docs/Web/Accessibility/ARIA)` and `[data-*](https://developer.mozilla.org/docs/Learn/HTML/Howto/Use_data_attributes)` attributes are written as in HTML with dashes.
    
4. 在jsx中使用 `{}` 访问js中的内容，官网描述`In this situation, you can use curly braces in your JSX to open a window to JavaScript.`
    
    通过引号将字符串数据传递给标签或者组件属性，
    
    ```jsx
    export default function Avatar() {
      return (
        <img
          className="avatar"
          src="https://i.imgur.com/7vQD0fPs.jpg"
          alt="Gregorio Y. Zara"
        />
      );
    }
    ```
    
    如果是使用js的变量动态赋值只需要使用`{}` 替换 `“”` 
    
    ```jsx
    export default function Avatar() {
      const avatar = 'https://i.imgur.com/7vQD0fPs.jpg';
      const description = 'Gregorio Y. Zara';
      return (
        <img
          className="avatar"
          src={avatar}
          alt={description}
        />
      );
    }
    ```
    
    `{}` : ****A window into the JavaScript world****
    
    ps:****Using “double curlies”: CSS and other objects in JSX****
    
    如果赋值对象，直接写的话需要使用双括号。
    
    ```jsx
    person={{ name: "Hedy Lamarr", inventions: 5 }}
    <ul style={
      {
        backgroundColor: 'black',
        color: 'pink'
      }
    }>
    // 内联css属性需要使用驼峰命名法
    ```
    

### 组件传参props

1. step1传参
    
    ```jsx
    export default function Profile() {
      return (
        <Avatar
          person={{ name: 'Lin Lanying', imageId: '1bX5QH6' }}
          size={100}
        />
      );
    }
    ```
    
2. step2解构props
    
    ```jsx
    function Avatar({ person, size }) {
      // person and size are available here
    }
    ```
    
    ps：in fact, props *are* the only argument to your component! React component functions accept a single argument, a `props` object:
    
    组件只能接收一个参数就是props。
    
3. 给props设置默认值
    
    ```jsx
    function Avatar({ person, size = 100 }) {
      // ...
    }
    ```
    
    默认值何时生效？
    
    The default value is only used if the `size` prop is missing or if you pass `size={undefined}`. But if you pass `size={null}` or `size={0}`, the default value will **not** be used.
    
    也就是说只有不传值，或者传值为undefined时生效，null和0并不会使用默认值。
    
4. props传值简化
    
    如果我们在传值的时候属性名和变量名相同，那么我们可以这样简化代码
    
    ```jsx
    function Profile({ person, size, isSepia, thickBorder }) {
      return (
        <div className="card">
          <Avatar
            person={person}
            size={size}
            isSepia={isSepia}
            thickBorder={thickBorder}
          />
        </div>
      );
    }
    
    简化如下======>
    
    function Profile(props) {
      return (
        <div className="card">
          <Avatar {...props} />
        </div>
      );
    }
    ```
    
5. 插槽
    
    When you nest content inside a JSX tag, the parent component will receive that content in a prop called `children`. For example, the `Card` component below will receive a `children` prop set to `<Avatar />` and render it in a wrapper div:
    
    You can think of a component with a `children` prop as having a **“hole”** that can be “filled in” by its parent components with arbitrary JSX.
    
    ```jsx
    import Avatar from './Avatar.js';
    
    function Card({ children }) {
      return (
        <div className="card">
          {children}
        </div>
      );
    }
    
    export default function Profile() {
      return (
        <Card>
          <Avatar
            size={100}
            person={{ 
              name: 'Katsuko Saruhashi',
              imageId: 'YfeOqp2'
            }}
          />
        </Card>
      );
    }
    ```
    
    ![https://react.dev/images/docs/illustrations/i_children-prop.png](https://react.dev/images/docs/illustrations/i_children-prop.png)
    
6. props的值并不是静态的，而是动态变化的，需要注意的是react内部数据是单向流动的也就说我们不能在子组件内部修改props的值而是应该在父组件中修改流向子组件。
    
    However, props are [immutable](https://en.wikipedia.org/wiki/Immutable_object)—a term from computer science meaning “unchangeable”. When a component needs to change its props (for example, in response to a user interaction or new data), `it will have to “ask” its parent component to pass it *different props*—a new object`! Its old props will then be cast aside, and eventually the JavaScript engine will reclaim the memory taken by them.
    
    **Don’t try to “change props”.** When you need to respond to the user input (like changing the selected color), you will need to “set state”, which you can learn about in [State: A Component’s Memory.](https://react.dev/learn/state-a-components-memory)
    

### 条件渲染

主要语法：In React, you can conditionally render JSX using JavaScript syntax like `if` statements, `&&`, and `? :` operators.

```jsx
function Item({ name, isPacked }) {
  if (isPacked) {
    return <li className="item">{name} ✔</li>;
  }
  return <li className="item">{name}</li>;
}

export default function PackingList() {
  return (
    <section>
      <h1>Sally Ride's Packing List</h1>
      <ul>
        <Item 
          isPacked={true} 
          name="Space suit" 
        />
        <Item 
          isPacked={true} 
          name="Helmet with a golden leaf" 
        />
        <Item 
          isPacked={false} 
          name="Photo of Tam" 
        />
      </ul>
    </section>
  );
}
```