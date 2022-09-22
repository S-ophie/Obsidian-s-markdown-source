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
0921

```ts
interface ITotal {

	photo_num_max?: number; // 自定义字段，当前最大 photo_num
	
	// 以下为接口字段
	
	name: string;
	
	photo_num: number;
	
	inner_color_bfb: string;
	
	out_color_bfb: string;
	
	spec_photo_bfb: string;

}

// 字面量类型

// type CompareType = 'photo_num'| 'spec_photo_bfb'| 'out_color_bfb'| 'inner_color_bfb';

  
type CompareType = Extract<

	keyof ITotal,

	'photo_num' | 'spec_photo_bfb' | 'out_color_bfb' | 'inner_color_bfb'

>;

type DetailType = `${CompareType}_max_name`;

  

/**

* 四种维度的胜出名单对象

*/


// type WinnerName = {

	// photo_num_max_name: string;

	// out_color_bfb_max_name: string;

	// inner_color_bfb_max_name: string;

	// spec_photo_bfb_max_name: string;

// };

  

// type WinnerName = {

	// [key in DetailType]: string;

// };

type WinnerName = Record<DetailType, string>;

  

type OptionWinner = Partial<WinnerName>
```

#### 字面量类型

```ts

type CompareType =| 'photo_num'| 'spec_photo_bfb'| 'out_color_bfb'| 'inner_color_bfb';

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


#### Record

```ts

type WinnerName = Record<DetailType, string>;

```

---
0927

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




参考：
- https://ts.xcatliu.com/
- https://react-typescript-cheatsheet.netlify.app/docs/basic/getting-started/forward_and_create_ref/

