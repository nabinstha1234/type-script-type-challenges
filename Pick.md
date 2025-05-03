
  ### Question

  Implement the built-in `Pick<T, K>` generic without using it.

  Constructs a type by picking the set of properties `K` from `T`

  For example:

  ```ts
  interface Todo {
    title: string
    description: string
    completed: boolean
  }

  type TodoPreview = MyPick<Todo, 'title' | 'completed'>

  const todo: TodoPreview = {
      title: 'Clean room',
      completed: false,
  }
  ```

```ts
/* _____________ Your Code Here _____________ */

type MyPick<T, K extends keyof T> = {
  [key in K]:T[key]
}

/* _____________ Test Cases _____________ */
import type { Equal, Expect } from '@type-challenges/utils'

type cases = [
  Expect<Equal<Expected1, MyPick<Todo, 'title'>>>,
  Expect<Equal<Expected2, MyPick<Todo, 'title' | 'completed'>>>,
  // @ts-expect-error
  MyPick<Todo, 'title' | 'completed' | 'invalid'>,
]

interface Todo {
  title: string
  description: string
  completed: boolean
}

interface Expected1 {
  title: string
}

interface Expected2 {
  title: string
  completed: boolean
}
```

This is what the most of the people do. I just want to explain the concept and usage of keyof and in here to avoid it for those who are new to it. 

keyof: Take the interface akey adn save it as a union type
```ts
interface userInfo {
  name: string
  age: number
}
type keyofValue = keyof userInfo
// keyofValue = "name" | "age"
```
in: Get the value of the union type, mainly used for construction array sand objects 

Remember not to use it for intercafe, otherwise an error will be reported

```ts
type name = 'firstname' | 'lastname'
type Name = {
  [key in name]: string
}
// Name = { firstname: string, lastname: string }
```

While development we can use:
```ts
function pickValue(o:object, key:string){
    return o[key];
}
const obj= {name:"Nabin", lastName:"Shrestha"}
const firstName = pickValue(obj,'name');
```
This gives error:
Element implicitly has an 'any' type because expression of type 'string' can't be used to index type '{}'.
  No index signature with a parameter of type 'string' was found on type '{}'.

The solution:
```ts
function pickValue<T extends Object,K extends keyof T>(o: T,key: K): T[K] {
  return o[key]
}
const obj= {name:"Nabin", lastName:"Shrestha"}
const firstName = pickValue(obj,'name');
```