# TypeScript Tips and Tricks

## 1. Basic Types
- `string` - Represents text values.
- `number` - Represents numeric values.
- `boolean` - Represents `true` or `false`.
- `null` - Represents a null value.
- `undefined` - Represents an undefined value.
- `any` - Disables type checking (use cautiously).
- `unknown` - Similar to `any`, but requires type checking before use.
- `void` - Represents no return value (commonly used for functions that do not return anything).
- `never` - Represents a function that never returns (e.g., throws an error).
- `bigint` - Represents large numbers.
- `symbol` - Represents unique symbols.

## 2. Complex Types
- **Tuples**: Fixed-length arrays with specific types.
  ```typescript
  let tuple: [string, number] = ["Alice", 30];
  ```
- **Enums**: Named constants.
  ```typescript
  enum Role { Admin, User, Guest }
  let userRole: Role = Role.Admin;
  ```
- **Unions (`|`)**: Multiple allowed types.
  ```typescript
  let id: string | number = 123;
  ```
- **Intersection (`&`)**: Combines multiple types.
  ```typescript
  type Employee = { name: string } & { id: number };
  ```
- **Literals**: Exact values.
  ```typescript
  type Direction = "left" | "right";
  let move: Direction = "left";
  ```

## 3. Functions
- **Typed Parameters & Return Values**
  ```typescript
  function add(a: number, b: number): number {
    return a + b;
  }
  ```
- **Optional Parameters**
  ```typescript
  function greet(name: string, age?: number) {
    console.log(`Hello, ${name}`);
  }
  ```
- **Rest Parameters**
  ```typescript
  function sum(...numbers: number[]): number {
    return numbers.reduce((acc, num) => acc + num, 0);
  }
  ```

## 4. TypeScript Utility Types
- **`Partial<T>`**: Makes all properties optional.
  ```typescript
  type User = { name: string; age: number };
  type PartialUser = Partial<User>;
  ```
- **`Required<T>`**: Makes all properties required.
  ```typescript
  type RequiredUser = Required<PartialUser>;
  ```
- **`Readonly<T>`**: Makes all properties readonly.
  ```typescript
  type ReadonlyUser = Readonly<User>;
  ```
- **`Pick<T, K>`**: Selects specific keys from a type.
  ```typescript
  type PickedUser = Pick<User, "name">;
  ```
- **`Omit<T, K>`**: Removes specific keys from a type.
  ```typescript
  type OmittedUser = Omit<User, "age">;
  ```
- **`Record<K, T>`**: Creates an object type with specified key-value pairs.
  ```typescript
  type RolePermissions = Record<"admin" | "user", string>;
  ```
- **`Exclude<T, U>`**: Removes types from a union.
  ```typescript
  type StringOrNumber = string | number;
  type OnlyString = Exclude<StringOrNumber, number>;
  ```
- **`Extract<T, U>`**: Extracts common types from a union.
  ```typescript
  type ExtractedType = Extract<StringOrNumber, string>;
  ```
- **`NonNullable<T>`**: Removes `null` and `undefined` from a type.
  ```typescript
  type DefinedString = NonNullable<string | null | undefined>;
  ```
- **`ReturnType<T>`**: Gets the return type of a function.
  ```typescript
  function foo(): number { return 42; }
  type FooReturnType = ReturnType<typeof foo>;
  ```
- **`Parameters<T>`**: Gets the parameter types of a function.
  ```typescript
  type FooParams = Parameters<typeof foo>;
  ```
- **`ConstructorParameters<T>`**: Gets the constructor parameters of a class.
  ```typescript
  class Person { constructor(name: string, age: number) {} }
  type PersonParams = ConstructorParameters<typeof Person>;
  ```

## 5. Advanced Type Features
- **Type Assertions** (Casting)
  ```typescript
  let someValue: any = "hello";
  let strLength: number = (someValue as string).length;
  ```
- **Index Signatures**
  ```typescript
  type Dictionary = { [key: string]: number };
  ```
- **Mapped Types**
  ```typescript
  type Optional<T> = { [K in keyof T]?: T[K] };
  ```
- **Conditional Types**
  ```typescript
  type Check<T> = T extends string ? "String" : "Not a string";
  ```
- **Infer Keyword**
  ```typescript
  type ReturnTypeInfer<T> = T extends (...args: any[]) => infer R ? R : never;
  ```

---


