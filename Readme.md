# Video Tutorial

> https://www.youtube.com/watch?v=BwuLxPH8IDs

> https://pro.academind.com/courses/enrolled/762406

# Official TypeScript Website

> https://www.typescriptlang.org/

> Basic Types: https://www.typescriptlang.org/docs/handbook/2/everyday-types.html

# Useful Commands for CLI and Compiling

- automate reloading live server site when I change + compile file:
  - type "npm init" in CLI (to be able to install usefull third party packages)
  - npm i --save-dev lite-server ("--save-dev" to mark it at a development only tool - in package.json writen under devDependencies -, that helps during dev phase; lite-server is smth like "nodemon")
  - add "scripts": { "start": "lite-server" } to package.json
- compile 1 file: use "tsc app.ts" in CLI; then in case all compiling errors are shown in the console
- watch 1 file AND compile it after every saved change: use watch mode "tsc app.ts --watch" or "tsc app.ts -w"
- compile entire project: first "tsc --init" to initialize a TS managed project; creates tsconfig.json; then type only "tsc" or "tsc -w" in CLI to compile every ts file or to watch all changes
- tsconfig.json: can exclude files from being compiled or include files

  - include files: then have to list after `"compilerOptions": { }` all(!) files or folders that should be compiled;
  - exclude + include files: when add both then `compilation = included files/folders - excluded files/-folders`

  ```JSON
    "compilerOptions": {
      "sourceMap": true, // app.js.map files are created while compilation; so in browser in source area I can see ts code for better debugging
      "outDir": "./dist", // output of compiled ts files (so js files) is found in this folder; also folder structure is replicated automatically
      "rootDir": "./src", // TS only checks this folder to compile files and would replicate this folder structure (with all subfolders) --> without defining this, TS compiles every ts file found in project and replicates all found structure in outDir folder
      "removeComments": true, // to make output js files smaller
      "noEmitOnError": false, // default is false; so TS creates js files even if there is an error; if set to true, then no file is emitted if any file fails to compile

    }
    "exclude": [
      "node_modules", // if I exclude no other files, then "node_modules" is by default excluded; when I add other exclusions, then I have to list "node_modules" to exclude all files inside node_modules folder
      "a-using-ts.ts",
      "*.ts", // * equal wildcard, now all files with ending .ts are excluded
      "**/*.ts" // * now every file with ending .ts in every order is excluded
    ],
    "include": [
      "app.ts", // file name
      "components" // folder
    ]
  ```

# Primitive Types and Reference Types

- Primitives: const, let, var, strings, booleans, undefined, null
- Reference Types: objects, arrays
- Difference:

  - related to memory management: JS knows two types of memory: Stack and Heap
  - stack: easy-to-access memory that simply manages its items as a stack. Only items for which the size is known in advance can go onto the stack. This is the case for numbers, strings, booleans.
  - heap: memory for items of which you can’t pre-determine the exact size and structure. Since objects and arrays can be mutated and change at runtime, they have to go into the heap therefore.
  - for each heap item, the exact address is stored in a pointer which points at the item in the heap. This pointer in turn is stored on the stack.

  ```JavaScript
  let person = { name: 'Matchu' }
  let newPerson = person
  newPerson.name = 'Pitchu'
  // This prints 'Pitchu' because I never copied person obj itself to newPerson
  // only copied the pointer; it points still at the same address in memory
  console.log(person.name)
  ```

# Core Types in TypeScript

TypeScript's type system only helps during development (e.g. before code gets compiled)

- number: no differentiation btw integers, floats ...: 1, 5.3, -10 is possible
- string: 'T', "T", `T`
- boolean: true, false (attention: no "truthy" or "falsy" values)
- object: { age: 30 }
- array: [1, 2, 3]
- Tuple: [1, 2] - fixed-length array; definition would be i.e. role: [number, string]
- Enum: enum { NEW, OLD } - only exists in TS, not in JS; automatically enumerated global constant identifiers; so when I need identifiers that are human readable

  ```TypeScript
  enum Role { ADMIN, READ_ONLY, AUTHOR };
  ```

- any: \* - any kind of value, no specific type assignment; avoid it because it gives away all advantages of TS
- unknown: is the type-safe counterpart of any. Anything is assignable to unknown, but unknown isn't assignable to anything but itself and any without a type assertion or a control flow based narrowing (siehe example in coding file)
- void: void can be declared as the return type of a function, that means that function has no return statement; example:

  ```TypeScript
  function add(num: number): void { console.log(num)};
  ```

- never Type: indicates the values that will never occur. The never type is used when you are sure that something is never going to occur. For example, you write a function which will not return to its end point or always throws an exception. example:

  ```TypeScript
  const generateError = (message: string, code: number): never => {
    throw { message: message, errorCode: code };
  }
  ```

- union types: defining more than one data type for a variable or a function parameter

  ```TypeScript
  input: number | string;
  ```

- literal types: There are three sets of literal types available in TS: strings, numbers, and booleans; by using literal types you can allow an exact value which a string, number, or boolean must have

  ```TypeScript
  resultConversation: 'as-number' | 'as-text';
  ```

- type alias: can store types like union or literal type(s) in a custom named type that I can use everywhere in my code; Type aliases are sometimes similar to interfaces, but can name primitives, unions, tuples, and any other types that you'd otherwise have to write by hand. Aliasing doesn't actually create a new type - it creates a new name to refer to that type.

  ```TypeScript
  type Combinable = number | string;
  let input: Combinable = 5;
  ```

- Function Return Types: define the return type of a function; example:

  ```TypeScript
  function add(num: number): number { }
  ```

- Function Types: define type(s) of parameters of function and type of return of function; so only function which fulfills types can be stored in a variable; example: ,

  ```TypeScript
  let combineValues: (a: number, b: number) => number
  ```

- Function Types and Callbacks: define in parameters of a function the type of a callback function; example:

  ```TypeScript
  const addAndHandle = (n1: number, n2: number, cb: (num: number) => void) => {
    const result = n1 + n2;
    cb(result);
  }
  ```

- ! - exclamation mark tells TS that 'testBtn' exists and so that variable `const testBtn` is never null

  ```TypeScript
  const testBtn = document.getElementById('testBtn')!;
  ```

- !! - ensures the resulting type is a boolean (true or false)

  ```TypeScript
  isValid = isValid && !!obj[prop];
  ```

# Classes and TypeScript - Summary of TS file

> More on (JS) Classes: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes

- define properties, methods and constructor
- "this" keyword to refer to the object itself
- creation of new instance of a class (-> a new obj based on a class)
- "private", "protected" and "public" (is the default) Access Modifiers: mark properties and methods with these keywords
  - "private": means that is only accessible from inside the class / from inside the created object
  - "protected": means like "private" + also in any class that "extends" this class
  - "public": I can access it from outside and manipulate it
- Shorthand Initialization: in order to not repeat, don't need property creation (f.e. "name: string", "id: string") AND "this.name = name", "this.id = id" in constructor function; only need private or public and wished variable names as parameters in constructor function
- "readonly" properties: not allowed to change after initialization
- inheritance: inherit main class to another subtype class using "extends" keyword;
- overriding properties and methods of base class: possible in every subtype class with new declaration of properties or methods
- getters & setters
- static properties & methods with "static" keyword: allows you to add properties & methods to classes which are not accessed on an instance of the class, so I don't need to call first new ClassName and save this in a constant; I access static methods & properties directly on the class; Example: Math constructor function or globally available function
- abstract classes: usefull if I want to enforce that all classes based on one class share some common methods and properties;
- singletons & private constructors: to make sure that I can only create one obj based on this class

# Interfaces - Summary of TS file

> More on TS Interfaces: https://www.typescriptlang.org/docs/handbook/interfaces.html

- interfaces describes how an object looks like (-> it's like a custom type to type check an obj later)

  ```TypeScript
  interface InterfaceName {
    propertyName: string;
    functionName(parameter1: number, parameter2: boolean): number | void;
  }
  ```

- using interfaces with classes: to share the structure of functionalities among different classes -> to enforce that the property and function structure is at least inside a certain class, but of course more can also be added in the respective class;

  ```TypeScript
  class ClassName implements FirstInterface, AnotherInterface {
  ```

- extending interfaces: combine interfaces with "extends" like for classes; extend with more than one interface with comma separation

  ```TypeScript
  interface FirstInterface extends AnotherInterface {
    // wished structure
  }
  ```

- interfaces as an alternative to function types

  - function type definition:

  ```TypeScript
  type AddFn = (a: number, b: number) => number;
  ```

  - interface definition with anonymous function

  ```TypeScript
  interface AddFn {
    (a: number, b: number): number;
  }
  ```

- optional parameters & properties: use question mark (?) to tell TS that property might exist BUT it doesn't have to

  ```TypeScript
  interface FirstInterface {
    readonly property?: string;
  }
  ```

# Advanced Types

> More on Advanced Types: https://www.typescriptlang.org/docs/handbook/advanced-types.html

- Intersection Types with "&" sign

  - combining object types: put all properties together in combined object type

  ```TypeScript
    type Admin = { name: string };
    type Employee = { startDate: Date };
    type ElevatedEmployee = Admin & Employee;
  ```

  - intersection of union types: "Universal" is of type number

  ```TypeScript
    type Combination = string | number;
    type Numeric = number | boolean;
    type Universal = Combination & Numeric;
  ```

- Type Guards: approach of checking if a certain property or method exists before you use it

  ```TypeScript
  function add7(a: Combinable, b: Combinable) {
    if(typeof a === 'string' || typeof b === 'string') return a.toString() + b.toString();
    return a + b;
  }
  ```

  - use key (as a string) in object to check if this key is in obj

  ```TypeScript
  const printEmployeeInformation = (emp: UnknownEmployee) => {
    if('privileges' in emp) console.log('Privileges: ' + emp.privileges)
  }
  ```

  - type guard for classes with JavaScript keyword "instanceof" to check if some obj is based on a certain class

  ```TypeScript
  if(vehicle instanceof Truck) {
    vehicle.loadCargo(1000)
  }
  ```

- Discriminated Unions: is a pattern that makes implementing type guards easier; define a property of a literal type (f.e. type: 'bird') that describes/determines exactly this object

  ```TypeScript
  interface Bird {
    type: 'bird';
    flyingSpeed: number;
  }
  ```

- Type Casting: helps to tell TS that some value is of a specific type where TS is not able to detect it on his own, but I as a dev know it; use case: selecting DOM element because TS doesn't analyse html document -> so only knows often that's an HTMLElement, but not which exact type (f.e. an HTMLInputElement)

  - with "<" + ElementName + ">"

  ```TypeScript
  const userInputElement = <HTMLInputElement>document.getElementById('user-input')!;
  ```

  - for React: "as" keyword to avoid clash to similar JSX syntax in React

  ```TypeScript
  const userInputElement = document.getElementById('user-input')! as HTMLInputElement;
  ```

- Index Types with `[key: typeName]`: to have flexibility that I don't need to know in advance which property names I wanna use AND how many I will need;

  ```TypeScript
  interface ErrorContainer {
    [key: string]: string;
  }
  ```

- Function Overloads: to tell TS what's the exact return if arguments are of this or this type; only works with function declaration, not function expression

  ```TypeScript
  function add7(a: number, b: number): number;
  function add7(a: string, b: string): string;
  function add7(a: string, b: number): string;
  function add7(a: Combinable, b: Combinable) {
    // function logic here
  }
  ```

- Optional Chaining: use question mark (?), that means if data exists access next property; if data in front of ? is undefined, then it will not access the property after AND not throw a runtime error

  ```TypeScript
  console.log(fetchedUserData?.job?.title)
  ```

- Nullish Coalescing Operator "??": if value before is null OR undefined (Attention: NOT empty string, NOT zero), then fallback after is used

  ```TypeScript
  const storedData = userInput2 ?? 'DEFAULT';
  ```

# Generic Types

- gives flexibility: regarding the values I pass in a function or I use in a class
- combined with type safety: I get full type support what I do with the class or the result of a generic function

- Built-in Generics are main types (like Array, Object, Promise ...) that allow a variety of data types rather than a single data type; generic type parameter is specified in angle brackets `const names: Array<string> = ['Matchu']`

- Generic Functions: thanks to generic definition TS infers dynamically the types of the arguments used in the function AND TS knows that return of function is the intersection of T & U;

  - T, U and so on in alphabetical order is a convention
  - Working with Constraints: keyword "extends" restricts the dynamically set types of T and U to be objects
    ```TypeScript
    const merge = <T extends object, U extends object>(objA: T, objB: U) => Object.assign(objA, objB);
    ```
  - use an interface to indicate TS that every element based on it has a certain property; can also precise the return of this function
    ```TypeScript
    interface Lenghty {
      length: number
    }
    const countAndDescribe = <T extends Lenghty>(element: T): [T, string] => {
      // function logic
    }
    ```
  - "keyof" Constraint: use keyword "keyof" to ensure that key exists in object
    ```TypeScript
    const extractAndConvert = <T extends object, U extends keyof T>(obj: T, key: U) => {
      return console.log(`Value: ${obj[key]}`);
    }
    ```

- Generic Classes: use generic type T and use it for property "data" that this will be and Array (-> T[])

  ```TypeScript
  class DataStorage<T extends string | number | boolean> {
    private data: T[] = [];
  }
  const textStorage = new DataStorage<string>();
  const numberStorage = new DataStorage<number>();
  // ...
  ```

- Generic Utility Types:

  > handy built-in utility types: https://www.typescriptlang.org/docs/handbook/utility-types.html

  - Partial Type: wraps my own created type / interface and sets all properties as optional

    ```TypeScript
    interface Course {
      title: string;
    }
    const createCourse = (title: string) => {
      let courseGoal: Partial<Course> = {};
      courseGoal.title = title;
      return courseGoal;
    }
    ```

  - Readonly Type: cannot manipulate variable, obj, array after creation

    ```TypeScript
    const names2: Readonly<string[]> = ['Matchu', 'Mio'];
    ```

- Generic Types vs Union Types:
  - Union types: defines f.e. in case of "(string | number | boolean)[]" that I'm free to use all these types for the items of array
  - Generic Types: defines dynamically a type f.e. depending on inserted argument type, BUT then locks in this type for entire function or class (-> f.e. string is set internally)

# Decoractors

> More on Decorators: https://www.typescriptlang.org/docs/handbook/decorators.html

- a decorator is a special kind of declaration that can be attached to a class declaration, method, accessor, property or parameter
- set "experimentalDecorators": true in tsconfig.json to be able to use decorators
- decorator runs when class definition is found (NOT when class is instantiated)

- Decorator function

  - convention to start with uppercase starting letter
  - decorator to a class has 1 parameter (-> constructor)
  - to invoke decorator function add the identifier "@" and the function name (-> @Logger)

    ```TypeScript
    const Logger = (constructor: Function) => console.log(constructor);

    @Logger
    class Person {
      // ...
    }
    ```

- Decorator factories

  - return decorator function, so I can invoke it with @Logger(argument) and pass this argument to the inner decorator function

  ```TypeScript
  const Logger = (log: string) => {
    return function(constructor: Function) {
      console.log('LogString: ', log);
      console.log('Constructor: ', constructor);
    }
  }

  @Logger('Hello')
  class Person {
    // ...
  }
  ```

- Building More Useful Decorators with Templates: look at detailed example in decorators file

  - hook into html element and insert own html, dynamic data etc.

    ```TypeScript
    const WithTemplate = (template: string, hookId: string) => {
      // to tell TS that I don't need constructor here, add underscore as parameter (_)
      return function(_: Function) {
      const hookEl = document.getElementById(hookId);
      if(hookEl) {
        hookEl.innerHTML = template;
      }
    }

    @WithTemplate('<h1>Hello</h1>', 'decoratorId')
    class Person {
      // ...
    }
    ```

- Adding Multiple Decorators:

  - execution of decorator functions is bottom up;
  - BUT: regarding the decorator factories, so the normal invocation with @Logger etc., they run top down
    ```TypeScript
    @Logger
    @WithTemplate('<h1>Hello</h1>', 'decoratorId')
    class Person {
      // ...
    }
    ```

- Property decorators takes 2 arguments:

  - target of the property (-> if called on an instance, then it's prototype of obj that was created; -> if static property, target refers to constructor function);
  - property name
    ```TypeScript
    const Log = (target: any, propName: string | Symbol) => {
      console.log('Property decorator: ', target, propName);
    }
    ```

- Accessor (setters, getters) decorators takes 3 arguments:

  - target of the property (look above)
  - name of accessor
  - descriptor with TS built-in type PropertyDescriptor
    ```TypeScript
    const Log2 = (target: any, name: string, descriptor: PropertyDescriptor) => {
      console.log('Accessor decorator: ', target, name, descriptor)
    }
    ```

- Method decorators takes 3 arguments: same as for accessor decorators

  ```TypeScript
  const Log3 = (target: any, name: string | Symbol, descriptor: PropertyDescriptor) => {
    console.log('Method decorator: ', target, name, descriptor)
  }
  ```

- parameter decorators takes 3 arguments

  ```TypeScript
  const Log4 = (target: any, name: string | Symbol, position: number) => {
    console.log('Parameter decorator: ', target, name, position)
  }
  ```

  ```TypeScript
  class Product {
    @Log
    title: string;
    private _price: number;

    @Log2
    set price(val: number) {
      if(val > 0) this._price = val;
        else throw new Error('Invalid price - should be positive')
    }

    constructor(t: string, p: number) {
      this.title = t;
      this._price = p;
    }

    @Log3
    getPriceWithTax(@Log4 tax: number) {
      return this._price * (1 + tax);
    }
  }
  ```

- Returning (and changing) a Class in a Class Decorator

  - returning a new constructor function (with syntatic sugar "class extends constructor") which is based on the original constructor (-> I keep all properties of original class)

- Other Decorator Return Types: return inside of Decorator functions (Attention: NOT decorator factories, they return always of course) is possible on accessor and method decorators; can return a new PropertyDescriptor

- Data Validation with Decorators:
  - look at example in file
  - recommanded npm class-validator package is f.e. availabe here: https://www.npmjs.com/package/class-validator
