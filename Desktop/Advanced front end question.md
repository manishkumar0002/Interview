
# What is event loop?

The event loop is a core mechanism in programming, especially in single-threaded environments like JavaScript, that handles asynchronous operations without blocking the main thread. It continuously monitors a call stack for synchronous tasks and an event queue for async ones, dispatching queued tasks only when the stack is empty.[^1_1][^1_2]

## Core Components

- **Call Stack**: Executes synchronous code in a last-in, first-out (LIFO) order; functions are pushed on and popped off as they complete.[^1_2]
- **Event Queue (Task Queue)**: Holds callbacks from async operations like timers (`setTimeout`), network requests, or user events until ready.[^1_3][^1_2]
- **Web APIs** (in browsers/Node.js): Handle async tasks outside the JavaScript engine, pushing callbacks to the queue when done.[^1_2]


## How It Works

JavaScript runs sync code first on the call stack. Async tasks defer to the event loop, which checks: if stack is empty, it pulls the next task from the queue to the stack for execution. This enables non-blocking I/O, like handling UI clicks while fetching data.[^1_1][^1_2]

For example, `console.log('start'); setTimeout(() => console.log('timeout'), 0); console.log('end');` outputs "start", "end", then "timeout" because the timeout callback waits in the queue.[^1_2]
<span style="display:none">[^1_10][^1_4][^1_5][^1_6][^1_7][^1_8][^1_9]</span>

<div align="center">⁂</div>

[^1_1]: https://en.wikipedia.org/wiki/Event_loop

[^1_2]: https://www.educative.io/answers/what-is-an-event-loop-in-javascript

[^1_3]: https://www.builder.io/blog/visual-guide-to-nodejs-event-loop

[^1_4]: https://www.youtube.com/watch?v=WNrHrwm1wkU

[^1_5]: https://www.greatfrontend.com/questions/quiz/what-is-event-loop-what-is-the-difference-between-call-stack-and-task-queue

[^1_6]: https://www.geeksforgeeks.org/javascript/what-is-an-event-loop-in-javascript/

[^1_7]: https://www.w3schools.com/nodejs/nodejs_event_loop.asp

[^1_8]: https://www.youtube.com/watch?v=8aGhZQkoFbQ

[^1_9]: https://www.w3schools.com/nodeJs/nodejs_event_loop.asp

[^1_10]: https://www.geeksforgeeks.org/node-js/node-js-event-loop/


---

# How call stack works?

The call stack is a fundamental data structure in programming that tracks function calls using a Last-In-First-Out (LIFO) principle.[^2_1]

## Core Mechanics

Functions are pushed onto the stack as "stack frames" when called, storing details like parameters, local variables, and return addresses. The top frame executes first; once complete, it pops off, resuming the previous frame below it.[^2_2][^2_1]

This handles nested calls seamlessly—for instance, in `foo()` calling `bar()`, which calls `baz()`: the stack grows as `foo` → `bar` → `baz`, then shrinks as `baz` finishes first.[^2_3]

## Role in Execution

It ensures proper control flow in single-threaded languages like JavaScript, linking back to the event loop by clearing before async tasks dequeue. Overflow occurs if recursion exhausts allocated memory, crashing the program.[^2_4][^2_5]
<span style="display:none">[^2_10][^2_6][^2_7][^2_8][^2_9]</span>

<div align="center">⁂</div>

[^2_1]: https://developer.mozilla.org/en-US/docs/Glossary/Call_stack

[^2_2]: https://webflow.com/glossary/call-stack

[^2_3]: https://www.geeksforgeeks.org/javascript/what-is-the-call-stack-in-javascript/

[^2_4]: https://en.wikipedia.org/wiki/Call_stack

[^2_5]: https://www.freecodecamp.org/news/understanding-the-javascript-call-stack-861e41ae61d4/

[^2_6]: https://www.interviewcake.com/concept/java/call-stack

[^2_7]: https://www.geeksforgeeks.org/c/function-call-stack-in-c/

[^2_8]: https://www.reddit.com/r/javahelp/comments/gj3wks/eli5_whats_a_call_stack/

[^2_9]: https://developer.mozilla.org/ru/docs/Glossary/Call_stack

[^2_10]: https://www.youtube.com/watch?v=7z9aar_9DB8


---

# What is microtask queue?

The microtask queue is a high-priority queue in JavaScript's event loop that holds tasks like Promise `.then()` callbacks and `queueMicrotask()` calls, executed after the call stack empties but before macrotasks (e.g., `setTimeout`) from the task queue.[^3_1][^3_2]

## Priority in Event Loop

It processes **after current synchronous code** and **before rendering or macrotasks**, ensuring microtasks run first in each loop iteration—even newly added ones until the queue clears.[^3_2][^3_4]

## Key Differences

| Queue Type | Examples | Execution Timing |
| :-- | :-- | :-- |
| Microtask | Promises, MutationObserver | Post-call stack, pre-macrotask |
| Macrotask | setTimeout, events, I/O | After microtasks fully processed [^3_7][^3_3] |

For instance, `setTimeout(fn, 0)` queues as macrotask, but `Promise.resolve().then(fn)` as microtask—running sooner.[^3_10]
<span style="display:none">[^3_5][^3_6][^3_8][^3_9]</span>

<div align="center">⁂</div>

[^3_1]: https://www.codeguage.com/v1/courses/advanced-js/microtasks

[^3_2]: https://www.greatfrontend.com/questions/quiz/explain-the-concept-of-a-microtask-queue

[^3_3]: https://dev.to/devopsfundamentals/nodejs-fundamentals-microtask-queue-4l0l

[^3_4]: https://developer.mozilla.org/en-US/docs/Web/API/HTML_DOM_API/Microtask_guide

[^3_5]: https://codedamn.com/news/javascript/what-is-microtask-queue-in-javascript

[^3_6]: https://www.red-gate.com/simple-talk/development/javascript/microtask-queues-in-node-js-event-loop/

[^3_7]: https://www.linkedin.com/pulse/understanding-call-stack-microtask-queue-macrotask-javascript-jha-47euc

[^3_8]: https://developer.mozilla.org/en-US/docs/Web/API/HTML_DOM_API/Microtask_guide/In_depth

[^3_9]: https://www.geeksforgeeks.org/javascript/what-is-the-difference-between-microtask-queue-and-callback-queue-in-asynchronous-javascript/

[^3_10]: https://javascript.info/microtask-queue


---

# What is debounce and throttle?

Debounce and throttle are JavaScript techniques to limit how often resource-heavy functions (like API calls or DOM updates) execute during rapid events such as typing, scrolling, or resizing.

## Debounce Mechanics

Debounce delays function execution until a specified wait time passes without further event triggers, collapsing multiple calls into one final execution. It's ideal for search inputs, where you only want to query after the user stops typing.[^4_1][^4_2]

## Throttle Mechanics

Throttle ensures a function runs at most once per fixed interval (e.g., every 200ms), regardless of event frequency, allowing periodic execution during continuous events. It suits scrolling or mouse movement to maintain smooth performance.[^4_4][^4_1]

## Key Comparison

| Aspect | Debounce | Throttle |
| :-- | :-- | :-- |
| Execution | Once, after events stop | Regularly, at fixed intervals |
| Best For | Final actions (e.g., search) | Ongoing events (e.g., scroll) |
| Calls During Burst | None until pause | At least one per interval        [^4_1][^4_2] |

Example: For a resize handler, throttle updates every 100ms; debounce waits 100ms post-resize.[^4_5]
<span style="display:none">[^4_10][^4_3][^4_6][^4_7][^4_8][^4_9]</span>

<div align="center">⁂</div>

[^4_1]: https://www.syncfusion.com/blogs/post/javascript-debounce-vs-throttle

[^4_2]: https://stackoverflow.com/questions/25991367/difference-between-throttling-and-debouncing-a-function

[^4_3]: https://www.reddit.com/r/reactjs/comments/roy2om/debounce_vs_throttle/

[^4_4]: https://dev.to/nilebits/javascript-performance-optimization-debounce-vs-throttle-explained-5768

[^4_5]: https://www.geeksforgeeks.org/javascript/difference-between-debouncing-and-throttling/

[^4_6]: https://www.greatfrontend.com/questions/quiz/explain-the-concept-of-debouncing-and-throttling

[^4_7]: https://kettanaito.com/blog/debounce-vs-throttle

[^4_8]: https://stackoverflow.com/questions/30222791/throttle-debounce-functions

[^4_9]: https://www.youtube.com/watch?v=cjIswDCKgu0

[^4_10]: https://www.reddit.com/r/javascript/comments/eelzed/debounce_vs_throttle_definitive_visual_guide/


---

# What is shallow copy vs deep copy?

Shallow copy and deep copy are techniques in JavaScript for duplicating objects or arrays, differing in how they handle nested structures.[^5_1]

## Shallow Copy

A shallow copy creates a new object with copied top-level properties, but nested objects/arrays retain shared references to the originals. Modifying a nested property in the copy affects the source.[^5_2][^5_1]

Example: `{ ...obj }` or `Object.assign({}, obj)` produces a shallow copy.[^5_1]

## Deep Copy

A deep copy recursively duplicates all levels, creating independent objects even for nested ones, so changes to the copy don't impact the source.[^5_3]

Common methods include `JSON.parse(JSON.stringify(obj))` (limited, skips functions/dates) or `structuredClone(obj)` for modern browsers.[^5_1]

## Key Differences

| Aspect | Shallow Copy | Deep Copy |
| :-- | :-- | :-- |
| Nesting | Shares references | Fully independent |
| Performance | Faster, less memory | Slower, more memory |
| Use Case | Simple/flattened data | Complex nested structures |

<span style="display:none">[^5_10][^5_5][^5_6][^5_7][^5_8][^5_9]</span>

<div align="center">⁂</div>

[^5_1]: https://www.greatfrontend.com/questions/quiz/explain-the-difference-between-shallow-copy-and-deep-copy

[^5_2]: https://www.freecodecamp.org/news/copying-stuff-in-javascript-how-to-differentiate-between-deep-and-shallow-copies-b6d8c1ef09cd/

[^5_3]: https://mayallo.com/deep-copy-vs-shallow-copy-in-javascript/

[^5_4]: https://dev.to/hkp22/javascript-shallow-copy-vs-deep-copy-examples-and-best-practices-3k0a

[^5_5]: https://www.reddit.com/r/learnjavascript/comments/1be1oav/deep_vs_shallow_copy/

[^5_6]: https://www.mindsetconsulting.com/javascript-shallow-copy-deep-copy/

[^5_7]: https://dev.to/jahid6597/beyond-cloning-sculpting-javascript-mirrors-with-the-art-of-shallow-copy-and-the-depth-of-deep-copy-3p64

[^5_8]: https://www.geeksforgeeks.org/javascript/what-is-shallow-copy-and-deep-copy-in-javascript/

[^5_9]: https://makimo.com/blog/shallow-and-deep-copies-in-javascript/

[^5_10]: https://developer.mozilla.org/en-US/docs/Glossary/Shallow_copy


---

# What is spread operator?

The spread operator (`...`) in JavaScript expands iterables like arrays, strings, or objects into individual elements or properties, simplifying tasks like copying, merging, or passing arguments.[^6_1][^6_4]

## Array Usage

It creates shallow copies or merges arrays: `const copy = [...original];` or `const merged = [...arr1, ...arr2];`. This unpacks elements, e.g., `Math.max(...[1, 5, 3])` returns 5.[^6_2][^6_1]

## Object Usage

For objects, `{...obj}` copies top-level properties shallowly: `const merged = {...obj1, ...obj2};` overrides duplicates from right to left.[^6_4][^6_1]

## Function Arguments

Pass arrays as separate params: `func(...args)` expands `[1,2,3]` into `func(1,2,3)`. Note: it's shallow, so nested objects share references.[^6_4]
<span style="display:none">[^6_10][^6_3][^6_5][^6_6][^6_7][^6_8][^6_9]</span>

<div align="center">⁂</div>

[^6_1]: https://www.programiz.com/javascript/spread-operator

[^6_2]: https://www.geeksforgeeks.org/javascript/javascript-spread-operator/

[^6_3]: https://www.geeksforgeeks.org/javascript/javascript-spread-syntax/

[^6_4]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax

[^6_5]: https://www.youtube.com/watch?v=RuDdltsfaVc

[^6_6]: https://www.freecodecamp.org/news/javascript-spread-and-rest-operators/

[^6_7]: https://www.w3schools.com/howto/howto_js_spread_operator.asp

[^6_8]: https://www.w3schools.com/react/react_es6_spread.asp

[^6_9]: https://dev.to/marinamosti/understanding-the-spread-operator-in-javascript-485j

[^6_10]: https://www.digitalocean.com/community/tutorials/js-spread-operator


---

# What is destructuring?

Destructuring in JavaScript is an ES6 feature that lets you unpack values from arrays or properties from objects into distinct variables in a concise way.[^7_1][^7_2]

## Array Destructuring

Extract elements by position: `const [a, b] = [1, 2];` assigns `a=1`, `b=2`. Skip items with commas (`const [a,,c] = [1,2,3];`), use rest (`const [a, ...rest] = [1,2,3,4];`), or set defaults (`const [a=0, b] = [^7_5];`).[^7_2][^7_1]

## Object Destructuring

Pull properties by name: `const {name, age} = {name: 'Alice', age: 30};`. Rename with aliases (`const {name: fullName} = obj;`), nest (`const {user: {name}} = data;`), or add defaults (`const {name='Unknown'} = obj;`).[^7_4][^7_2]

## Common Uses

Works in function params (`function greet({name}) { ... }`), loops (`for(const [key,value] of map)`), and enhances readability over manual assignments.[^7_6][^7_2]
<span style="display:none">[^7_10][^7_3][^7_7][^7_8][^7_9]</span>

<div align="center">⁂</div>

[^7_1]: https://www.w3schools.com/js/js_destructuring.asp

[^7_2]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring

[^7_3]: https://www.geeksforgeeks.org/javascript/destructuring-assignment-in-javascript/

[^7_4]: https://www.geekster.in/articles/destructuring-in-javascript/

[^7_5]: https://dev.to/siddharthshyniben/the-complete-guide-to-destructuring-in-javascript-4b5g

[^7_6]: https://dmitripavlutin.com/javascript-object-destructuring/

[^7_7]: https://mimo.org/glossary/javascript/destructuring

[^7_8]: https://www.freecodecamp.org/news/javascript-object-destructuring-spread-operator-rest-parameter/

[^7_9]: https://javascript.info/destructuring-assignment

[^7_10]: https://www.freecodecamp.org/news/destructuring-in-javascript/


---

# What is optional chaining?

Optional chaining (`?.`) is a JavaScript operator introduced in ES2020 that safely accesses nested object properties, array elements, or function calls, returning `undefined` instead of throwing an error if any part of the chain is `null` or `undefined`.[^8_1]

## Syntax Forms

- Property access: `obj?.prop` checks if `obj` exists before accessing `prop`.
- Dynamic/bracket notation: `obj?.[expr]` or `arr?.[index]` for computed keys or arrays.
- Method calls: `func?.()` or `obj?.method?()` executes only if callable.[^8_2][^8_1]


## Practical Example

Without it: `user.address.street` crashes if `user` or `address` is missing. With chaining: `user?.address?.street` safely returns `undefined`.[^8_1]

```
const user = { name: 'Alice', profile: { age: 30 } };
console.log(user?.profile?.age); // 30
console.log(user?.contact?.email); // undefined (no error)
```


## Benefits Over Alternatives

Replaces verbose checks like `user && user.address && user.address.street`, simplifying nested access in APIs, React props, or config objects.[^8_8][^8_2]
<span style="display:none">[^8_10][^8_3][^8_4][^8_5][^8_6][^8_7][^8_9]</span>

<div align="center">⁂</div>

[^8_1]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Optional_chaining

[^8_2]: https://www.freecodecamp.org/news/javascript-optional-chaining/

[^8_3]: https://www.codecademy.com/resources/docs/javascript/optional-chaining

[^8_4]: https://mimo.org/glossary/javascript/optional-chaining

[^8_5]: https://stackoverflow.com/questions/59623674/how-can-i-use-optional-chaining-with-arrays-and-functions

[^8_6]: https://www.freecodecamp.org/news/optional-chaining-javascript/

[^8_7]: https://www.geeksforgeeks.org/javascript/javascript-optional-chaining/

[^8_8]: https://javascript.info/optional-chaining

[^8_9]: https://developer.mozilla.org/de/docs/Web/JavaScript/Reference/Operators/Optional_chaining

[^8_10]: https://dev.to/codeofrelevancy/javascripts-optional-chaining-operator-3pfn


---

# What is reconciliation?

Reconciliation is React's process of efficiently updating the DOM by comparing (diffing) the new virtual DOM tree against the previous one after state or prop changes.

## Core Phases

React first renders components to create a new virtual DOM, then applies a diffing algorithm to identify minimal changes, and finally commits those updates to the real DOM. This avoids full re-renders, boosting performance.[^9_1][^9_2]

## Diffing Rules

- **Element types differ**: Replace the entire subtree.
- **Same type**: Update attributes/props; recurse into children.
- **Keys in lists**: Track items efficiently for adds/removes/reorders.[^9_2][^9_4]


## Optimization Tips

Use stable `key` props in lists to minimize unnecessary re-renders. Reconciliation heuristics assume trees are stable, so avoid inline functions/conditionals that break assumptions.[^9_2]
<span style="display:none">[^9_10][^9_3][^9_5][^9_6][^9_7][^9_8][^9_9]</span>

<div align="center">⁂</div>

[^9_1]: https://www.geeksforgeeks.org/reactjs/reactjs-reconciliation/

[^9_2]: https://namastedev.com/blog/react-reconciliation-algorithm-explained-7/

[^9_3]: https://shiftasia.com/community/reactjs-reconciliation-how-it-works/

[^9_4]: https://www.greatfrontend.com/questions/quiz/what-is-reconciliation-in-react

[^9_5]: https://www.dhiwise.com/post/a-deep-dive-into-react-reconciliation-algorithm

[^9_6]: https://cekrem.github.io/posts/react-reconciliation-deep-dive/

[^9_7]: https://stevekinney.com/courses/react-performance/understanding-reconciliation-react-19

[^9_8]: https://legacy.reactjs.org/docs/reconciliation.html

[^9_9]: https://www.developerway.com/posts/reconciliation-in-react

[^9_10]: https://www.youtube.com/watch?v=xC8mmhiZ2P8


---

# How React re-renders?

React components re-render when their state changes (via `setState` or `useState`), props are updated by a parent, or a parent component re-renders, triggering reconciliation to diff virtual DOM trees and apply minimal DOM updates.[^10_1][^10_3]

## Trigger Mechanisms

- **State updates**: Calling `setCount(count + 1)` queues a re-render after the current execution, re-running the component function to generate new JSX.[^10_1]
- **Props changes**: Children re-render if parent passes new values, even if identical (use `React.memo` to optimize).[^10_3]
- **Parent re-renders**: By default, all children re-render unless memoized; hooks like `useEffect` run post-render.[^10_3][^10_1]


## Render Process

1. React creates a new virtual DOM from the updated component.
2. Diffs against previous tree via reconciliation.
3. Commits only changed real DOM nodes (e.g., text updates, not full replacement).[^10_1]

For example, updating state in a parent cascades to children; `useCallback`/`useMemo` prevent unnecessary re-runs.[^10_3]
<span style="display:none">[^10_10][^10_2][^10_4][^10_5][^10_6][^10_7][^10_8][^10_9]</span>

<div align="center">⁂</div>

[^10_1]: https://www.geeksforgeeks.org/reactjs/re-rendering-components-in-reactjs/

[^10_2]: https://blog.logrocket.com/how-when-to-force-react-component-re-render/

[^10_3]: https://stackoverflow.com/questions/64500491/how-does-react-js-re-render-components

[^10_4]: https://react.dev/learn/render-and-commit

[^10_5]: https://www.joshwcomeau.com/react/why-react-re-renders/

[^10_6]: https://www.developerway.com/posts/react-re-renders-guide

[^10_7]: https://stackoverflow.com/questions/41004631/trace-why-a-react-component-is-re-rendering

[^10_8]: https://www.youtube.com/watch?v=u_hjMGfYVGs

[^10_9]: https://dev.to/adevnadia/react-re-renders-guide-why-components-re-render-4ml

[^10_10]: https://www.reddit.com/r/reactjs/comments/18oax23/confused_with_react_rerender/


---

# What causes re-render?

React components re-render primarily due to changes in their state, props, or parent component updates, as these signal React to diff the virtual DOM and apply changes efficiently.

## Main Triggers

- **State changes**: Calling `setState()` (class) or `useState`/`useReducer` setter functions queues a re-render by marking the component "dirty."[^11_4]
- **Props updates**: New or changed props from a parent cause the child to re-evaluate its render function.[^11_4]
- **Parent re-renders**: Children re-render by default when parents do, unless optimized with `React.memo`, `useMemo`, or `shouldComponentUpdate`.[^11_11]


## Other Causes

- **Context value changes**: Components using `useContext` re-render if the context updates.
- **Hooks like `useEffect`**: Side effects can indirectly trigger via state/props.
- **Force re-render**: Rare cases use `forceUpdate()` (class), `useReducer` hacks, or key prop changes, but avoid as they bypass optimizations.[^11_3]

| Trigger Type | Example | Optimization |
| :-- | :-- | :-- |
| State | `setCount(count + 1)` | Batch updates in React 18+ |
| Props | `<Child data={newData} />` | `React.memo` |
| Parent | Parent state change | `useCallback` for stable props |

<span style="display:none">[^11_1][^11_10][^11_2][^11_5][^11_6][^11_7][^11_8][^11_9]</span>

<div align="center">⁂</div>

[^11_1]: https://coreui.io/blog/how-to-force-a-react-component-to-re-render/

[^11_2]: https://jbinternational.co.uk/article/view/3585

[^11_3]: https://blog.logrocket.com/how-when-to-force-react-component-re-render/

[^11_4]: https://www.geeksforgeeks.org/reactjs/re-rendering-components-in-reactjs/

[^11_5]: https://www.joshwcomeau.com/react/why-react-re-renders/

[^11_6]: https://www.developerway.com/posts/react-re-renders-guide

[^11_7]: https://stackoverflow.com/questions/30626030/can-you-force-a-react-component-to-rerender-without-calling-setstate

[^11_8]: https://stackoverflow.com/questions/41004631/trace-why-a-react-component-is-re-rendering

[^11_9]: https://react.dev/learn/render-and-commit

[^11_10]: https://dev.to/adevnadia/react-re-renders-guide-why-components-re-render-4ml

[^11_11]: https://stackoverflow.com/questions/64500491/how-does-react-js-re-render-components


---

# What is memo?

`React.memo` is a higher-order component that memoizes functional components to prevent unnecessary re-renders when props remain unchanged.[^12_1]

## How It Works

```
It performs a shallow comparison of previous and current props; if identical, React skips rendering and reuses the last result. Wrap components like `const MyComp = React.memo(({data}) => <div>{data}</div>);`.[^12_2][^12_1]
```


## When to Use

Ideal for expensive child components (lists, charts) whose props don't change often, even if parents re-render. Combine with `useCallback`/`useMemo` for stable prop references to maximize effectiveness.[^12_1]

## Comparison Table

| Feature | React.memo | useMemo/useCallback |
| :-- | :-- | :-- |
| Target | Entire component | Values/functions |
| Comparison | Shallow props | Custom/dependency array |
| Use Case | Prevent child re-renders | Stabilize inline props |

<span style="display:none">[^12_10][^12_3][^12_4][^12_5][^12_6][^12_8][^12_9]</span>

<div align="center">⁂</div>

[^12_1]: https://www.geeksforgeeks.org/reactjs/how-does-react-memo-optimize-functional-components-in-react/

[^12_2]: https://dev.to/engrsakib/reactmemo-optimizing-performance-in-react-applications-5hmf

[^12_3]: https://www.contentful.com/blog/react-memo-improve-performance/

[^12_4]: https://strapi.io/blog/react-memo-optimize-functional-components-guide

[^12_5]: https://react.dev/reference/react/memo

[^12_6]: https://refine.dev/blog/react-memo-guide/

[^12_7]: https://blog.logrocket.com/react-memo/

[^12_8]: https://www.geeksforgeeks.org/reactjs/what-is-memoization-in-react/

[^12_9]: https://dev.to/ichintansoni/using-reactmemo-to-optimize-your-react-applications-577a

[^12_10]: https://hygraph.com/blog/react-memo


---

# What is useMemo?

`useMemo` is a React Hook that memoizes expensive computations, caching a value and recomputing it only when specified dependencies change between renders.[^13_1][^13_3]

## Syntax and Usage

```jsx
const memoizedValue = useMemo(() => expensiveOperation(a, b), [a, b]);
```

The callback runs on mount and whenever dependencies update; otherwise, it returns the cached result, skipping recalculation.[^13_3][^13_9]

## Common Scenarios

- **Derived state**: Filter/sort large arrays, e.g., `const visibleTodos = useMemo(() => todos.filter(t => !t.completed), [todos]);`.
- **Prevent child re-renders**: Stable object references for `React.memo` children.
- **Heavy math**: Complex formulas based on props/state.[^13_2][^13_3]

Avoid overusing—premature optimization adds overhead; profile first. Unlike `useCallback` (memoizes functions), `useMemo` targets values.[^13_4][^13_1]
<span style="display:none">[^13_10][^13_5][^13_6][^13_7][^13_8]</span>

<div align="center">⁂</div>

[^13_1]: https://maxrozen.com/understanding-when-use-usememo

[^13_2]: https://www.telerik.com/blogs/learn-how-usememo-hook-once-all

[^13_3]: https://www.geeksforgeeks.org/reactjs/react-js-usememo-hook/

[^13_4]: https://refine.dev/blog/react-usememo/

[^13_5]: https://www.freecodecamp.org/news/how-to-work-with-usememo-in-react/

[^13_6]: https://www.developerway.com/posts/how-to-use-memo-use-callback

[^13_7]: https://www.joshwcomeau.com/react/usememo-and-usecallback/

[^13_8]: https://www.w3schools.com/react/react_usememo.asp

[^13_9]: https://react.dev/reference/react/useMemo

[^13_10]: https://www.reddit.com/r/reactjs/comments/1ap3gdx/what_is_the_purpose_of_usememoing_a_whole/


---

# What is useCallback?

`useCallback` is a React Hook that memoizes callback functions, returning the same function instance across re-renders unless its dependencies change, to optimize child component performance.

## Syntax and Usage

```jsx
const memoizedCallback = useCallback(() => {
  doSomething(a, b);
}, [a, b]);
```

It caches the function based on the dependency array; empty `[]` means it persists forever (rarely useful).[^14_1][^14_7]

## Primary Purpose

Prevents new function objects on every render, which would trigger child re-renders when passed as props to `React.memo`-wrapped components. Stabilizes references for event handlers, `useEffect` deps, or API calls.[^14_3][^14_1]

## useMemo vs useCallback

| Hook | Memoizes | Use Case Example |
| :-- | :-- | :-- |
| `useMemo` | Values | `const filtered = useMemo(() => arr.filter(...), [arr]);` |
| `useCallback` | Functions | `const handleClick = useCallback(() => ..., [deps]);` |

Example: `const handleSave = useCallback((id) => update(id), [userId]);` passed to a memoized child avoids unnecessary child renders.[^14_2]
<span style="display:none">[^14_10][^14_4][^14_5][^14_6][^14_8][^14_9]</span>

<div align="center">⁂</div>

[^14_1]: https://www.dhiwise.com/post/understanding-the-usecallback-hook-a-guide-to-react

[^14_2]: https://semaphore.io/blog/react-usecallback-hook

[^14_3]: https://www.geeksforgeeks.org/reactjs/react-js-usecallback-hook/

[^14_4]: https://stackoverflow.com/questions/71265042/what-is-usecallback-in-react-and-when-to-use-it

[^14_5]: https://refine.dev/blog/react-usecallback-guide/

[^14_6]: https://www.telerik.com/blogs/react-basics-how-use-react-usecallback-effectively

[^14_7]: https://react.dev/reference/react/useCallback

[^14_8]: https://www.w3schools.com/react/react_usecallback.asp

[^14_9]: https://coderpad.io/blog/development/a-guide-to-using-reacts-usecallback-hook/

[^14_10]: https://www.youtube.com/watch?v=MxIPQZ64x0I


---

# Controlled vs uncontrolled components?

Controlled and uncontrolled components in React are two approaches to managing form input state.

## Controlled Components

React state drives the input value via `value` prop, with `onChange` updating state on every keystroke. This makes React the "single source of truth," enabling real-time validation, formatting, or conditional logic.

```jsx
const [value, setValue] = useState('');
<input value={value} onChange={(e) => setValue(e.target.value)} />
```


## Uncontrolled Components

DOM handles state internally; React accesses via `ref` only when needed (e.g., submit). Simpler for basic forms, fewer re-renders, but limited real-time control.[^15_1][^15_3]

```jsx
const ref = useRef();
<input ref={ref} defaultValue="" />
const handleSubmit = () => ref.current.value;
```


## Comparison

| Aspect | Controlled | Uncontrolled |
| :-- | :-- | :-- |
| State | React state | DOM |
| Re-renders | Every change | Minimal |
| Validation | Real-time | On submit |
| Use Case | Complex forms | Simple inputs |

<span style="display:none">[^15_10][^15_2][^15_4][^15_6][^15_7][^15_8][^15_9]</span>

<div align="center">⁂</div>

[^15_1]: https://www.scaler.com/topics/react/controlled-and-uncontrolled-components-in-react/

[^15_2]: https://dev.to/ale3oula/navigating-reacts-controlled-vs-uncontrolled-components-48kb

[^15_3]: https://www.greatfrontend.com/questions/quiz/what-is-the-difference-between-controlled-and-uncontrolled-react-components

[^15_4]: https://www.geeksforgeeks.org/reactjs/controlled-vs-uncontrolled-components-in-reactjs/

[^15_5]: https://dev.to/maximlogunov/controlled-vs-uncontrolled-components-in-react-a-practical-guide-k79

[^15_6]: https://stackoverflow.com/questions/42522515/what-are-the-differences-between-controlled-and-uncontrolled-components-in-react/42522792

[^15_7]: https://pieces.app/blog/controlled-vs-uncontrolled-components-in-react

[^15_8]: https://www.freecodecamp.org/news/what-are-controlled-and-uncontrolled-components-in-react/

[^15_9]: https://legacy.reactjs.org/docs/uncontrolled-components.html

[^15_10]: https://www.reddit.com/r/reactjs/comments/1e4jj3n/controlled_or_uncontrolled_form_elements/


---

# Lifting state up?

Lifting state up is a React pattern where shared state is moved from child components to their nearest common parent (ancestor) to enable synchronization across siblings.

## Why It's Needed

When two sibling components need the same data (e.g., Celsius/Fahrenheit converters sharing temperature), separate local state causes drift. Lifting to the parent makes it the "single source of truth," passing state and setters as props.[^16_1]

## How to Implement

1. Identify shared state needed by multiple children.
2. Move `useState` to the common parent.
3. Pass state and update functions down via props.

Example:

```jsx
// Parent with lifted state
function Parent() {
  const [temp, setTemp] = useState(0);
  return (
    <div>
      <CelsiusInput temp={temp} onTempChange={setTemp} />
      <FahrenheitInput temp={temp} onTempChange={setTemp} />
    </div>
  );
}
```


## Benefits

- Ensures all components reflect identical data.
- Enables data flow between non-hierarchical siblings.
- Precursor pattern for Context API when lifting too far up the tree.[^16_2][^16_1]
<span style="display:none">[^16_10][^16_3][^16_4][^16_5][^16_6][^16_7][^16_8][^16_9]</span>

<div align="center">⁂</div>

[^16_1]: https://www.geeksforgeeks.org/reactjs/lifting-state-up-in-reactjs/

[^16_2]: https://www.dhiwise.com/post/understanding-react-a-comprehensive-guide-to-lifting-state-up

[^16_3]: https://legacy.reactjs.org/docs/lifting-state-up.html

[^16_4]: https://www.freecodecamp.org/news/what-is-lifting-state-up-in-react/

[^16_5]: https://react.dev/learn/sharing-state-between-components

[^16_6]: https://codeskiller.codingblocks.com/library/articles/lifting-state-up-in-react-js

[^16_7]: https://www.geeksforgeeks.org/reactjs/explain-the-concept-of-lifting-up-in-react-hooks/

[^16_8]: https://www.youtube.com/watch?v=nqxmnMIa3os

[^16_9]: https://www.youtube.com/watch?v=ZluNj0-NpNI

[^16_10]: https://www.reddit.com/r/react/comments/yp4kuu/newbie_here_dont_understand_why_lifting_up_state/


---

# Context API?

React Context API is a built-in state-sharing mechanism that lets you pass data through the component tree without prop drilling to deeply nested children.[^17_1]

## Core Components

- **createContext**: Creates a context object with a default value: `const ThemeContext = createContext('light');`.
- **Provider**: Wraps components and supplies the current value: `<ThemeContext.Provider value={theme}>...</ThemeContext.Provider>`. Children re-render when value changes.[^17_3][^17_1]
- **Consumer/useContext**: Accesses value via `useContext(ThemeContext)` in functional components or `<Context.Consumer>` in class ones.[^17_3]


## When to Use

Perfect for global data like themes, user auth, or locale that multiple unrelated components need. Avoid for frequent updates (causes widespread re-renders); use with `useMemo` for stable values or combine with reducers for complex state.[^17_2][^17_1]

Example:

```jsx
const UserContext = createContext();
function App() {
  const [user, setUser] = useState(null);
  return (
    <UserContext.Provider value={{user, setUser}}>
      <Profile />
    </UserContext.Provider>
  );
}
function Profile() {
  const {user} = useContext(UserContext); // No props needed
}
```

<span style="display:none">[^17_10][^17_4][^17_5][^17_6][^17_7][^17_8][^17_9]</span>

<div align="center">⁂</div>

[^17_1]: https://www.geeksforgeeks.org/reactjs/explain-new-context-api-in-react/

[^17_2]: https://learn.react-js.dev/advanced-concepts/context-api

[^17_3]: https://www.freecodecamp.org/news/react-context-api-tutorial-examples/

[^17_4]: https://www.loginradius.com/blog/engineering/react-context-api

[^17_5]: https://www.geeksforgeeks.org/reactjs/what-is-the-react-context-api/

[^17_6]: https://www.freecodecamp.org/news/react-context-api-explained-with-examples/

[^17_7]: https://legacy.reactjs.org/docs/context.html

[^17_8]: https://www.youtube.com/watch?v=3yrMcx02jXs

[^17_9]: https://www.youtube.com/watch?v=t9WmZFnE6Hg

[^17_10]: https://react.dev/learn/passing-data-deeply-with-context


---

# How routing works (React Router)?

React Router enables client-side routing in single-page React apps (SPAs) by syncing URL changes with component rendering, without full page reloads.[^18_1][^18_2]

## Core Setup

```
Wrap your app in `<BrowserRouter>` (uses HTML5 History API) or `<HashRouter>` (URL hash-based). Define routes inside `<Routes>` using `<Route path="..." element={<Component />} />`, matching current URL to render components.[^18_2][^18_1]
```

Example:

```jsx
import { BrowserRouter, Routes, Route, Link } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <nav><Link to="/">Home</Link> <Link to="/about">About</Link></nav>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </BrowserRouter>
  );
}
```


## Key Features

```
- **Navigation**: `<Link to="path">` or `useNavigate()` hook for programmatic jumps (e.g., `navigate('/about')`). `<NavLink>` adds `active` styling.[^18_1]
```

- **Dynamic Routes**: `path="/users/:id"` captures params via `useParams()` (e.g., `{id}` object).[^18_1]

```
- **Nested Routes**: Child `<Routes>` inside layouts use `<Outlet />` to render sub-routes.[^18_1]
```


## How It Works

On URL change (browser nav or links), React Router diffs location, matches first `<Route>`, renders its `element`, and updates History API. Falls back to 404 for unmatched paths.[^18_2]
<span style="display:none">[^18_10][^18_3][^18_4][^18_5][^18_6][^18_7][^18_8][^18_9]</span>

<div align="center">⁂</div>

[^18_1]: https://www.geeksforgeeks.org/reactjs/reactjs-router/

[^18_2]: https://www.w3schools.com/react/react_router.asp

[^18_3]: https://www.youtube.com/watch?v=kXlDTBW-eQ4

[^18_4]: https://ui.dev/react-router-tutorial

[^18_5]: https://www.freecodecamp.org/news/a-complete-beginners-guide-to-react-router-include-router-hooks/

[^18_6]: https://www.youtube.com/watch?v=oTIJunBa6MA

[^18_7]: https://www.youtube.com/watch?v=943D7U74_sQ

[^18_8]: https://www.sitepoint.com/react-router-complete-guide/

[^18_9]: https://reactrouter.com/tutorials/quickstart

[^18_10]: https://reactrouter.com


---

# How you handle form validation?

React form validation ensures user inputs meet requirements before submission, using controlled components for real-time feedback and error handling.

## Manual Validation (useState)

Track form values and errors in state. Validate on `onChange` (real-time) or `onBlur` (field exit), updating error state with custom rules (regex, length checks).

```jsx
const [formData, setFormData] = useState({email: '', password: ''});
const [errors, setErrors] = useState({});

const validateEmail = (email) => /\S+@\S+\.\S+/.test(email);

const handleChange = (field, value) => {
  setFormData({...formData, [field]: value});
  if (errors[field]) setErrors({...errors, [field]: ''}); // Clear error
};

const handleSubmit = (e) => {
  e.preventDefault();
  const newErrors = {};
  if (!validateEmail(formData.email)) newErrors.email = 'Invalid email';
  if (formData.password.length < 8) newErrors.password = 'Min 8 chars';
  setErrors(newErrors);
  if (Object.keys(newErrors).length === 0) {
    // Submit
  }
};
```

Display: `{errors.email && <span className="error">{errors.email}</span>}`[^19_1]

## Popular Libraries

| Library | Strengths | When to Use |
| :-- | :-- | :-- |
| **React Hook Form** | Minimal re-renders, schema validation (Zod/Yup) | Performance-critical forms |
| **Formik** | Batteries-included, built-in validation | Complex forms quickly |
| **Zod** | Type-safe schema validation | TypeScript projects |

## Best Practices

- **Real-time feedback**: Validate on input for UX, disable submit if invalid.
- **Clear messages**: Specific errors like "Email must include '@'" vs "Invalid input".
- **Visual cues**: Red borders, icons for errors; green for success.
- **Accessibility**: `aria-invalid`, `aria-describedby` for screen readers.
- **Server-side**: Always validate backend too—client is trustless.[^19_2][^19_4]
<span style="display:none">[^19_10][^19_3][^19_5][^19_6][^19_7][^19_8][^19_9]</span>

<div align="center">⁂</div>

[^19_1]: https://dev.to/obere4u/how-to-perform-form-validation-in-react-3kgi

[^19_2]: https://formspree.io/blog/react-form-validation/

[^19_3]: https://www.youtube.com/watch?v=wqzv-CzJy5k

[^19_4]: https://www.dhiwise.com/post/react-form-validation-best-practices-with-tips-and-tricks

[^19_5]: https://4geeks.com/lesson/form-validation

[^19_6]: https://daily.dev/blog/form-on-react-best-practices

[^19_7]: https://www.reddit.com/r/reactjs/comments/14blis7/what_is_the_best_and_efficient_way_to_validate/

[^19_8]: https://www.freecodecamp.org/news/how-to-validate-forms-in-react/

[^19_9]: https://www.reddit.com/r/reactnative/comments/17ltppg/best_practices_for_input_or_form_validations_in/

[^19_10]: https://dev.to/ajones_codes/a-better-guide-to-forms-in-react-47f0


---

# How to optimize React performance?

React performance optimization focuses on minimizing re-renders, reducing bundle size, and avoiding expensive computations to ensure smooth user experience.

## Prevent Unnecessary Re-renders

- **Memoize components**: Wrap with `React.memo` to skip renders when props are unchanged (shallow comparison).[^20_1]
- **Stabilize callbacks**: Use `useCallback` for functions and `useMemo` for objects/arrays passed as props to memoized children.
- **Virtualize lists**: `react-window` or `react-virtualized` for large lists—render only visible items.


## Bundle and Loading

- **Code splitting**: `React.lazy()` + `Suspense` for route-based or on-demand components: `const LazyComp = lazy(() => import('./Comp'));`.[^20_1]
- **Avoid large bundles**: Analyze with webpack-bundle-analyzer; split heavy dependencies.


## Efficient State and Rendering

| Technique | Use Case | Example |
| :-- | :-- | :-- |
| `useMemo` | Expensive calculations | `const sum = useMemo(() => arr.reduce((a,b)=>a+b), [arr]);` |
| `useCallback` | Event handlers to children | `const handleClick = useCallback(fn, [deps]);` |
| `shouldComponentUpdate/PureComponent` | Class components | Auto shallow prop/state checks |

## Other Best Practices

- **Keys in lists**: Stable, unique `key={item.id}` prevents full list re-mounts.
- **Avoid anonymous functions**: `{items.map(item => <Comp key={item.id} />)}`—define outside or memoize.
- **Profile first**: React DevTools Profiler tab identifies bottlenecks before optimizing.[^20_1]
<span style="display:none">[^20_10][^20_2][^20_3][^20_4][^20_5][^20_6][^20_7][^20_8][^20_9]</span>

<div align="center">⁂</div>

[^20_1]: https://dev.to/ra1nbow1/supercharging-react-performance-best-tips-and-tools-4ff0

[^20_2]: https://nareshit.com/blogs/react-performance-optimization-techniques

[^20_3]: https://supertokens.com/blog/5-tips-for-optimizing-your-react-apps-performance

[^20_4]: https://www.simform.com/blog/react-performance/

[^20_5]: https://legacy.reactjs.org/docs/optimizing-performance.html

[^20_6]: https://www.reddit.com/r/reactjs/comments/1f6abzy/performance_optimization_strategies_for/

[^20_7]: https://www.codementor.io/blog/react-optimization-5wiwjnf9hj

[^20_8]: https://www.freecodecamp.org/news/react-performance-optimization-techniques/

[^20_9]: https://blog.sentry.io/react-js-performance-guide/

[^20_10]: https://www.youtube.com/watch?v=LuQ8VWh6eYE


---

# How to prevent unnecessary re-renders?

Preventing unnecessary re-renders in React improves performance by skipping redundant component executions when props/state haven't meaningfully changed.[^21_1]

## Memoize Components

Wrap functional components with `React.memo` for shallow prop comparison—skips render if props match previous ones. Use `PureComponent` for class components (auto `shouldComponentUpdate`).

```jsx
const Child = React.memo(({value}) => {
  console.log('Child render');
  return <div>{value}</div>;
});
```


## Stabilize Props

New objects/functions on each render break memoization. Fix with hooks:

- `useCallback(fn, [deps])` for stable callbacks: `const handleClick = useCallback(() => doSomething(id), [id]);`
- `useMemo(value, [deps])` for objects/arrays: `const filteredData = useMemo(() => data.filter(...), [data]);`[^21_2][^21_1]


## List Optimization

Use stable `key` props in arrays: `items.map(item => <Item key={item.id} />)` prevents full re-mounts during reorder/add/remove.[^21_1]

## Complete Strategy

| Technique | Targets | When to Use |
| :-- | :-- | :-- |
| `React.memo` | Entire components | Expensive children with stable props |
| `useCallback` | Functions | Callbacks to memoized children |
| `useMemo` | Values/Computations | Derived state, expensive calcs |
| Stable keys | Lists | Dynamic arrays |

Profile with React DevTools first—optimize measured bottlenecks, not guesses.[^21_1]
<span style="display:none">[^21_10][^21_3][^21_4][^21_5][^21_6][^21_7][^21_8][^21_9]</span>

<div align="center">⁂</div>

[^21_1]: https://www.syncfusion.com/blogs/post/what-is-memoization-in-react

[^21_2]: https://www.geeksforgeeks.org/reactjs/what-is-memoization-in-react/

[^21_3]: https://www.topcoder.com/thrive/articles/memoization-in-react-js

[^21_4]: https://refine.dev/blog/react-memo-guide/

[^21_5]: https://www.bitovi.com/blog/how-and-when-to-memoize-your-react-application

[^21_6]: https://react.dev/reference/react/memo

[^21_7]: https://www.epicreact.dev/memoization-and-react

[^21_8]: https://react.dev/reference/react/useMemo

[^21_9]: https://dev.to/bilelsalemdev/mastering-react-a-deep-dive-into-memoization-and-component-optimization-675

[^21_10]: https://www.freecodecamp.org/news/memoization-in-javascript-and-react/


---

# Code splitting?

Code splitting in React breaks large bundles into smaller chunks loaded on demand, reducing initial load time and improving performance in SPAs.[^22_1]

## React.lazy + Suspense

```
Use `React.lazy()` for dynamic imports and `<Suspense>` for loading states: `const LazyComp = lazy(() => import('./HeavyComp'));`. Wrap in `<Suspense fallback={<Spinner />}>` to show fallbacks during chunk fetch.[^22_4][^22_1]
```

```
<Suspense fallback={<div>Loading...</div>}>
  <Routes>
    <Route path="/dashboard" element={<LazyDashboard />} />
  </Routes>
</Suspense>
```


## Best Strategies

- **Route-based**: Split at route level with React Router—load page code only when navigated.
- **Component-based**: Lazy-load heavy modals, charts, or conditional features below the fold.
- **Webpack auto-handling**: Create React App/Next.js configure dynamic `import()` automatically.[^22_3]


## Benefits and Trade-offs

| Aspect | Impact |
| :-- | :-- |
| Initial Load | Smaller JS bundle (e.g., 50KB vs 2MB) |
| TTI (Time to Interactive) | Faster core app startup |
| Network | Extra HTTP requests for chunks |
| Caching | Browser caches chunks separately |

Start with routes for biggest wins; measure with Lighthouse before/after.[^22_1]
<span style="display:none">[^22_10][^22_2][^22_5][^22_6][^22_7][^22_8][^22_9]</span>

<div align="center">⁂</div>

[^22_1]: https://dev.to/kirubelkinfe/mastering-code-splitting-in-react-best-practices-for-optimal-performance-2o40

[^22_2]: https://www.reddit.com/r/reactjs/comments/10y2tif/maximising_performance_with_react_code_splitting/

[^22_3]: https://www.greatfrontend.com/blog/code-splitting-and-lazy-loading-in-react

[^22_4]: https://www.geeksforgeeks.org/reactjs/code-splitting-in-react/

[^22_5]: https://legacy.reactjs.org/docs/code-splitting.html

[^22_6]: https://dev.to/franklin030601/code-splitting-in-react-js-4o2g

[^22_7]: https://www.xenonstack.com/insights/code-splitting-in-react

[^22_8]: https://www.youtube.com/watch?v=JU6sl_yyZqs

[^22_9]: https://www.mindbowser.com/react-code-splitting-guide/

[^22_10]: https://www.velotio.com/engineering-blog/optimize-react-app-performance-by-code-splitting


---

# what Lazy loading?

Lazy loading is a performance technique in React that defers loading non-critical components or resources until they're needed, reducing initial bundle size and speeding up page loads.[^23_1]

## React Implementation

```
Uses `React.lazy()` with dynamic imports and `<Suspense>` for loading fallbacks: `const LazyComp = lazy(() => import('./HeavyComp'));`. While the chunk loads, `<Suspense fallback={<Spinner />}>` shows a placeholder.[^23_3][^23_1]
```

```jsx
import { lazy, Suspense } from 'react';
const Dashboard = lazy(() => import('./Dashboard'));

function App() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <Routes>
        <Route path="/dashboard" element={<Dashboard />} />
      </Routes>
    </Suspense>
  );
}
```


## Common Use Cases

- **Route-level**: Load page components only when navigated (pairs with React Router).
- **Below-the-fold**: Images, modals, charts that appear on scroll/interaction.
- **Conditional**: Admin panels for non-admin users.[^23_4]


## Benefits vs Code Splitting

| Aspect | Lazy Loading | Regular Bundle |
| :-- | :-- | :-- |
| Initial Load | Smaller JS chunks | Full app code |
| UX | Loading spinners | Instant (if small) |
| Network | More requests | Single request |

Webpack auto-creates chunks from `import()` calls. Always wrap in Error Boundaries—failed imports throw errors.[^23_1]
<span style="display:none">[^23_10][^23_2][^23_5][^23_6][^23_7][^23_8][^23_9]</span>

<div align="center">⁂</div>

[^23_1]: https://www.geeksforgeeks.org/reactjs/lazy-loading-in-react-and-how-to-implement-it/

[^23_2]: https://www.geeksforgeeks.org/reactjs/implementing-lazy-loading-for-functional-components-in-react/

[^23_3]: https://www.greatfrontend.com/blog/code-splitting-and-lazy-loading-in-react

[^23_4]: https://www.browserstack.com/guide/lazy-loading-in-react

[^23_5]: https://www.prioxis.com/blog/lazy-loading-in-react

[^23_6]: https://react.dev/reference/react/lazy

[^23_7]: https://dev.to/joodi/lazy-loading-in-react-4gdg

[^23_8]: https://strapi.io/blog/lazy-loading-in-react

[^23_9]: https://www.youtube.com/watch?v=nS5qbSJLGx8

[^23_10]: https://www.syncfusion.com/blogs/post/lazy-loading-with-react


---

# Why keys are important?

Keys in React are special props required when rendering lists, helping React identify which items changed, were added, or removed during reconciliation.[^24_1]

## Why Keys Matter

React's diffing algorithm uses keys as stable identities to minimize DOM operations. Without unique keys, React assumes list items shifted positions, causing unnecessary re-renders or state loss across the entire list.[^24_2]

Bad example (index keys):

```jsx
{items.map((item, index) => <Item key={index} />)} // Causes bugs on reorder
```

Good example:

```jsx
{items.map((item) => <Item key={item.id} />)} // Stable, unique ID
```


## Performance Impact

| Scenario | Without Proper Keys | With Stable Keys |
| :-- | :-- | :-- |
| Add item at start | Re-mounts ALL items | Only adds new item |
| Reorder list | Re-renders entire list | Updates positions efficiently |
| Remove middle item | Shifts/re-renders remaining | Removes only target |

## Best Practices

- Use unique, stable IDs from data (`item.id`, `item.uuid`).
- Avoid array `index` for dynamic lists (reorder/add/remove breaks).
- Never use random values or Math.random()—keys must persist across renders.
- For static lists, index is acceptable (no mutations).

Stable keys preserve component state (focus, scroll position) and enable optimal reconciliation.[^24_3][^24_5]
<span style="display:none">[^24_10][^24_4][^24_6][^24_7][^24_8][^24_9]</span>

<div align="center">⁂</div>

[^24_1]: https://namastedev.com/blog/understanding-react-key-prop-6/

[^24_2]: https://www.greatfrontend.com/questions/quiz/what-is-the-purpose-of-the-key-prop-in-react

[^24_3]: https://namastedev.com/blog/understanding-react-key-prop-4/

[^24_4]: https://www.reddit.com/r/reactjs/comments/1ku5w5l/is_this_correct_for_why_is_the_key_prop_important/

[^24_5]: https://www.emoosavi.com/blog/understanding-react-key-prop

[^24_6]: https://www.hackerone.com/blog/leveraging-key-property-react-efficient-rendering

[^24_7]: https://www.epicreact.dev/why-react-needs-a-key-prop

[^24_8]: https://kentcdodds.com/blog/understanding-reacts-key-prop

[^24_9]: https://www.developerway.com/posts/react-key-attribute

[^24_10]: https://dev.to/arnaudambro/mastering-and-taking-advantage-of-react-key-prop-2k21


---

# How large app folder structure looks?

Large React app folder structures organize code by **feature** rather than type for scalability, separating concerns across components, hooks, utilities, and pages.[^25_1][^25_2]

## Recommended Large-Scale Structure

```
src/
├── app/                 # Root component + providers (Router, QueryClient, Theme)
│   ├── index.tsx
│   └── providers.tsx
├── pages/ | app/        # Route-based pages (Next.js style)
│   ├── dashboard/
│   │   ├── page.tsx
│   │   ├── layout.tsx
│   │   └── loading.tsx
│   └── users/
│       └── [id]/
│           └── page.tsx
├── features/            # Domain/business logic modules
│   ├── auth/
│   │   ├── components/
│   │   ├── hooks/
│   │   ├── services/
│   │   └── types.ts
│   └── posts/
│       ├── components/
│       │   ├── PostList.tsx
│       │   └── PostCard.tsx
│       ├── hooks/
│       └── api.ts
├── components/          # Reusable UI components (global)
│   ├── ui/              # Atomic (Button, Input, Modal)
│   ├── layout/          # Header, Sidebar, Footer
│   └── common/          # Table, FormField
├── hooks/               # Custom hooks (global)
│   ├── useDebounce.ts
│   └── useLocalStorage.ts
├── context/             # React Context providers
│   ├── AuthContext.tsx
│   └── ThemeContext.tsx
├── services/            # API calls, external integrations
│   ├── api/
│   └── storage.ts
├── utils/               # Helpers, constants, validators
├── types/               # Global TypeScript interfaces
└── lib/                 # Configs (React Query, Axios)
```


## Key Patterns

- **Feature folders** group related files (components + hooks + API for that domain).
- **Component colocation**: Tests, styles next to source (`PostList.test.tsx`, `PostList.module.css`).
- **Pages → Features → Components** hierarchy prevents prop drilling.
- **Atomic design** in `components/ui/` for headless primitives.

This scales from 10 to 10k+ components while maintaining discoverability.[^25_4][^25_1]
<span style="display:none">[^25_10][^25_3][^25_5][^25_6][^25_7][^25_8][^25_9]</span>

<div align="center">⁂</div>

[^25_1]: https://www.robinwieruch.de/react-folder-structure/

[^25_2]: https://www.sitepoint.com/organize-large-react-application/

[^25_3]: https://www.youtube.com/watch?v=Mm6_DlO5vvs

[^25_4]: https://www.linkedin.com/pulse/ultimate-react-folder-structure-2025-sujan-adhikari-zxvzf

[^25_5]: https://www.developerway.com/posts/react-project-structure

[^25_6]: https://dev.to/pramod_boda/recommended-folder-structure-for-react-2025-48mc

[^25_7]: https://dev.to/itswillt/folder-structures-in-react-projects-3dp8

[^25_8]: https://blog.webdevsimplified.com/2022-07/react-folder-structure/

[^25_9]: https://www.youtube.com/watch?v=QhSXNY8sy_0

[^25_10]: https://www.reddit.com/r/reactjs/comments/r4l23i/react_folder_structure_for_enterprise_level/


---

# How did you design frontend architecture?

Frontend architecture design follows scalable patterns prioritizing separation of concerns, performance, and maintainability for large React applications.[^26_1]

## Core Layers

```
├── Presentation (Components/UI) - Pure, reusable UI
├── Business Logic (Hooks/Services) - State, API calls  
├── Data Layer (API, Storage) - External integrations
└── Infrastructure (Config, Utils) - Cross-cutting concerns
```


## Key Design Principles

**1. Feature-Sliced Structure**: Organize by domain/feature slices (`features/auth/`, `features/dashboard/`) containing all related files (components, hooks, API, types)—eliminates prop drilling.[^26_1]

**2. Custom Hooks First**: Extract logic into `useAuth()`, `usePosts()`—components stay thin, logic reusable/testable. Follow "hooks over HOCs" pattern.[^26_2]

**3. Centralized API Layer**: Single `services/api/` with interceptors handles auth, caching (React Query/TanStack Query), error mapping—not inline `fetch()` calls.[^26_3]

**4. State Strategy**:

- Local: `useState` + `useReducer`
- Shared: Context API (simple globals)
- Complex: Zustand/Jotai (lightweight) or Redux Toolkit (legacy scale)


## Performance Architecture

| Concern | Technique | Implementation |
| :-- | :-- | :-- |
| Re-renders | `React.memo` + `useCallback` | Stable props to children |
| Lists | `react-window` | Virtualized scrolling |
| Bundles | `React.lazy()` + Suspense | Route/component splitting |
| Queries | React Query | Auto-caching, optimistic updates |

## Modern Stack (2026)

```
Framework: Next.js 15 (App Router)
State: TanStack Query + Zustand
Styling: Tailwind + shadcn/ui
Routing: File-based (Next.js)
Validation: Zod + React Hook Form
Testing: Vitest + React Testing Library + MSW
```

**Single Responsibility**: Components render, hooks manage logic, services handle data. Custom hooks + atomic components (`Button`, `Input`) enable massive scale while staying maintainable.[^26_2][^26_1]
<span style="display:none">[^26_10][^26_4][^26_5][^26_6][^26_7][^26_8][^26_9]</span>

<div align="center">⁂</div>

[^26_1]: https://www.geeksforgeeks.org/reactjs/react-architecture-pattern-and-best-practices/

[^26_2]: https://dev.to/mourya_modugula/best-practices-for-designing-a-robust-react-architecture-5387

[^26_3]: https://nareshit.com/blogs/react-architecture-best-practices-scalable-apps

[^26_4]: https://www.simform.com/blog/react-architecture-best-practices/

[^26_5]: https://www.bacancytechnology.com/blog/react-architecture-patterns-and-best-practices

[^26_6]: https://www.sitepoint.com/react-architecture-best-practices/

[^26_7]: https://www.uxpin.com/studio/blog/react-best-practices/

[^26_8]: https://www.freecodecamp.org/news/best-practices-for-react/

[^26_9]: https://strapi.io/blog/react-and-nextjs-in-2025-modern-best-practices

[^26_10]: https://www.etatvasoft.com/blog/react-architecture-patterns/


---

# How did you handle booking UI state?

Booking UI state management requires coordinating multiple interdependent pieces: dates, guests, room selection, pricing, availability, and form validation.

## State Structure

```
bookingState = {
  step: 'dates' | 'rooms' | 'guests' | 'summary' | 'payment',
  checkIn: Date | null,
  checkNights: number,
  guests: { adults: 1, children: 0 },
  selectedRooms: { roomTypeId: quantity },
  promoCode: '',
  totalPrice: 0,
  isValid: false,
  errors: {}
}
```


## Recommended Architecture

**Multi-layer approach** for complex booking flows:

```
features/booking/
├── state/
│   ├── useBookingState.ts     # Main reducer + actions
│   ├── useAvailability.ts     # Async availability checks
│   └── usePricing.ts          # Dynamic pricing logic
├── components/
│   ├── DatePickerStepper.tsx
│   ├── RoomSelector.tsx
│   └── GuestCounter.tsx
└── validation/
    └── bookingSchema.ts       # Zod schema
```


## Implementation Pattern

```jsx
// Custom reducer for booking flow
const useBookingState = () => {
  const [state, dispatch] = useReducer(bookingReducer, initialState);
  
  const setDates = useCallback((checkIn, nights) => {
    dispatch({ type: 'SET_DATES', payload: { checkIn, nights } });
  }, []);
  
  const addRoom = useCallback((roomTypeId) => {
    dispatch({ type: 'ADD_ROOM', payload: roomTypeId });
  }, []);
  
  return { state, setDates, addRoom, ... };
};
```


## State Synchronization Strategy

1. **Local form state**: `React Hook Form + Zod` for validation
2. **Availability sync**: `useEffect` on date/guest changes → API call
3. **Pricing recalc**: `useMemo` based on [dates, rooms, promoCode]
4. **Stepper progress**: Derived from `step` + `isValid`

## Multi-Step Booking Flow

| Step | State Dependencies | Validation |
| :-- | :-- | :-- |
| Dates | `checkIn, nights` | Valid date range |
| Rooms | `dates, guests` | Availability check |
| Guests | `selectedRooms` | Capacity limits |
| Summary | All above | Final price calc |

## Key Optimizations

- **Split concerns**: Separate availability, pricing, form state
- **Debounced API**: `useDebounce` for availability checks
- **Memoized selectors**: `useMemo` for `totalPrice`, `availableRooms`
- **Optimistic updates**: Show rooms immediately, validate async
- **Persist draft**: `localStorage` for abandoned bookings

```
<BookingProvider>
  <Stepper currentStep={state.step}>
    <DateStep onNext={handleDateSelect} />
    <RoomStep rooms={availableRooms} />
  </Stepper>
</BookingProvider>
```

This scales from simple hotel booking to complex multi-property, multi-service reservations while maintaining performance and UX.[^27_1]
<span style="display:none">[^27_10][^27_2][^27_3][^27_4][^27_5][^27_6][^27_7][^27_8][^27_9]</span>

<div align="center">⁂</div>

[^27_1]: https://www.lochana.co/projects/hotel-booking-redux-state-management

[^27_2]: https://reacthandbook.dev/state-management

[^27_3]: https://www.developerway.com/posts/react-state-management-2025

[^27_4]: https://www.youtube.com/watch?v=RkWpJ4XUHuw

[^27_5]: https://github.com/RMontSerrat/booking-react-example

[^27_6]: https://www.youtube.com/watch?v=QSjHs5Qd-GQ

[^27_7]: https://www.shadcn.io/template/author/eduamdev

[^27_8]: https://react.dev/learn/managing-state

[^27_9]: https://blog.logrocket.com/modern-guide-react-state-patterns/

[^27_10]: https://helicon.ai/blog/application-state-management-patterns/


---

# How did you prevent duplicate booking?

Preventing duplicate bookings requires a **multi-layer defense** combining optimistic UI, real-time sync, backend constraints, and transaction isolation.

## Frontend Prevention Strategy

### 1. **Optimistic Locking with Version Tokens**

```jsx
const usePreventDoubleBooking = () => {
  const [bookingLock, setBookingLock] = useState(null);
  
  const attemptBooking = async (bookingData) => {
    // Generate temporary lock token
    const tempLock = crypto.randomUUID();
    setBookingLock(tempLock);
    
    try {
      const response = await api.bookRoom({
        ...bookingData,
        version: room.version, // Optimistic lock
        clientLock: tempLock
      });
      
      if (response.isDuplicate) {
        toast.error('Room no longer available');
        return false;
      }
      return true;
    } finally {
      setBookingLock(null);
    }
  };
};
```


### 2. **Real-time Availability (WebSocket/Pusher)**

```jsx
// Listen for booking conflicts from other users
useEffect(() => {
  const channel = pusher.subscribe(`room-${roomId}`);
  channel.bind('booking-conflict', (data) => {
    if (tempBookingId === data.tempId) {
      setError('Booking taken by another user');
      resetForm();
    }
  });
  
  return () => channel.unsubscribe();
}, [roomId]);
```


### 3. **Race Condition-Resistant Flow**

```
1. User selects dates/rooms → Generate tempBookingId
2. Show "HOLDING" status (5-min timer)
3. Poll availability every 3s OR WebSocket updates
4. On payment → Finalize with tempBookingId + version check
5. Backend: `SELECT FOR UPDATE` + atomic insert
```


## Backend Implementation (Critical)

### Database Constraints

```sql
-- Prevent overlapping bookings per room
CREATE UNIQUE INDEX idx_room_date_overlap 
ON bookings (room_id, checkin_date, checkout_date) 
WHERE status = 'confirmed';

-- Or use EXCLUDE constraint for date ranges
ALTER TABLE bookings ADD CONSTRAINT no_overlap 
EXCLUDE USING gist (
  tsrange(checkin_date, checkout_date) WITH &&,
  room_id WITH =
) WHERE status = 'confirmed';
```


### Transaction with SELECT FOR UPDATE

```javascript
app.post('/book', async (req, res) => {
  const client = await pool.connect();
  try {
    await client.query('BEGIN');
    
    // Lock room availability
    const availability = await client.query(
      'SELECT * FROM room_availability WHERE room_id = $1 AND date BETWEEN $2 AND $3 FOR UPDATE',
      [roomId, checkIn, checkOut]
    );
    
    if (availability.rows.length === 0) {
      throw new Error('Not available');
    }
    
    // Optimistic version check
    if (req.body.version !== availability.rows[^28_0].version) {
      throw new Error('Version conflict - room taken');
    }
    
    await client.query('INSERT INTO bookings ...');
    await client.query('COMMIT');
    
  } catch (error) {
    await client.query('ROLLBACK');
    res.status(409).json({ error: 'Booking unavailable' });
  }
});
```


## UI/UX Implementation

### Visual Indicators

```
Room Status States:
✅ Available → "Book Now" (green)
⏳ Holding → "Complete Booking" (orange, 4:59 timer)
❌ Taken → "No Longer Available" (red)
```


### Timeout Handling

```jsx
const useBookingTimer = (tempBookingId) => {
  useEffect(() => {
    const timeout = setTimeout(() => {
      if (tempBookingId) {
        api.releaseHold(tempBookingId);
        navigate('/rooms');
      }
    }, 300000); // 5 minutes
    
    return () => clearTimeout(timeout);
  }, [tempBookingId]);
};
```


## Complete Flow Summary

| Layer | Mechanism | Fallback |
| :-- | :-- | :-- |
| **UI** | Temp hold + timer | Auto-release |
| **API** | Version check + SELECT FOR UPDATE | 409 Conflict |
| **DB** | Unique constraints + transactions | Hard constraint |
| **Real-time** | WebSocket conflict notification | Poll backup |

**Key**: Never trust frontend availability—always validate backend with proper transaction isolation. The 5-minute hold window gives great UX while database constraints provide bulletproof protection.[^28_1]
<span style="display:none">[^28_10][^28_2][^28_3][^28_4][^28_5][^28_6][^28_7][^28_8][^28_9]</span>

<div align="center">⁂</div>

[^28_1]: https://demo.mobiscroll.com/react/eventcalendar/prevent-double-booking-events

[^28_2]: https://www.reddit.com/r/rails/comments/7vrmja/updating_clients_in_realtime_to_avoid_double/

[^28_3]: https://demo.mobiscroll.com/react/scheduler/prevent-double-booking-events

[^28_4]: https://www.reddit.com/r/webdev/comments/10di9t2/how_to_preventing_double_booking_for_a_cinema/

[^28_5]: https://stackoverflow.com/questions/72180172/how-to-prevent-duplicate-booking-and-set-time-slots-using-nextjs-and-prisma

[^28_6]: https://www.linkedin.com/posts/mayank-k1211_github-arungk703cultfit-booking-automation-activity-7324864897333227520-Tn5K

[^28_7]: https://github.com/jlcarrascof/Booking-React-Clone

[^28_8]: https://www.youtube.com/watch?v=RkWpJ4XUHuw

[^28_9]: https://forum.bryntum.com/viewtopic.php?t=16705

[^28_10]: https://www.youtube.com/watch?v=_95dCYv2Xv4


---

# How did you handle loading and error states?

Loading and error states were managed systematically across UI layers using consistent patterns for optimal UX in booking flows.

## Standardized State Triad

Every data-fetching operation follows `{ data, loading, error }` pattern:

```jsx
const useRoomAvailability = (roomId, dates) => {
  const [state, setState] = useState({
    data: null,
    loading: false,
    error: null
  });

  const fetchAvailability = useCallback(async () => {
    setState(s => ({ ...s, loading: true, error: null }));
    try {
      const result = await api.getAvailability(roomId, dates);
      setState({ data: result, loading: false, error: null });
    } catch (err) {
      setState({ data: null, loading: false, error: err.message });
    }
  }, [roomId, dates]);

  return { ...state, refetch: fetchAvailability };
};
```


## Component Rendering Logic

```
if (loading)     → <SkeletonLoader />
elif (error)     → <ErrorMessage retry={refetch} />
else (data)      → <RoomResults data={data} />
```


## Loading States Hierarchy

```
Global App Loader    → <FullScreenSpinner />
Page-level           → <PageSkeleton />
Section-level        → <RoomGridSkeleton />
Row-level            → <RoomCardSkeleton />
Button-level         → <LoadingButtonSpinner />
```


## Error Boundary Implementation

```jsx
class BookingErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false, error: null };
  }

  static getDerivedStateFromError(error) {
    return { hasError: true, error };
  }

  componentDidCatch(error, errorInfo) {
    // Log to service (Sentry)
    logError(error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      return (
        <ErrorScreen 
          message={this.state.error?.message}
          onRetry={this.props.onRetry}
          onHome={navigateToHome}
        />
      );
    }
    return this.props.children;
  }
}
```


## Network Status Manager

Centralized optimistic feedback:

```jsx
const NetworkStatus = {
  IDLE: 'idle',
  LOADING: 'loading', 
  SUCCESS: 'success',
  ERROR: 'error'
};

// Context tracks concurrent requests
const useGlobalLoading = () => {
  const { pendingRequests } = useContext(NetworkContext);
  return pendingRequests > 0;
};
```


## Skeleton Implementation

```jsx
const RoomCardSkeleton = () => (
  <div className="animate-pulse">
    <div className="h-48 bg-gray-200 rounded-lg mb-4"></div>
    <div className="h-6 bg-gray-200 rounded w-3/4 mb-2"></div>
    <div className="h-4 bg-gray-200 rounded w-1/2"></div>
  </div>
);
```


## Progressive Enhancement Flow

```
1. Skeleton (immediate) → Builds layout
2. Data arrives → Fades in over skeleton  
3. Error → Retry/Error UI (non-blocking)
4. Empty state → "No rooms available"
```


## Key Patterns Used

| State | UI Pattern | Duration |
| :-- | :-- | :-- |
| **Loading** | Skeleton + Progress | < 2s typical |
| **Error** | Inline message + Retry | Persistent until resolved |
| **Empty** | Illustration + CTA | When `data?.length === 0` |
| **Success** | Content + Optimistic feedback | Default state |

**Fallback Chain**: Skeleton → Error Boundary → Full App Reset ensures users always see *something*, never blank screens. Retry logic with exponential backoff prevents API spam during outages.[^29_1]
<span style="display:none">[^29_10][^29_2][^29_3][^29_4][^29_5][^29_6][^29_7][^29_8][^29_9]</span>

<div align="center">⁂</div>

[^29_1]: https://www.youtube.com/watch?v=ojFlwOc0tDc

[^29_2]: https://www.youtube.com/watch?v=IyQ7Wm3k88o

[^29_3]: https://www.reddit.com/r/reactjs/comments/j7qb37/how_to_handle_loading_state_when_its_derived_from/

[^29_4]: https://www.youtube.com/watch?v=_0iFVd_bC9w

[^29_5]: https://coreui.io/answers/how-to-show-loading-state-in-react/

[^29_6]: https://blog.logrocket.com/handling-react-loading-states-react-loading-skeleton/

[^29_7]: https://www.robinwieruch.de/react-form-loading-pending-action/

[^29_8]: https://www.youtube.com/watch?v=V0VfR0eaz98

[^29_9]: https://dev.to/cristiansifuentes/mastering-react-suspense-loading-states-done-right-4083

[^29_10]: https://github.com/anishparajuli/ticket-booking


---

# How did you protect admin routes?

Admin routes were protected using a **layered security approach** combining authentication, role-based authorization, and route guards with seamless UX.

## Auth Context + Role Management

```jsx
// context/AuthContext.tsx
export const AuthContext = createContext({
  user: null,
  isAdmin: false,
  isAuthenticated: false,
  login: () => {},
  logout: () => {}
});

const useAuth = () => {
  const { user } = useContext(AuthContext);
  return {
    isAuthenticated: !!user,
    isAdmin: user?.role === 'admin',
    hasPermission: (permission) => user?.permissions?.includes(permission)
  };
};
```


## Route Guard Components

```jsx
// guards/AdminRoute.tsx
const AdminRoute = ({ children }) => {
  const { isAuthenticated, isAdmin, isLoading } = useAuth();
  
  if (isLoading) return <LoadingSpinner />;
  
  if (!isAuthenticated) {
    return <Navigate to="/login" state={{ from: location }} replace />;
  }
  
  if (!isAdmin) {
    return <Navigate to="/unauthorized" replace />;
  }
  
  return children;
};

// Usage
<Route 
  path="/admin/*" 
  element={
    <AdminRoute>
      <AdminLayout>
        <AdminRoutes />
      </AdminLayout>
    </AdminRoute>
  } 
/>
```


## Router Structure

```
<BrowserRouter>
  <AuthProvider>
    <Routes>
      {/* Public */}
      <Route path="/login" element={<Login />} />
      
      {/* Protected */}
      <Route element={<ProtectedLayout />}>
        <Route path="/" element={<Dashboard />} />
        <Route path="/profile" element={<Profile />} />
      </Route>
      
      {/* Admin */}
      <Route path="/admin/*" element={
        <AdminRoute>
          <AdminLayout />
        </AdminRoute>
      }>
        <Route path="dashboard" element={<AdminDashboard />} />
        <Route path="users" element={<UserManagement />} />
      </Route>
    </Routes>
  </AuthProvider>
</BrowserRouter>
```


## Token-Based Backend Verification

```jsx
// hooks/useAdminApi.ts
const useAdminApi = () => {
  const { token } = useAuth();
  
  const apiCall = useCallback(async (endpoint) => {
    const response = await fetch(`/api/admin${endpoint}`, {
      headers: { Authorization: `Bearer ${token}` }
    });
    
    if (response.status === 403) {
      // Invalid admin token
      logout();
      navigate('/unauthorized');
      return;
    }
    
    return response.json();
  }, [token]);
};
```


## Security Layers

| Layer | Protection | Fallback |
| :-- | :-- | :-- |
| **Route Guard** | Role check on navigation | Redirect + location state |
| **API Middleware** | JWT role verification | 403 → Logout |
| **Component-level** | `useAdmin()` hook | Hide UI elements |
| **Database** | Row-level security | Backend constraint |

## UX Implementation

```jsx
// Loading states during auth check
const ProtectedComponent = () => {
  const { isAdmin, isLoading } = useAuth();
  
  if (isLoading) return <div>Verifying access...</div>;
  if (!isAdmin) return <div>Access denied</div>;
  
  return <AdminContent />;
};
```


## Key Features

- **Location preservation**: Failed auth redirects preserve `from` URL
- **Centralized auth**: Single `useAuth()` hook everywhere
- **Progressive loading**: Spinner → Auth check → Content/Redirect
- **Dual verification**: Frontend guard + backend API checks
- **Graceful degradation**: Network/auth failures handled consistently

**Critical**: Frontend guards provide UX; backend JWT verification provides security. Never trust client-side role checks alone.[^30_1][^30_2]
<span style="display:none">[^30_10][^30_3][^30_4][^30_5][^30_6][^30_7][^30_8][^30_9]</span>

<div align="center">⁂</div>

[^30_1]: https://marmelab.com/react-admin/Authentication.html

[^30_2]: https://stackoverflow.com/questions/43164554/how-to-implement-authenticated-routes-in-react-router-4

[^30_3]: https://www.reddit.com/r/reactjs/comments/17et9gm/best_way_to_handle_admin_routes_for_an_application/

[^30_4]: https://stackoverflow.com/questions/61240342/react-how-to-structure-routes-for-admin-user-and-public

[^30_5]: https://developer.auth0.com/resources/guides/spa/react/basic-authentication

[^30_6]: https://marmelab.com/react-admin/Routing.html

[^30_7]: https://coreui.io/answers/how-to-implement-authentication-in-react/

[^30_8]: https://www.youtube.com/watch?v=qq8IXLAOyLc

[^30_9]: https://marmelab.com/react-admin/doc/2.9/Authentication.html

[^30_10]: https://forum.freecodecamp.org/t/express-react-router-auth-for-one-admin-user/244225


---

# How did you store JWT securely?

JWT tokens were stored using **HttpOnly Secure cookies** with a backend proxy pattern, avoiding client-side storage vulnerabilities like XSS attacks.[^31_1][^31_4]

## Secure Storage Implementation

### Backend-Controlled Cookie Setting

```jsx
// API response sets cookie (never exposed to JS)
app.post('/api/login', async (req, res) => {
  const { email, password } = req.body;
  const user = await authenticateUser(email, password);
  
  const token = generateJWT(user);
  
  res.cookie('auth_token', token, {
    httpOnly: true,      // Prevents XSS access
    secure: true,        // HTTPS only
    sameSite: 'strict',  // CSRF protection
    maxAge: 15 * 60 * 1000 // 15 minutes
  });
  
  res.json({ success: true });
});
```


### Proxy API Pattern (No Client Token Access)

```jsx
// Frontend NEVER sees the token
const useApi = () => {
  return useCallback(async (endpoint, options = {}) => {
    // Browser auto-sends cookie with every request
    const response = await fetch(`/api${endpoint}`, {
      credentials: 'include',  // Include cookies
      ...options
    });
    
    if (response.status === 401) {
      window.location.href = '/login';
    }
    
    return response.json();
  }, []);
};

// Usage - transparent to components
const rooms = useQuery(['rooms'], () => api('/rooms'));
```


## Multi-Layer Token Strategy

```
Access Token (15min)  → HttpOnly cookie → Proxy API calls
Refresh Token (7 days)→ Backend session → Auto-refresh
```


### Automatic Refresh Flow

```javascript
// Backend middleware auto-refreshes
app.use('/api/protected', async (req, res, next) => {
  let token = req.cookies.auth_token;
  
  try {
    const decoded = verifyJWT(token);
    req.user = decoded;
    next();
  } catch (exp) {
    if (exp.name === 'TokenExpiredError') {
      // Auto-issue new token from refresh session
      const newToken = refreshAccessToken(req.session.refreshToken);
      res.cookie('auth_token', newToken, { httpOnly: true, secure: true });
      req.user = verifyJWT(newToken);
      next();
    } else {
      res.status(401).json({ error: 'Unauthorized' });
    }
  }
});
```


## Security Guarantees

| Attack Vector | Protection | Status |
| :-- | :-- | :-- |
| **XSS** | HttpOnly flag | ✅ Blocked |
| **CSRF** | SameSite=strict | ✅ Blocked |
| **MITM** | Secure + HTTPS | ✅ Encrypted |
| **Network** | Short expiry (15min) | ✅ Limited blast radius |

## Logout Implementation

```javascript
app.post('/api/logout', (req, res) => {
  // Immediately invalidate cookie
  res.clearCookie('auth_token', {
    httpOnly: true,
    secure: true,
    sameSite: 'strict'
  });
  
  // Backend blacklist (Redis, 15min)
  blacklistToken(req.cookies.auth_token);
  
  res.json({ success: true });
});
```


## Frontend UX Layer

```jsx
// Components oblivious to tokens
const useAuthStatus = () => {
  const [status, setStatus] = useState('checking');
  
  useEffect(() => {
    api('/auth/verify')
      .then(() => setStatus('authenticated'))
      .catch(() => setStatus('unauthenticated'));
  }, []);
  
  return status;
};
```

**Why HttpOnly wins**: Zero client-side token exposure. XSS can't steal what JavaScript can't access. CSRF blocked by SameSite. Backend controls refresh/revocation. Scales perfectly for booking apps with sensitive admin operations.[^31_4][^31_1]
<span style="display:none">[^31_10][^31_2][^31_3][^31_5][^31_6][^31_7][^31_8][^31_9]</span>

<div align="center">⁂</div>

[^31_1]: https://workos.com/blog/secure-jwt-storage

[^31_2]: https://snyk.io/blog/top-3-security-best-practices-for-handling-jwts/

[^31_3]: https://www.descope.com/blog/post/developer-guide-jwt-storage

[^31_4]: https://www.syncfusion.com/blogs/post/secure-jwt-storage-best-practices

[^31_5]: https://curity.io/resources/learn/jwt-best-practices/

[^31_6]: https://www.cyberchief.ai/2023/05/secure-jwt-token-storage.html

[^31_7]: https://www.reddit.com/r/node/comments/n5qf8b/best_way_to_store_jwt_tokens/

[^31_8]: https://duendesoftware.com/learn/best-practices-using-jwts-with-web-and-mobile-apps

[^31_9]: https://www.reddit.com/r/webdev/comments/x15xvg/jwt_storage_best_practices/

[^31_10]: https://www.reddit.com/r/reactjs/comments/sivveh/most_securebest_practice_for_storing_jwt_on/


---

## How did you handle payment integration?

Payment integration used **Stripe Checkout** with a secure backend proxy pattern. Frontend created checkout sessions via API, never handling card data directly. Backend generated Stripe sessions with `stripe.checkout.sessions.create()` using booking details (room, dates, price), redirecting to Stripe's hosted checkout. Success/cancel URLs handled post-payment flows with webhook verification for final booking confirmation.[^32_1]

```jsx
// Frontend - create checkout session
const handlePayment = async () => {
  const session = await api.post('/create-checkout-session', {
    bookingId,
    amount: totalPrice * 100, // cents
    roomId
  });
  stripe.redirectToCheckout({ sessionId: session.id });
};
```

Backend verified payment completion via Stripe webhooks before confirming bookings.

## How did you manage global state?

**Layered state architecture**: Local `useState` for forms, **TanStack Query** for server data (rooms, availability), **Zustand** for UI state (booking flow, theme), **Context API** for auth. Query handled caching, optimistic updates, infinite scroll; Zustand managed stepper state across unrelated components. No Redux—modern, lightweight tools with devtools integration.

```
Server State → TanStack Query (auto-cache, sync)
UI State     → Zustand (simple, fast)
Auth         → Context + HttpOnly cookies
Form State   → React Hook Form + Zod
```


## How did you handle logout?

**Secure multi-step logout**: Frontend called `/api/logout` → Backend cleared HttpOnly cookies + blacklisted JWT (Redis, 15min window) + cleared server sessions. Client-side cleared localStorage, reset auth context, navigated to login with location preservation. Real-time WebSocket notified other tabs/sessions.

```jsx
const handleLogout = async () => {
  await api.post('/logout'); // Clears cookies + blacklist
  localStorage.clear();
  authDispatch({ type: 'LOGOUT' });
  navigate('/login', { replace: true });
};
```


## How did you deploy frontend?

**Vercel deployment pipeline**: Git-based CI/CD with automatic previews per PR. Production builds used `next build` (Next.js App Router), code splitting, image optimization. Environment vars via Vercel dashboard (STRIPE_KEYS, API_URL). Custom domain + auto HTTPS. Monitoring with Sentry + Vercel Analytics. Rollbacks via Git revert.

```
git push → Vercel build → Preview URL → Review → Promote to prod
npm run build → 2m 30s → Live in 30s
```

<span style="display:none">[^32_10][^32_2][^32_3][^32_4][^32_5][^32_6][^32_7][^32_8][^32_9]</span>

<div align="center">⁂</div>

[^32_1]: https://www.youtube.com/watch?v=3OOHC_UzrKA

[^32_2]: https://www.youtube.com/watch?v=72iEz5iopqQ

[^32_3]: https://www.zignuts.com/blog/stripe-payment-integration-in-reactjs-guide

[^32_4]: https://dev.to/mominmahmud/implementing-stripe-payments-in-a-react-app-a-step-by-step-guide-4dd9

[^32_5]: https://www.geeksforgeeks.org/mern/how-to-integrate-stripe-payments-in-react-app-using-express/

[^32_6]: https://docs.stripe.com/sdks/stripejs-react

[^32_7]: https://www.youtube.com/watch?v=45Z1tqZx4fM

[^32_8]: https://docs.stripe.com/payments/accept-a-payment?platform=react-native

[^32_9]: https://www.youtube.com/watch?v=ZkpRfry5a_Y

[^32_10]: https://blog.risingstack.com/stripe-payments-integration-tutorial-javascript/

