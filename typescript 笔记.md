#### `type`
类型别名

#### `interface`
接口：`?` 可选、任意、`readonly`只读属性

#### `&`
交叉类型： 两边的成员并集

#### `|`
联合类型： 两边的成员交集

#### `!`
非空断言

#### `useRef`
```typescript
function Foo() {
  // Technical-wise, this returns MutableRefObject<number | null>
  const intervalRef = useRef<number | null>(null);

  // You manage the ref yourself (that's why it's called MutableRefObject!)
  useEffect(() => {
    intervalRef.current = setInterval(...);
    return () => clearInterval(intervalRef.current);
  }, []);

  // The ref is not passed to any element's "ref" prop
  return <button onClick={/* clearInterval the ref */}>Cancel timer</button>;
}
```

#### `as` 
类型断言：欺骗编译器

#### `forwardRef`
```typescript
import { forwardRef, ReactNode } from "react";

interface Props {
  children?: ReactNode;
  type: "submit" | "button";
}
export type Ref = HTMLButtonElement;

export const FancyButton = forwardRef<Ref, Props>((props, ref) => (
  <button ref={ref} className="MyClassName" type={props.type}>
    {props.children}
  </button>
));
```

#### `import type`
和编译有关  `importsNotUsedAsValues`


#### `StyleHTMLAttributes` 的扩展
```typescript 
// eslint-disable-next-line react/no-typos
import 'react';
declare module 'react' {
  interface StyleHTMLAttributes<T> extends React.HTMLAttributes<T> {
	jsx?: boolean;
	global?: boolean;
  }
}
```



---

参考：
- https://ts.xcatliu.com/
- https://react-typescript-cheatsheet.netlify.app/docs/basic/getting-started/forward_and_create_ref/