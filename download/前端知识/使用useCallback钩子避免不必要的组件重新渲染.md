# 使用useCallback钩子避免不必要的组件重新渲染

**问题描述：**

<font style="color:rgba(0, 0, 0, 0.9);">在 React 中，函数组件的每次渲染都会重新创建函数。如果这些函数被作为子组件的 props 传递，或者被用作某些依赖于函数引用的逻辑（如事件处理器、回调函数等），那么每次组件重新渲染时，这些函数的引用都会改变。这会导致子组件也重新渲染，即使子组件的逻辑并没有真正改变，从而造成不必要的性能开销。</font>

**解决方法：**

`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgba(0, 0, 0, 0.03);">useCallback</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 用于缓存一个函数，使其在组件的多次渲染过程中保持引用稳定（即函数的内存地址不变）。当组件重新渲染时，如果依赖项没有发生变化，</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgba(0, 0, 0, 0.03);">useCallback</font>`<font style="color:rgba(0, 0, 0, 0.9);"> 会返回之前缓存的函数，而不是创建一个新的函数。</font>





> 更新: 2025-06-24 09:04:31  
> 原文: <https://www.yuque.com/quwaidi-exykz/lv0i4k/dn8rvrhflv7vu6vy>