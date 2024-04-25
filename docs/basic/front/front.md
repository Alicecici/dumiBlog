---
nav:
  title: 基础知识
  order: 0
  second:
    title: 前端
    order: 0
---

# 前端部分

## 1. flex 布局

**flex 布局和响应式布局的区别**

Flex 布局是一种弹性盒子布局，通过设置容器的 display 属性为 flex，可以让子元素按照一定的规则进行排列和对齐。Flex 布局可以实现灵活的布局，适应不同屏幕尺寸和内容长度的变化。

响应式布局是一种根据设备屏幕尺寸和浏览器窗口大小来调整布局的方式。通过使用媒体查询和 CSS3 的响应式设计技术，可以实现在不同设备上展示不同的布局和样式。响应式布局可以让网页在不同设备上都能够有良好的显示效果。

总结，Flex 布局主要是针对子元素的排列和对齐进行灵活布局，而响应式布局主要是针对整体布局的调整来适应不同设备和屏幕尺寸。

参考文档：https://ruanyifeng.com/blog/2015/07/flex-grammar.html

## 2. css 全局变量和 css modules 的使用

CSS Modules 是一种让 CSS 具有局部作用域的技术，它将 CSS 文件中的类名（选择器）自动进行命名空间处理，避免全局污染，从而实现了局部作用域的效果。

缺点：

1. **学习成本**：对于初学者来说，需要花一些时间来了解和适应 CSS Modules 的工作原理和使用方法。
2. **文件体积增加**：由于 CSS Modules 会自动生成独一无二的类名，可能会导致生成的 CSS 文件体积增加，但这通常不会对性能产生显著影响。
3. **不适用于全局样式**：有些样式可能需要在全局中生效，使用 CSS Modules 可能会增加一定的复杂度。

## 3.表单

1：表单的验证方式有哪些？

1. **客户端验证**：在用户填写表单时，通过 JavaScript 代码对表单字段进行验证，例如检查是否为空、格式是否正确等。常见的客户端验证方式包括使用正则表达式、内置的 HTML5 表单验证属性（如 required、pattern 等）以及前端验证库（如 Validator.js、Joi 等）。
2. **服务端验证**：在表单提交到后端之后，后端服务器可以再次对表单数据进行验证，确保数据的完整性和正确性。服务端验证可以防止绕过客户端验证而提交错误数据，同时也可以保护后端系统的安全。
3. **第三方库和框架**：许多前端框架和库提供了方便的表单验证功能，如 React Hook Form、Formik、VeeValidate 等，可以简化表单验证的实现过程并提供丰富的验证规则和反馈方式。
4. **自定义验证规则**：根据具体业务需求，可以自定义验证规则来满足特定的验证要求，例如自定义正则表达式、自定义验证函数等。

2：表单的提交方式有哪些？

1. **同步提交**：通过 form 标签的 submit 按钮实现表单的同步提交，页面会刷新并等待服务器响应。这是最传统的表单提交方式，适用于一些简单的场景。
2. **异步提交**：通过 JavaScript 代码监听表单的提交事件，使用 Ajax 技术（如 XMLHttpRequest、Fetch API、axios 等）将表单数据异步提交到服务器，实现无需刷新页面的数据提交和响应。
3. **使用表单库**：一些现代的表单库和框架提供了更加便捷的表单提交方式，例如 React Hook Form、Formik 等，它们提供了简洁的 API 和状态管理，可以方便地处理表单数据的提交和验证。
4. **RESTful API 提交**：如果后端接口采用 RESTful 风格，表单提交可以按照 RESTful API 的规范进行，使用 POST、PUT 等 HTTP 请求方法提交表单数据。
5. **WebSocket 提交**：在一些实时性要求高的场景下，可以使用 WebSocket 技术实现表单数据的实时提交和更新，实现即时通讯和数据同步。

## 4.axios 的使用

需要回答的问题 1：axios 和 fetch 的区别

1. 拦截器：
   - axios：提供拦截器功能，用于在请求处理之前或之后执行一些操作，例如日志记录、权限控制等。
   - fetch：没有内置的拦截器，但可以使用第三方库来实现类似的功能。
2. 错误处理：
   - axios：支持错误处理，可以抛出 HTTP 错误或其他自定义错误，并提供了默认的错误处理逻辑。
   - fetch：没有内置的错误处理逻辑，但可以使用第三方库来实现错误处理功能。
3. 配置灵活性：
   - axios：配置非常灵活，可以通过传递参数来定制请求，例如设置 baseURL、请求头等。
   - fetch：配置相对固定，可以通过继承 FetchOptions 类来定制请求，但灵活性比 axios 差。
4. 异步处理：
   - axios：默认使用 Promise 作为异步处理机制，支持同步和异步请求。
   - fetch：使用 Promise 作为异步处理机制，与 axios 类似。
5. 请求和响应数据：
   - axios： both axios and fetch can handle both request and response data, with clear and easy-to-understand data structures.
   - fetch： fetch can only handle response data, while request data is encoded in the body of the request, making it more difficult to understand and manipulate.

需要回答的问题 2：介绍下 ajax

是一种用于异步处理网页内容的技术。它允许网页在打开时动态加载数据，而不需要重新加载整个页面。AJAX 通常用于处理与服务器端的通信，通过发送 XML 请求到服务器，并在服务器端处理 XML 响应。

与 fetch 和 axios 不同，ajax 通常用于与服务器端的 XML 数据进行交互，而不是 JSON 数据。因此，ajax 请求和响应的数据结构与 fetch 和 axios 不同。

## 5.React 全局状态管理

### 什么是全局状态管理？

各个组件数据共享的一种方式，以一个全局单例模式管理，有点类似于全局的数据仓库，

对 `vue` 应用程序进行全局状态管理的工具，比如 vuex 和 pinia

### 为什么需要全局状态管理？

**参考文档**

https://umijs.org/docs/max/dva

[https://vuex.vuejs.org/zh/#%E4%BB%80%E4%B9%88%E6%98%AF%E2%80%9C%E7%8A%B6%E6%80%81%E7%AE%A1%E7%90%86%E6%A8%A1%E5%BC%8F%E2%80%9D%EF%BC%9F](https://vuex.vuejs.org/zh/#什么是“状态管理模式”？)

在项目中会遇到多个组件视图依赖同一个状态，并且其中的一个组件更改这个状态时，其余依赖了该状态的组件视图也要相应的更新。可以把组件的共享状态抽取出来，以一个全局单例模式管理。通过定义和隔离状态管理中的各种概念并通过强制规则维持视图和状态间的独立性，我们的代码将会变得更结构化且易维护。

### 在 React （umijs） 中如何使用全局状态管理？

创建一个 models，存储数据信息，useState 存储状态，useEffect 存放方法，然后导出这个 models，其他组件可以调用这个 models 的数据信息和方法，实现全局状态管理

### 常见问题/注意事项

1 页面刷新会导致全局状态丢失。在页面跳转时尽量使用组件库提供跳转组件，如`<Link>`组件。而不是原生的`<a>`标签。

2 可以使用 sessionStorage 来存储全局状态，但是需要注意的是 sessionStorage 存储的数据是字符串类型，需要手动转换。

3 在 hook 中，使用 useEffect 来在页面第一个加载完成后从 sessionStorage 中获取全局状态。

### umijs 中数据流

`@umi/max` 内置了**数据流管理**[插件](https://github.com/umijs/umi/blob/master/packages/plugins/src/model.ts)，它是一种基于 `hooks` 范式的轻量级数据管理方案，可以在 Umi 项目中管理全局的共享数据。将其中的状态或数据变成了**全局数据**，不同的组件在使用该 Model 时，拿到的是同一份状态或数据。

### umijs 中的全局初始状态

`@umi/max` 内置了**全局初始状态管理**[插件](https://github.com/umijs/umi/blob/master/packages/plugins/src/initial-state.ts)，允许您快速构建并在组件内获取 Umi 项目全局的初始状态。

全局初始状态是一种特殊的 Model。

全局初始状态在整个 Umi 项目的最开始创建。编写 `src/app.ts` 的导出方法 `getInitialState()`，其返回值将成为全局初始状态。

各种插件和您定义的组件都可以通过 `useModel('@@initialState')` 直接获取到这份全局的初始状态。

参考 umijs 官网：https://umijs.org/docs/max/data-flow
