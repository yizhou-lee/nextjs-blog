---
title: "Redux学习笔记（一）Redux的结构与数据流"
date: "2023-07-16"
---

## 安装相关依赖

```
npm install @reduxjs/toolkit react-redux
```

## 使用 redux 步骤

- 在 src 下创建 app/store.js

  - Redux 中的 store 是一个对象，用于存储应用程序的整个状态树（state tree）。它是 Redux 的核心概念之一，用于集中管理和更新应用程序的状态。

  - ```javascript
    import { configureStore } from "@reduxjs/toolkit";
    import counterReducer from "../features/counter/counterSlice";

    export const store = configureStore({
      reducer: {
        counter: counterReducer,
      },
    });
    ```

- 在 index.js 中放入 provider

  - ```javascript
    import ReactDOM from "react-dom/client";
    import "./index.css";
    import App from "./App";
    import { store } from "./app/store";
    import { provider } from "react-redux";

    const root = ReactDOM.createRoot(document.getElementById("root"));
    root.render(
      <React.StrictMode>
        <Provider store={store}>
          <App />
        </Provider>
      </React.StrictMode>
    );
    ```

- 在 src 下创建 features/counter/counterSlice.js

  - 其作用是定义一个 Redux 的 Slice（切片）用于管理一个计数器（counter）相关的状态和操作。

  - 使用`createSlice`函数从 Redux Toolkit 中创建了一个 Slice。`name`字段用于指定 Slice 的名称，`initialState`字段定义了初始状态为 0。`reducers`字段定义了一组操作（increment、decrement、reset），它们是纯函数，用于更新计数器状态。

  - 通过`createSlice`函数创建的 Slice 会自动生成与每个 reducer 对应的 action，我们可以通过解构赋值的方式将它们导出，以便在其他组件中使用。最后，我们通过`counterSlice.reducer`导出 Slice 的 reducer 函数。

  - ```javascript
    import { createSlice } from "@reduxjs/toolkit";

    const initialState = {
      count: 0,
    };

    export const counterSlice = createSlice({
      name: "counter",
      initialState,
      reducers: {
        increment: (state) => {
          state.count += 1;
        },
        decrement: (state) => {
          state.count -= 1;
        },
        reset: (state) => {
          state.count = 0;
        },
      },
    });

    export const { increment, decrement, reset } = counterSlice.actions;

    export default counterSlice.reducer;
    ```

- 创建 src/features/counter/Counter.js 组件

  - ```javascript
    import { useSelector, useDispatch } from "react-redux";
    import { increment, decrement, reset } from "./counterSlice";

    const Counter = () => {
      const count = useSelector((state) => state.counter.count);
      const dispatch = useDispatch();

      return (
        <section>
          <p>{count}</p>
          <div>
            <button onClick={() => dispatch(increment())}>+</button>
            <button onClick={() => dispatch(decrement())}>-</button>
            <button onClick={() => dispatch(reset())}>reset</button>
          </div>
        </section>
      );
    };

    export default Counter;
    ```

  - `useDispatch`用于获取 Redux store 的 dispatch 函数，以便向 Redux store 分发 actions 来改变应用程序的状态。

  - `useSelector`用于选择性地从 Redux store 中获取状态，并在状态变化时重新渲染组件，以反映最新的状态。

## 总结

1. 导入相关库（@reduxjs/toolkit & react-redux）
2. 创建 store.js 用于存放所有的 reducer
3. 创建具体的 slice file，通过 createSlice 设置 name，initialState，reducers（具体的 actions）然后 export
4. 创建 component 并通过 useDispatch 和 useSelector 从 store 中获取状态和 actions，从而改变状态。

## 例子：

1. https://github.com/gitdagray/react_redux_toolkit/tree/main/01_lesson
2. https://github.com/gitdagray/react_redux_toolkit/tree/main/02_lesson
