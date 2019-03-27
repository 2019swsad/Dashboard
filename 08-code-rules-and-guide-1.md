# Code rules
## JavaScript part
## WE WILL USE TYPESCRIPT!!

install TypeScript
```
npm i typescript -g
```
compile TS to JS
```
tsc app.ts
```

### Base type 
Boolean
```
let isDone:boolean=false;
```

Number
```
let count:number=10;
```

string
```
let name:string='hello';
```

array
```
let list:number[]=[1,2,3]
let list:Array<number>=[1,2,3]
```

enum
```
enum Direction {
	NORTH,
    SOUTH,
    EAST,
    WEST
};

let dir: Direction = Direction.NORTH;
```

Dynamic
```
let dynamic:any = 4;
dynamic = "hello"
```

