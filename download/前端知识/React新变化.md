# React新变化

<details class="lake-collapse"><summary id="uc3caf0e2"><span class="ne-text">1.const [isPending,startTransition]=useTransition(),因为异步更新的时候，接口数据还没有响应回来的时候，我们一般都需要使用loading来等待页面的响应，但是这个loading设置的时机是我们手动控制的，现在有了这个钩子之后，就可以自动控制了。</span></summary><pre data-language="tsx" id="lCwIY" class="ne-codeblock language-tsx"><code>import React, { useState } from 'react';
const MyComponent = () =&gt; {
    const [data, setData] = useState(null);
    const [isLoading, setIsLoading] = useState(false);

    const fetchData = async () =&gt; {
        setIsLoading(true); // 手动设置加载状态
        const response = await fetch('https://api.example.com/data');
        const result = await response.json();
        setData(result);
        setIsLoading(false); // 手动清除加载状态
    };

    return (
        &lt;div&gt;
            &lt;h1&gt;My Component&lt;/h1&gt;
            {isLoading ? (
                &lt;p&gt;Loading...&lt;/p&gt;
            ) : (
                &lt;p&gt;{data ? `Data loaded: ${data}` : 'No data loaded yet.'}&lt;/p&gt;
            )}
            &lt;button onClick={fetchData} disabled={isLoading}&gt;
                {isLoading ? 'Loading...' : 'Load Data'}
            &lt;/button&gt;
        &lt;/div&gt;
    );
};

export default MyComponent;</code></pre><pre data-language="tsx" id="PXqCe" class="ne-codeblock language-tsx"><code>import React, { useState, useTransition } from 'react';
const MyComponent = () =&gt; {
    const [data, setData] = useState(null);
    const [isPending, startTransition] = useTransition();

    const fetchData = async () =&gt; {
        const response = await fetch('https://api.example.com/data');
        const result = await response.json();
        setData(result);
    };

    const loadData = () =&gt; {
        startTransition(fetchData); // 使用 startTransition 包装异步操作
    };

    return (
        &lt;div&gt;
            &lt;h1&gt;My Component&lt;/h1&gt;
            {isPending ? (
                &lt;p&gt;Loading...&lt;/p&gt;
            ) : (
                &lt;p&gt;{data ? `Data loaded: ${data}` : 'No data loaded yet.'}&lt;/p&gt;
            )}
            &lt;button onClick={loadData} disabled={isPending}&gt;
                {isPending ? 'Loading...' : 'Load Data'}
            &lt;/button&gt;
        &lt;/div&gt;
    );
};

export default MyComponent;</code></pre></details>
| **<font style="color:rgba(0, 0, 0, 0.9);">特性</font>** | `**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgba(0, 0, 0, 0.03);">useState</font>**` | `**<font style="color:rgba(0, 0, 0, 0.9);background-color:rgba(0, 0, 0, 0.03);">useTransition</font>**` |
| :--- | :--- | :--- |
| **<font style="color:rgba(0, 0, 0, 0.9);">用途</font>** | <font style="color:rgba(0, 0, 0, 0.9);">管理组件状态，触发重新渲染</font> | <font style="color:rgba(0, 0, 0, 0.9);">管理异步操作的过渡状态，优化用户体验</font> |
| **<font style="color:rgba(0, 0, 0, 0.9);">更新方式</font>** | <font style="color:rgba(0, 0, 0, 0.9);">手动调用 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgba(0, 0, 0, 0.03);">setState</font>`<font style="color:rgba(0, 0, 0, 0.9);">更新状态</font> | <font style="color:rgba(0, 0, 0, 0.9);">自动管理 </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgba(0, 0, 0, 0.03);">isPending</font>`<font style="color:rgba(0, 0, 0, 0.9);">状态</font> |
| **<font style="color:rgba(0, 0, 0, 0.9);">是否触发重新渲染</font>** | <font style="color:rgba(0, 0, 0, 0.9);">是，状态更新后触发重新渲染</font> | <font style="color:rgba(0, 0, 0, 0.9);">不直接触发重新渲染，但会影响渲染逻辑</font> |
| **<font style="color:rgba(0, 0, 0, 0.9);">适用场景</font>** | <font style="color:rgba(0, 0, 0, 0.9);">适用于所有需要状态管理的场景</font> | <font style="color:rgba(0, 0, 0, 0.9);">适用于异步操作（如网络请求）</font> |
| **<font style="color:rgba(0, 0, 0, 0.9);">是否需要手动管理状态</font>** | <font style="color:rgba(0, 0, 0, 0.9);">是，需要手动调用</font><font style="color:rgba(0, 0, 0, 0.9);"> </font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgba(0, 0, 0, 0.03);">setState</font>` | <font style="color:rgba(0, 0, 0, 0.9);">不需要，</font>`<font style="color:rgba(0, 0, 0, 0.9);background-color:rgba(0, 0, 0, 0.03);">isPending</font>`<font style="color:rgba(0, 0, 0, 0.9);">自动管理</font> |
| **<font style="color:rgba(0, 0, 0, 0.9);">是否支持渐进式渲染</font>** | <font style="color:rgba(0, 0, 0, 0.9);">不支持，状态更新会阻塞渲染</font> | <font style="color:rgba(0, 0, 0, 0.9);">支持，允许在异步操作完成前继续渲染</font> |


<details class="lake-collapse"><summary id="u13a385ff"><span class="ne-text">useOptimistic钩子函数：用于实现乐观更新（在用户界面提前显示预期结果的技术），而不是等后端操作完成后在更新界面。</span></summary><div data-type="color3" class="ne-alert"><p id="u6e330936" class="ne-p"><span class="ne-text">核心思想：</span></p><ol class="ne-ol"><li id="u13de2312" data-lake-index-type="0"><span class="ne-text">立即更新页面（应该是正确的结果的页面）</span></li><li id="ueb1d6da4" data-lake-index-type="0"><span class="ne-text">处理冲突，后端操作失败，会回滚到之前的状态，并提示用户。</span></li><li id="u0cada42a" data-lake-index-type="0"><span class="ne-text">实现方式：</span><code class="ne-code"><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); background-color: rgba(0, 0, 0, 0.03); font-size: 16px">useOptimistic</span></code><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 16px"> 钩子通常会返回两个值：</span><code class="ne-code"><strong><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); background-color: rgba(0, 0, 0, 0.03); font-size: 16px">optimisticValue</span></strong></code><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 16px">：一个乐观更新后的值，用于在界面中显示。</span><code class="ne-code"><strong><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); background-color: rgba(0, 0, 0, 0.03); font-size: 16px">setOptimisticValue</span></strong></code><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 16px">：一个函数，用于更新乐观值。</span></li><li id="u77c46b66" data-lake-index-type="0"><span class="ne-text">适用场景：点赞、收藏、评论、表单提交---常见于移动端</span></li></ol></div><pre data-language="tsx" id="xlYro" class="ne-codeblock language-tsx"><code>import { useState, useEffect } from 'react';
// 自定义 useOptimistic 钩子
function useOptimistic(initialValue) {
    const [optimisticValue, setOptimisticValue] = useState(initialValue);
    const [isOptimistic, setIsOptimistic] = useState(false);
    // 更新乐观值
    const updateOptimisticValue = (newValue) =&gt; {
        setOptimisticValue(newValue);
        setIsOptimistic(true);
    };
    // 回滚乐观值
    const rollbackOptimisticValue = () =&gt; {
        setOptimisticValue(initialValue);
        setIsOptimistic(false);
    };
    // 确认乐观值
    const confirmOptimisticValue = () =&gt; {
        setIsOptimistic(false);
    };
    return [optimisticValue, updateOptimisticValue, rollbackOptimisticValue, confirmOptimisticValue];
}</code></pre><pre data-language="tsx" id="j4k4K" class="ne-codeblock language-tsx"><code>import React, { useState } from 'react';
import { useOptimistic } from './useOptimistic';
const MyComponent = () =&gt; {
    const [currentName, setCurrentName] = useState('Initial Name');
    const [optimisticName, setOptimisticName, rollbackOptimisticName, confirmOptimisticName] = useOptimistic(currentName);
    const handleNameChange = async (newName) =&gt; {
        // 乐观更新界面
        setOptimisticName(newName);
        try {
            // 模拟异步请求
            await new Promise((resolve) =&gt; setTimeout(resolve, 2000));
            // 如果后端操作成功，确认乐观更新
            setCurrentName(newName);
            confirmOptimisticName();
        } catch (error) {
            // 如果后端操作失败，回滚乐观更新
            rollbackOptimisticName();
            alert('Failed to update name. Reverted to previous value.');
        }
    };

    return (
        &lt;div&gt;
            &lt;h1&gt;Current Name: {optimisticName}&lt;/h1&gt;
            &lt;input
                type=&quot;text&quot;
                value={optimisticName}
                onChange={(e) =&gt; setOptimisticName(e.target.value)}
            /&gt;
            &lt;button onClick={() =&gt; handleNameChange(optimisticName)}&gt;
                Update Name
            &lt;/button&gt;
        &lt;/div&gt;
    );
};
export default MyComponent;</code></pre><p id="u16be2f2b" class="ne-p"><span class="ne-text">与useState的区别：</span></p><ol class="ne-ol"><li id="uaedc7a27" data-lake-index-type="0"><span class="ne-text">useState用于声明和更新变量，用于触发组件重新渲染（同步操作）</span></li><li id="u009008f4" data-lake-index-type="0"><span class="ne-text">useOptimistic:用于处理乐观更新，提前显示预期结果，适用于异步操作，（减少不必要的重新渲染，提升性能）</span></li></ol></details>
<details class="lake-collapse"><summary id="u0141c76a"><span class="ne-text">useActionState</span><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 16px">是 React 19 引入的一个新 Hook，用于根据表单操作的结果更新组件状态。它特别适用于处理表单提交后的异步操作，如 API 调用、加载状态和错误反馈等场景.</span></summary><div data-type="color3" class="ne-alert"><p id="u92c7a629" class="ne-p"><span class="ne-text" style="font-size: 16px">核心功能：</span></p><ol class="ne-ol"><li id="u399e4c07" data-lake-index-type="0"><span class="ne-text" style="font-size: 16px">状态管理：返回数组包含（当前状态，表单操作函数以及一个pending标志），状态的初始值由initialValue决定，后续状态又表单操作函数的返回值决定。</span></li><li id="u528eca6a" data-lake-index-type="0"><span class="ne-text" style="font-size: 16px">表单操作函数：在表单提交时触发，接收两个参数，上一次的状态和表单数据。返回值会更新表单的状态。</span></li><li id="u7c210dd7" data-lake-index-type="0"><span class="ne-text" style="font-size: 16px">支持异步操作函数：useActionState会自动处理异步操作的加载状态，isPending标志可以用于显示加载动画和禁用按钮。</span></li><li id="u37ab1161" data-lake-index-type="0"><span class="ne-text" style="font-size: 16px">服务端组件渲染：允许在js完全加载之前显示服务器响应，可以通过permalink参数指定动态页面的url,来支持渐进式增强。</span></li></ol></div><p id="u12631136" class="ne-p"><span class="ne-text" style="font-size: 16px">基础用法：</span></p><pre data-language="tsx" id="y9yWN" class="ne-codeblock language-tsx"><code>import { useActionState } from 'react';
async function increment(previousState, formData) {
    return previousState + 1;
}
function CounterForm() {
    const [state, formAction] = useActionState(increment, 0);
    return (
        &lt;form action={formAction}&gt;
            &lt;p&gt;Counter: {state}&lt;/p&gt;
            &lt;button type=&quot;submit&quot;&gt;Increment&lt;/button&gt;
        &lt;/form&gt;
    );
}</code></pre><p id="ue5d59a27" class="ne-p"><span class="ne-text" style="font-size: 16px"></span></p></details>
<details class="lake-collapse"><summary id="uf38bd158"><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 16px">允许通过forwardRef和useImperativeHandle更优雅地处理子组件的引用（ref）和方法暴露。（使用useImperAtiveHandle这个主要是为了减少父组件对子组件内部状态的依赖，从而比面不必要的渲染）</span></summary><div data-type="color1" class="ne-alert"><p id="u1a17a3c0" class="ne-p"><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 16px">在 React 中，</span><code class="ne-code"><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); background-color: rgba(0, 0, 0, 0.03); font-size: 16px">ref</span></code><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 16px"> 是一种特殊的属性，用于直接访问 DOM 元素或子组件的实例。</span><code class="ne-code"><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); background-color: rgba(0, 0, 0, 0.03); font-size: 16px">forwardRef</span></code><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 16px"> 是一个高阶组件，允许你将一个 ref 通过组件传递给子组件，而 </span><code class="ne-code"><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); background-color: rgba(0, 0, 0, 0.03); font-size: 16px">useImperativeHandle</span></code><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 16px"> 则用于自定义暴露给父组件的实例值。</span></p></div><p id="u6a857b75" class="ne-p"><span class="ne-text" style="font-size: 16px">示例代码：</span></p><pre data-language="tsx" id="H4kzu" class="ne-codeblock language-tsx"><code>import React, { useRef, useImperativeHandle } from 'react';
const ChildComponent = React.forwardRef((props, ref) =&gt; {
    const localRef = useRef();
    useImperativeHandle(ref, () =&gt; ({
        // 暴露给父组件的方法
        focus: () =&gt; {
            localRef.current.focus();
        },
        getValue: () =&gt; {
            return localRef.current.value;
        }
    }));

    return (
        &lt;input ref={localRef} /&gt;
    );
});</code></pre><pre data-language="tsx" id="n884x" class="ne-codeblock language-tsx"><code>import React, { useRef } from 'react';
import ChildComponent from './ChildComponent';
const ParentComponent = () =&gt; {
    const childRef = useRef();
    const handleClick = () =&gt; {
        // 调用子组件暴露的方法
        childRef.current.focus();
        const value = childRef.current.getValue();
        console.log('Input value:', value);
    };
    return (
        &lt;div&gt;
            &lt;ChildComponent ref={childRef} /&gt;
            &lt;button onClick={handleClick}&gt;Focus Input and Get Value&lt;/button&gt;
        &lt;/div&gt;
    );
};</code></pre><div data-type="color5" class="ne-alert"><p id="ub4016f2f" class="ne-p"><span class="ne-text" style="font-size: 16px">核心：</span></p><ol class="ne-ol"><li id="u4c9612c3" data-lake-index-type="0"><code class="ne-code"><strong><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); background-color: rgba(0, 0, 0, 0.03); font-size: 16px">forwardRef</span></strong></code><strong><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); background-color: rgba(0, 0, 0, 0.03); font-size: 16px">是一个高阶组件，允许将ref实例传递给子组件</span></strong></li><li id="u91a2775a" data-lake-index-type="0"><strong><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); background-color: rgba(0, 0, 0, 0.03); font-size: 16px">useImperativeHandle用于自定义暴露给父组件的实例值（子组件的实例）</span></strong></li></ol></div><div data-type="success" class="ne-alert"><p id="u56754b9c" class="ne-p"><span class="ne-text" style="font-size: 16px">优势：</span></p><ol class="ne-ol"><li id="ub0e4e7eb" data-lake-index-type="0"><span class="ne-text" style="font-size: 16px">代码更加简洁：不需要手动创建或自定义ref或props传递方法</span></li><li id="u140c29cc" data-lake-index-type="0"><span class="ne-text" style="font-size: 16px">封装的更好:子组件只专注于自己的视线细节，只暴露必要的实现方法给父组件。</span></li><li id="u73e4e0c1" data-lake-index-type="0"><span class="ne-text" style="font-size: 16px">更好的性能：通过useImperativeHandle，子组件可以笔辩不必要的渲染，因为父组件只范文暴露的方法，而不是整个子组件的实例。</span></li></ol></div></details>
<details class="lake-collapse"><summary id="uee7739d9"><span class="ne-text">改造了 contextAPI 的使用，父传子不再需要写 provider 字样</span></summary><p id="ub160f998" class="ne-p"><span class="ne-text">基本用法：</span></p><ol class="ne-ol"><li id="u09013124" data-lake-index-type="0"><span class="ne-text">创建Context：</span></li></ol><pre data-language="tsx" id="zH1wJ" class="ne-codeblock language-tsx"><code>import React from 'react';
const MyContext = React.createContext();</code></pre><ol start="2" class="ne-ol"><li id="ue4f7f3a2" data-lake-index-type="0"><span class="ne-text">提供 Context 值（Provider）：</span></li></ol><pre data-language="tsx" id="j6wJI" class="ne-codeblock language-tsx"><code>import React from 'react';
import { MyContext } from './MyContext';
const ParentComponent = () =&gt; {
    const value = { name: 'Kimi' };
    return (
        &lt;MyContext.Provider value={value}&gt;
            &lt;ChildComponent /&gt;
        &lt;/MyContext.Provider&gt;
    );
};</code></pre><ol start="3" class="ne-ol"><li id="u07b5b07d" data-lake-index-type="0"><span class="ne-text">消费 Context 值（Consumer）：</span></li></ol><pre data-language="tsx" id="M6Asp" class="ne-codeblock language-tsx"><code>import React, { useContext } from 'react';
import { MyContext } from './MyContext';
const ChildComponent = () =&gt; {
    const { name } = useContext(MyContext);
    return &lt;div&gt;Hello, {name}!&lt;/div&gt;;
};</code></pre></details>
<details class="lake-collapse"><summary id="u0ddac56b"><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 16px">use是 React 19 引入的一个新 API，用于在组件的渲染阶段读取资源（如 Promise 或 Context）。它的主要目的是让开发者能够在渲染阶段直接处理异步数据，而不需要手动管理加载状态或错误处理。</span></summary><p id="ub4ee1625" class="ne-p"><span class="ne-text" style="font-size: 16px">use钩子的作用：</span></p><ol class="ne-ol"><li id="u9de7656d" data-lake-index-type="0"><strong><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 16px">读取 Promise 的结果</span></strong><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 16px">：</span></li><li id="u1bb8d779" data-lake-index-type="0"><strong><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 16px">读取 Context 的值(可以在条件渲染中使用)，而单独使用useContext则无法再条件渲染中获取context,会报错。</span></strong></li></ol><pre data-language="tsx" id="ObgZS" class="ne-codeblock language-tsx"><code>import React, { useContext } from 'react';
import ThemeContext from './ThemeContext';
function Heading({ children }) {
    // useContext 必须在组件的顶层调用
    const theme = useContext(ThemeContext);

    if (children == null) {
        return null;
    }

    return (
        &lt;h1 style={{ color: theme.color }}&gt;
            {children}
        &lt;/h1&gt;
    );
}

export default Heading;</code></pre></details>
<details class="lake-collapse"><summary id="uf2169953"><span class="ne-text">vite创建项目的命令：</span></summary><pre data-language="tsx" id="NtBkE" class="ne-codeblock language-tsx"><code>npm create vite@latest</code></pre><pre data-language="tsx" id="eweNp" class="ne-codeblock language-tsx"><code>npm create vite@latest my-vite-project</code></pre></details>
<details class="lake-collapse"><summary id="ub8d990f3"><span class="ne-text">水合错误，</span></summary><p id="u627171a3" class="ne-p"><code class="ne-code"><span class="ne-text" style="color: rgb(0, 0, 0); background-color: rgb(240, 240, 240)">&lt;div&gt;</span></code><span class="ne-text" style="color: rgb(51, 51, 51); font-size: 16px"> cannot be a descendant of </span><code class="ne-code"><span class="ne-text" style="color: rgb(0, 0, 0); background-color: rgb(240, 240, 240)">&lt;p&gt;</span></code><span class="ne-text" style="color: rgb(51, 51, 51); font-size: 16px">. This will cause a hydration error. </span></p><p id="u03da8317" class="ne-p"><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 16px">在 HTML 中，</span><code class="ne-code"><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); background-color: rgba(0, 0, 0, 0.03); font-size: 16px">&lt;p&gt;</span></code><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 16px"> 元素是一个段落元素，它只能包含 </span><strong><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 16px">短语内容（phrasing content）</span></strong><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 16px">，例如文本、链接（</span><code class="ne-code"><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); background-color: rgba(0, 0, 0, 0.03); font-size: 16px">&lt;a&gt;</span></code><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 16px">）、强调（</span><code class="ne-code"><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); background-color: rgba(0, 0, 0, 0.03); font-size: 16px">&lt;em&gt;</span></code><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 16px">）、强调（</span><code class="ne-code"><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); background-color: rgba(0, 0, 0, 0.03); font-size: 16px">&lt;strong&gt;</span></code><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 16px">）等。</span><code class="ne-code"><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); background-color: rgba(0, 0, 0, 0.03); font-size: 16px">&lt;p&gt;</span></code><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 16px"> 元素不能包含块级元素（block-level elements），如 </span><code class="ne-code"><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); background-color: rgba(0, 0, 0, 0.03); font-size: 16px">&lt;div&gt;</span></code><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 16px">、</span><code class="ne-code"><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); background-color: rgba(0, 0, 0, 0.03); font-size: 16px">&lt;ul&gt;</span></code><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 16px">、</span><code class="ne-code"><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); background-color: rgba(0, 0, 0, 0.03); font-size: 16px">&lt;table&gt;</span></code><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 16px"> 等。</span></p><pre data-language="tsx" id="kS9Ix" class="ne-codeblock language-tsx"><code>&lt;p&gt;
    &lt;div&gt;Some content&lt;/div&gt;
&lt;/p&gt;</code></pre><p id="udb552470" class="ne-p"><span class="ne-text" style="font-size: 16px">水合错误概念：</span></p><ol class="ne-ol"><li id="ufa6a434b" data-lake-index-type="0"><strong><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 16px">服务器渲染</span></strong><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 16px">：服务器会预先渲染组件并生成 HTML。</span></li><li id="uc5f15568" data-lake-index-type="0"><strong><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 16px">客户端水合</span></strong><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 16px">：客户端加载页面时，React 会将服务器生成的 HTML 与客户端渲染的组件进行比较，这个过程称为 </span><strong><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 16px">水合（hydration）</span></strong></li><li id="u2aaa8080" data-lake-index-type="0"><strong><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 16px">结构不匹配</span></strong><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 16px">：如果服务器生成的 HTML 和客户端渲染的结构不匹配，React 会抛出一个水合错误。</span></li></ol></details>
<details class="lake-collapse"><summary id="u2f23b83c"><span class="ne-text" style="color: rgb(51, 51, 51); font-size: 16px">给我来个useOptimistic的使用示例： 比如我要实现点赞功能， 通过promise settimeout resolve模拟接口请求， 你帮我写下代码。 我需要接口返回成功和返回失败的两种结果</span></summary><pre data-language="tsx" id="EBb5X" class="ne-codeblock language-tsx"><code>import React, { useState, useOptimistic } from 'react';
// 模拟异步接口请求
const simulateAPIRequest = (currentCount) =&gt; {
    return new Promise((resolve, reject) =&gt; {
        setTimeout(() =&gt; {
            // 模拟请求成功或失败
            const success = Math.random() &gt; 0.5; // 50% 的概率成功，50% 的概率失败
            if (success) {
                resolve(currentCount + 1); // 模拟成功，返回新的点赞数
            } else {
                reject(new Error('Failed to like')); // 模拟失败，抛出错误
            }
        }, 1000);
    });
};
function LikeButton() {
    const [likeCount, setLikeCount] = useState(0);
    const [optimisticCount, setOptimisticCount] = useOptimistic(likeCount);
    const handleLike = async () =&gt; {
        try {
            // 乐观更新点赞数
            setOptimisticCount(optimisticCount + 1);
            // 模拟异步请求
            const newCount = await simulateAPIRequest(likeCount);
            // 如果请求成功，更新实际的点赞数
            setLikeCount(newCount);
        } catch (error) {
            // 如果请求失败，恢复到原来的点赞数
            setOptimisticCount(likeCount);
            alert(`Failed to like: ${error.message}`);
        }
    };
    return (
        &lt;div&gt;
            &lt;p&gt;Likes: {optimisticCount}&lt;/p&gt;
            &lt;button onClick={handleLike}&gt;Like&lt;/button&gt;
        &lt;/div&gt;
    );
}
export default LikeButton;</code></pre></details>
<details class="lake-collapse"><summary id="u97f27234"><span class="ne-text" style="color: rgb(51, 51, 51); font-size: 16px">项目集成zustand后，如何构建和使用， devtools函数的作用。</span></summary><ul class="ne-ul"><li id="u2e18ae3f" data-lake-index-type="0"><span class="ne-text" style="color: rgb(51, 51, 51); font-size: 16px">import { devtools } from 'zustand/middleware'; 这个devTools的作用是？</span></li></ul><div data-type="success" class="ne-alert"><p id="u589f1aba" class="ne-p"><code class="ne-code"><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); background-color: rgba(0, 0, 0, 0.03); font-size: 16px">devtools</span></code><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 16px"> 是 </span><code class="ne-code"><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); background-color: rgba(0, 0, 0, 0.03); font-size: 16px">zustand</span></code><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 16px"> 提供的一个中间件，用于增强状态管理的功能，特别是为了方便开发者在开发过程中调试和检查状态。</span></p></div><ul class="ne-ul"><li id="u35c5f078" data-lake-index-type="0"><span class="ne-text" style="color: rgb(51, 51, 51); font-size: 16px">怎么在zustand中使用devtools</span></li></ul><div data-type="color1" class="ne-alert"><p id="ua325e188" class="ne-p"><span class="ne-text" style="font-size: 16px">1.安装依赖：</span><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 16px">确保已经安装了 Zustand 和 Redux DevTools 浏览器扩展；2.使用devTools中间件：</span></p></div><pre data-language="tsx" id="GkR0h" class="ne-codeblock language-tsx"><code>import { create } from 'zustand';
import { devtools } from 'zustand/middleware';
interface StoreState {
  count: number;
  increase: () =&gt; void;
  decrease: () =&gt; void;
}
const useStore = create&lt;StoreState&gt;(
  devtools(
    (set) =&gt; ({
      count: 0,
      increase: () =&gt; set((state) =&gt; ({ count: state.count + 1 })),
      decrease: () =&gt; set((state) =&gt; ({ count: state.count - 1 })),
    }),
    { name: 'my-app-store' } // 在 Redux DevTools 中显示的 Store 名称
  )
);
export default useStore;</code></pre><ul class="ne-ul"><li id="ud71d1966" data-lake-index-type="0"><span class="ne-text" style="color: rgb(51, 51, 51); font-size: 16px">enabled、name属性的作用是什么？</span></li></ul><div data-type="color5" class="ne-alert"><p id="u0fe98f87" class="ne-p"><span class="ne-text" style="font-size: 16px">enabled的作用是用来判断是否启用devTools,name是为每个store设置一个自己的名字，也是为了方便在调试的时候区分不同的store.</span></p></div><ul class="ne-ul"><li id="u013c08df" data-lake-index-type="0"><span class="ne-text" style="color: rgb(51, 51, 51); font-size: 16px">定义好store.ts后，在我的组件中如何打印userInfo的username以及修改userName？</span></li></ul><pre data-language="tsx" id="TYe7g" class="ne-codeblock language-tsx"><code>const {username,setUserName}=useUserStore()
console.log(&quot;&quot;,username)
useEffect(()=&gt;{
  setUserName(&quot;xiaomin&quot;)
},[])</code></pre><ul class="ne-ul"><li id="u3b06b4a8" data-lake-index-type="0"><span class="ne-text" style="color: rgb(51, 51, 51); font-size: 16px">const userInfo_Sample = useUserStore((state) =&gt; state.userInfo); 之前redux中的写法，这种写法还可以吗? 支持吗？（支持，而且很推荐这种写法）</span></li></ul></details>
<details class="lake-collapse"><summary id="ub84690aa"><span class="ne-text" style="color: rgb(51, 51, 51); font-size: 16px">安装react-route V7</span></summary><ul class="ne-ul"><li id="ue3bf4ad7" data-lake-index-type="0"><span class="ne-text" style="color: rgb(51, 51, 51); font-size: 16px">index: true 是什么意思？</span></li></ul><div data-type="danger" class="ne-alert"><p id="u0b1747bb" class="ne-p"><code class="ne-code"><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); background-color: rgba(0, 0, 0, 0.03); font-size: 12px">index: true</span></code><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 12px"> 是一个用于定义默认子路由的属性。它的主要作用是当父路由匹配但没有明确指定子路径时，自动渲染一个默认的子组件。</span></p><p id="ud9e29b90" class="ne-p"><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 12px">作用：</span></p><ol class="ne-ol"><li id="u3e17ea0d" data-lake-index-type="0"><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 12px">当父路由匹配的时候，但是没有其他子路由匹配，index路由就会被渲染。增加容错处理吧，感觉相当于NotFound页面的感觉；定义一个默认的子页面。注意（其不需要显示定义path），因为他会继承父路由的路径</span></li></ol></div><pre data-language="tsx" id="sfQg0" class="ne-codeblock language-tsx"><code>const router = createBrowserRouter([
  {
    path: '/settings',
    element: &lt;SettingsLayout /&gt;,
    children: [
      {
        index: true, // 默认子路由
        element: &lt;SettingsOverview /&gt;,
      },
      {
        path: 'profile',
        element: &lt;ProfilePage /&gt;,
      },
      {
        path: 'security',
        element: &lt;SecurityPage /&gt;,
      },
    ],
  },
]);</code></pre><ul class="ne-ul"><li id="u05a5f6f1" data-lake-index-type="0"><span class="ne-text" style="color: rgb(51, 51, 51); font-size: 16px">定义index: true时，path是否生效？(不生效)</span></li><li id="uf1ee8361" data-lake-index-type="0"><span class="ne-text" style="color: rgb(51, 51, 51); font-size: 16px">路由对象中。loader配置项的作用是什么? 有哪些应用场景? 支持异步写法吗，同步呢? loader写法是异步函数时，如果组件也使用懒加载，会不会导致组件已经加载完毕，然而loader异步请求的数据还没有拿到。</span></li></ul><div data-type="danger" class="ne-alert"><p id="u4db8f6f2" class="ne-p"><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 12px">loader配置项的作用：用于在路由渲染之前实现数据的预加载，主要还是为了提升用户的使用体验感吧。</span></p><p id="u2527cf30" class="ne-p"><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 12px">使用场景：数据预加载、ssr(服务器端渲染)，客户端数据处理、错误处理</span></p><p id="u0dd1365c" class="ne-p"><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 12px">支持异步写法，也支持同步写法。但是主要用于异步数据加载。</span></p><p id="u317831a2" class="ne-p"><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 12px">会导致，所以react提供了useLoaderData()钩子，用于在组件中获取loader的数据</span></p></div><pre data-language="tsx" id="U9NbI" class="ne-codeblock language-tsx"><code>import { useLoaderData } from &quot;react-router&quot;;
const App = () =&gt; {
  const data = useLoaderData();
  return &lt;div&gt;{data}&lt;/div&gt;;
};</code></pre><ul class="ne-ul"><li id="u438f704e" data-lake-index-type="0"><span class="ne-text" style="font-size: 16px">什么是布局路由？</span></li></ul><div data-type="tips" class="ne-alert"><p id="u73de4ac0" class="ne-p"><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 12px">布局路由是一种特殊的路由配置，用于为一组子路由共享相同的布局组件。它通常用于在多个页面之间复用公共的 UI 元素，例如导航栏、侧边栏或底部栏等。</span></p><p id="u8b0620e0" class="ne-p"><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 12px">应用场景：</span></p><ul class="ne-ul"><li id="ua26e7621" data-lake-index-type="0"><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 12px">共享导航栏和侧边栏</span></li><li id="ubef05400" data-lake-index-type="0"><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 12px">嵌套路由：布局路由可以用于嵌套多个子路由，每个子路由都可以继承父布局的样式和功能。</span></li><li id="u75590398" data-lake-index-type="0"><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 12px">页面分组：将具有相同页面布局的页面分组，便于管理和维护。</span></li></ul><p id="u650780cd" class="ne-p"><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 12px">实现方式：</span></p></div><pre data-language="tsx" id="yotLM" class="ne-codeblock language-tsx"><code>import React from 'react';
import { Routes, Route, Outlet } from 'react-router-dom';
// 定义布局组件
const PageLayout = () =&gt; {
  return (
    &lt;div&gt;
      &lt;h1&gt;Shared Layout&lt;/h1&gt;
      &lt;Outlet /&gt; {/* 子路由的渲染位置 */}
    &lt;/div&gt;
  );
};
// 定义子路由组件
const Privacy = () =&gt; {
  return &lt;div&gt;Privacy Policy&lt;/div&gt;;
};
const Tos = () =&gt; {
  return &lt;div&gt;Terms of Service&lt;/div&gt;;
};
// 定义路由配置
const App = () =&gt; {
  return (
    &lt;Routes&gt;
      &lt; elementRoute={&lt;PageLayout /&gt;}&gt;
        &lt;Route path=&quot;/privacy&quot; element={&lt;Privacy /&gt;} /&gt;
        &lt;Route path=&quot;/tos&quot; element={&lt;Tos /&gt;} /&gt;
      &lt;/Route&gt;
    &lt;/Routes&gt;
  );
};

export default App;</code></pre><ul class="ne-ul"><li id="u1af898ee" data-lake-index-type="0"><span class="ne-text" style="color: rgb(51, 51, 51); font-size: 16px">什么是索引路由，索引路由可以定义子节点吗？</span></li></ul><div data-type="color2" class="ne-alert"><p id="uc8597511" class="ne-p"><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 12px">索引路由（Index Route） 是一种特殊的子路由，它在父路由匹配但没有其他子路由匹配时被渲染。索引路由通常用于定义父路由的默认页面。</span></p><p id="u376fb552" class="ne-p"><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 12px">特点：</span></p><ol class="ne-ol"><li id="u35f0b1ec" data-lake-index-type="0"><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 12px">没有path，自动继承副路由的路径</span></li><li id="u7293cdd8" data-lake-index-type="0"><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 12px">默认渲染</span></li><li id="u072c489f" data-lake-index-type="0"><span class="ne-text" style="color: rgba(0, 0, 0, 0.9); font-size: 12px">嵌套支持：索引路由可以嵌套在布局路由中或其他副路由中。</span></li></ol></div><p id="u546536e5" class="ne-p"><span class="ne-text" style="font-size: 16px">写法：</span></p><pre data-language="tsx" id="DImuV" class="ne-codeblock language-tsx"><code>import { BrowserRouter, Routes, Route, Outlet } from 'react-router-dom';
const DashboardLayout = () =&gt; {
  return (
    &lt;div&gt;
      &lt;h1&gt;Dashboard Layout&lt;/h1&gt;
      &lt;Outlet /&gt; {/* 子路由的渲染位置 */}
    &lt;/div&gt;
  );
};
const DashboardHome = () =&gt; {
  return &lt;div&gt;Welcome to the Dashboard&lt;/div&gt;;
};
const Settings = () =&gt; {
  return &lt;div&gt;Settings Page&lt;/div&gt;;
};
const Profile = () =&gt; {
  return &lt;div&gt;Profile Page&lt;/div&gt;;
};
const App = () =&gt; {
  return (
    &lt;BrowserRouter&gt;
      &lt;Routes&gt;
        &lt;Route path=&quot;/dashboard&quot; element={&lt;DashboardLayout /&gt;}&gt;
          &lt;Route index element={&lt;DashboardHome /&gt;} /&gt; {/* 索引路由 */}
          &lt;Route path=&quot;settings&quot; element={&lt;Settings /&gt;} /&gt;
          &lt;Route path=&quot;profile&quot; element={&lt;Profile /&gt;} /&gt;
        &lt;/Route&gt;
      &lt;/Routes&gt;
    &lt;/BrowserRouter&gt;
  );
};</code></pre></details>


> 更新: 2025-07-08 08:36:56  
> 原文: <https://www.yuque.com/quwaidi-exykz/lv0i4k/vpgr73sn4q3mzzrh>