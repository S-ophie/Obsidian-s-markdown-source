### 0831&0907

---



#### type

类型别名
接口：`?` 可选、`readonly`只读属性

#### &


交叉类型： 两边的成员并集

#### |


联合类型： 两边的成员交集

#### !


非空断言

#### useRef


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

#### as 


类型断言：欺骗编译器

#### forwardRef


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

#### import type


和编译有关  `importsNotUsedAsValues`


#### StyleHTMLAttributes 的扩展


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


### 0921

---



```ts

// 字面量类型
// type CompareType = 'photo_num'| 'spec_photo_bfb'| 'out_color_bfb'| 'inner_color_bfb'; 

type IncludeNullUndefined = string | number | null | undefined;

// Exclude
type NoNull = Exclude<IncludeNullUndefined, null>;
// NonNullable
type NoNullNoUndefined = NonNullable<IncludeNullUndefined>;

// Extract
type CompareType = Extract<
  keyof ITotal,
  'photo_num' | 'spec_photo_bfb' | 'out_color_bfb' | 'inner_color_bfb'
>;

// Pick
type SubTotal = Pick<
  ITotal,
  'photo_num' | 'spec_photo_bfb' | 'out_color_bfb' | 'inner_color_bfb'
>;

type OtherSubTotal = Omit<
  ITotal,
  'photo_num' | 'spec_photo_bfb' | 'out_color_bfb' | 'inner_color_bfb'
>;

// keyof
type SecondCompareType = keyof SubTotal;

type DetailType = `${CompareType}_max_name`;

// type WinnerName = {
// photo_num_max_name: string;
// out_color_bfb_max_name: string;
// inner_color_bfb_max_name: string;
// spec_photo_bfb_max_name: string;
// };

// in 类型守卫

// type WinnerName = {
//   [key in DetailType]: string;
// };

// Record
type WinnerName = Record<DetailType, string>;


// Partial
type OptionWinner = Partial<WinnerName>;

// Required
type RequiredWinner = Required<OptionWinner>

```

#### 字面量类型

```ts

type CompareType = 'photo_num'| 'spec_photo_bfb'| 'out_color_bfb'| 'inner_color_bfb';

```

#### 模板字符串

```ts

type DetailType = `${CompareType}_max_name`;

```


#### in

```ts

type WinnerName = {

	[key in DetailType]: string;

};

```


#### Partial

```ts

type OptionWinner = Partial<WinnerName>

```

---



### 0927

---



#### Record

```ts

type WinnerName = Record<DetailType, string>;


// 空对象

type NullObject = Record<string, never>

```

#### keyof


```ts

type CompareType = Extract<

	keyof ITotal,

	'photo_num' | 'spec_photo_bfb' | 'out_color_bfb' | 'inner_color_bfb'

>;

```


#### Extract


```ts

type CompareType = Extract<

	keyof ITotal,

	'photo_num' | 'spec_photo_bfb' | 'out_color_bfb' | 'inner_color_bfb'
	
>;

```


![[Excalidraw/Drawing 2022-09-22 12.40.51.excalidraw]]

#### Pick

```ts
type SubTotal = Pick<
  ITotal,
  'photo_num' | 'spec_photo_bfb' | 'out_color_bfb' | 'inner_color_bfb'
>;
```

#### Exclude

```ts
type IncludeNullUndefined = string | number | null | undefined;

// Exclude
type NoNull = Exclude<IncludeNullUndefined, null>;
```

#### NonNullable

```ts
// NonNullable
type NoNullNoUndefined = NonNullable<IncludeNullUndefined>;
```

#### Omit

```ts
type OtherSubTotal = Omit<
  ITotal,
  'photo_num' | 'spec_photo_bfb' | 'out_color_bfb' | 'inner_color_bfb'
>;
```

#### Required

```ts
type RequiredWinner = Required<OptionWinner>
```

**除了内置的泛型工具还有社区的，and you can try write it yourself (泛型函数)**

https://github.com/sindresorhus/type-fest
