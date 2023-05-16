# React

## Hooks

### useCallback

重新渲染之间保留函数定义

```js
const cachedFn = useCallback(fn, dependencies)
```

参数：

- `fn`：是需要缓存的函数
- `dependencies` ：`fn`函数使用的响应值列表

### useContext

```js
const value = useContext(SomeContext)
```

返回组件上方最近的`SomeContext.Provider`的值，如果没有，使用默认值

用法：

- 向组件树深处传递数据
- 全局状态管理
- 主题设置
- 用户认证状态

### useDebugValue

```js
useDebugValue(value, format?)
```

在React DevTools中为自定义hook添加一个label

### useDeferredValue

```js
const deferredValue = useDeferredValue(value)
```

延迟获取最新的状态值，以实现更平滑的用户界面更新，自带防抖的

在`<Suspense>`中使用deferredValue

### useEffect

```js
useEffect(setup, dependencies?)
```

同步组件和外部系统

用途：

- 执行副作用函数：发送网络请求、访问浏览器API、订阅事件等
- 处理生命周期事件
- 数据订阅和更新
- 更新DOM调用外部函数或API

### useId

```js
const id = useId()
```

为无障碍属性生成唯一ID

### useImperativeHandle

```js
useImperativeHandle(ref, createHandle, dependencies?)
```

向父组件暴露子组件的实例或特定方法，以便父组件可以直接操作子组件

参数：

- `ref`是从`forwardRef`获得的第二个参数
- `createHandle` 定义暴露给父组件的接口

```js
import { forwardRef, useImperativeHandle, useRef } from 'react';

const ChildComponent = forwardRef((props, ref) => {
  const childRef = useRef();

  // 定义需要暴露给父组件的接口
  useImperativeHandle(ref, () => ({
    // 暴露方法
    someMethod() {
      // 执行一些操作
    },
    // 暴露属性
    someProperty: 'some value',
    // 其他需要暴露的方法和属性...
  }));

  // 子组件的其他逻辑和渲染

  return <div>Child Component</div>;
});

export default ChildComponent;

```

### useInsertionEffect

```js
useInsertionEffect(setup, dependencies?)
```

DOM提交之前执行的useEffect版本

用于动态加载CSS

### useLayoutEffect

浏览器重绘屏幕前触发

用于浏览器重绘之前测量屏幕尺寸

### useMemo

```js
const cachedValue = useMemo(calculateValue, dependencies)
```

用途：

- 缓存计算的值
- 缓存组件
- 缓存函数

### useReducer

```js
const [state, dispatch] = useReducer(reducer, initialArg, init?)
```

封装组件的状态更新逻辑

参数：

- `reducer`：定义状态更新逻辑的函数，参数为state和action，返回下一个state
- `initialArg`：初始状态

返回值：

- `state`：当前状态
- `dispatch`：更新状态触发渲染的函数

### useRef

```js
const ref = useRef(initialValue)
```

引用一个不需要渲染的值，通常用于操作DOM

### useState

```js
const [state, setState] = useState(initialState);
```

添加状态变量

### useSyncExternalStore

```js
const snapshot = useSyncExternalStore(subscribe, getSnapshot, getServerSnapshot?)
```

用于订阅外部状态store、浏览器API

参数：

- `subscribe`：订阅一个store并返回一个取消订阅的函数，需要传递一个callback参数
- `getSnapshot`： 获取store的数据快照
- `getServerSnapshot`：返回store的初始值

### useTransition

```js
const [isPending, startTransition] = useTransition()
```

更新状态不阻塞UI

参数：

- `isPending` ：是否有未处理的过渡
- `startTransition`：使得状态过渡更新

```js
  function selectTab(nextTab) {
    startTransition(() => {
      setTab(nextTab);
    });
  }
```

## 内置组件

### `<Fragment>(<>...</>)`

```
<>
  <OneChild />
  <AnotherChild />
</>
```

### `<Profiler>`

衡量渲染性能

```
<Profiler id="App" onRender={onRender}>
  <App />
</Profiler>
```

onRender回调函数

```js
function onRender(id, phase, actualDuration, baseDuration, startTime, commitTime) {
  // Aggregate or log render timings...
}
```

### `<StrictMode>`

开发环境查找BUG

### `<Suspense>`（回看）

```
<Suspense fallback={<Loading />}>
  <SomeComponent />
</Suspense>
```

fallback通常用于渲染更新前的loading组件

react在后台会先渲染value，再渲染deferedValue

## API

### createContext

创建上下文

### forwardRef

传递ref给自定义组件

```react
import { forwardRef } from 'react';
// 子组件
const MyInput = forwardRef(function MyInput(props, ref) {
  const { label, ...otherProps } = props;
  return (
    <label>
      {label}
      <input {...otherProps} ref={ref} />
    </label>
  );
});

// 父组件
function Form() {
  const ref = useRef(null);

  function handleClick() {
    ref.current.focus();
  }

  return (
    <form>
      <MyInput label="Enter your name:" ref={ref} />
      <button type="button" onClick={handleClick}>
        Edit
      </button>
    </form>
  );
}
```

### lazy

实现懒加载

```react
import { lazy } from 'react';

const MarkdownPreview = lazy(() => import('./MarkdownPreview.js'));
```

### memo

如果组件的输入没有发生变化，则跳过重新渲染

```react
import { memo } from 'react';

const SomeComponent = memo(function SomeComponent(props) {
  // ...
});
```

参数：

- Component ：需要缓存的组件
- arePropsEqual：接受前一个props和新的props，返回比较的值，指定比较函数

### startTransition

非阻塞地更新state

```js
import { startTransition } from 'react';

function TabContainer() {
  const [tab, setTab] = useState('about');

  function selectTab(nextTab) {
    startTransition(() => {
      setTab(nextTab);
    });
  }
  // ...
}
```



# React DOM



## 组件

## API

### createProtal

将组件的渲染结果插入到DOM树的其他位置

```js
<div>
  <SomeComponent />
  {createPortal(children, domNode, key?)}
</div>
```

### flushSync

同步刷新组件的渲染

### createRoot

将根组件渲染到HTML文档的特定位置

### unmountComponentAtNode