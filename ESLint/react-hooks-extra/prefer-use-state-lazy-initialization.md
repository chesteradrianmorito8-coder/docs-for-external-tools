Pattern: Direct function call in `useState`

Issue: -

## Description

Enforces function calls made inside `useState` to be wrapped in an `initializer function`.

A function can be invoked inside a `useState` call to help create its initial state. However, subsequent renders will still invoke the function while discarding its return value. This is wasteful and can cause performance issues if the function call is expensive.

To combat this issue React allows useState calls to use an [initializer function](https://react.dev/reference/react/useState#avoiding-recreating-the-initial-state) which will only be called on the first render.


Example of **incorrect** code:

```js
import React, { useState } from "react";

function MyComponent() {
  const [value, setValue] = useState(generateTodos());
  //                                 ^^^^^^^^^^^^^^^
  //                                 - Don't call a function directly inside the 'useState' call.

  return null;
}

declare function generateTodos(): string[];
```

Example of **correct** code:

```js
import React, { useState } from "react";

function MyComponent() {
  // 🟢 Good: Use an initializer function to avoid recreating the initial state
  const [value, setValue] = useState(() => generateTodos());

  return null;
}

declare function generateTodos(): string[];
```
