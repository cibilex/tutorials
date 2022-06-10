# Classes
## **Inheritance** :
```javascript
class ChildClass extends ParentClass  
```
```javascript
class Animal{
  constructor(name){
    this.speed=0
    this.name=name
  }
  run(speed){
    this.speed=speed
    console.log(`${this.name} is runnig with speed ${this.speed} h/s`)
  }
  stop(){
    this.speed=0;
    console.log(`${this.name} stopped,ok by why ?`)
  }
}
class Rabbit extends Animal{
  hide(){
    console.log(this.name+"hided")
  }
}
const animal=new Animal("rabbit")
const rabbit=new Rabbit("rabbit")
```
Rabbit class have all properties of Animal class.Also rabbit class have hide() property.

#### rabbit structure:
own properties=>Rabbit prototype =>Animal prototype

When we call a property:
1. is searched in own object  
1. after  is searched on object prototype  
1. after  is searched on object prototype of prototype

```javascript
Array.prototype.join=function(){console.log("hi,I went to vacation.Call me later :)")}
new Array("21",21).join() 
// output: hi,I went to vacation.Call me later :)
```

### super keywoard
super() : call the parent constructor  
super.method() : call the parent method function.It is easy to use.  
```javascript
class Rabbit extends Animal{
  hide(){
    super.stop()
    console.log(this.name+"hided")
  }
}
const rabbit=new Rabbit("rabbit")
rabbit.hide()
// output: 
// rabbit stopped,ok by why ? 
// rabbithided
```
#### super() :
When we extend a class from another class:
extended class will have a "derived constructor".The means is that Parent class must be create constructor.Child  class itself can not create constructor.we need the "super" method to do it.
-Use super() before using "this". 

```javascript
class Rabbit extends Animal{
  constructor(name,feet){
   this.name=name,
    this.feet=feet
  }} // ust call super constructor in derived class before accessing 'this' or returning from derived constructor
class Rabbit extends Animal{
  constructor(name,feet){
    super(name),
    this.feet=feet
  }} 
```
### the parent constructor always uses the parent field

## Static properties and Methods

  These are assign to own functions of class.these are not assign to Class.property
  ```javascript
      class User{
  static sayHi(){
    console.log("hi")
  }
}
User.sayHi() // hi
  ``` 
  or 
  ```javascript
        class User{}
        User.sayHi(){
    console.log("hi")
  }
User.sayHi() // hi
  ``` 
  #### to use for default values:
  ```javascript
    class User{
  constructor(username,surname){
    this.username=username;
    this.surname=surname
  }
  static defaultUser(){
    return new this("mehmet","ilhan")
  }
}
const defaultUser=User.defaultUser()
console.log(defaultUser.username) //"mehmet"
```
### Static properties
```javascript
class Article {
  static publisher = "Ilya Kantor";
}
alert( Article.publisher ); // Ilya Kantor
```

#### static methods and properties are inherited  
## Typescript on Classess  

 ### Fields
 ```javascript
 class User{
    username:string;
    age:number;
}
const user=new User()
user.age=21 // ok
user.age="213" //err
 ```

 we can also have initializers.Like:
 ```javascript
 class User{
    username="ahmet";
    age=21;
}
const user=new User()
console.log(user.age) // 21
 ```

 #### **strictPropertyInitialization**

 When we type a field on class and it doesn't have a initializer or doesn't provide a value in constructor.This command will throw an error.
 ```javascript
 class User{
    age:number;  //err
    username:string;
    surname="ahmet"
    constructor(){
        this.username="ahmet"
    }
}
 ```

 ```javascript
 class OKGreeter {
  // Not initialized, but no error
  name!: string;
}
 ```

### readonly
When we use "readonly" for a property. We only can  change that property  in constructor.
```javascript
class User{
  readonly username:string="ahmet" ; //string
    constructor(username:string){
        this.username=username
    }
    change(){
      this.username="asd" //err 
    }
}
const user=new User()
user.username="as" //err
```

### Constructor 
Constructor is similar to functions.

```javascript
class User{
x:string;
y:string  
    constructor(x="x",y="y"){ //default values x:string,y:string
      this.x=x;
      this.y=y;
    }
}
```

```javascript
class User{
x:string;
y:string;
    constructor(x:string,y:string){
    this.x=x;
    this.y=y;
    }
}
```

constructor signature -function signature differents:  
1. constructor doesn't have a return value so doesn't have a return type.
2. constructor can't has a type parameters.

### Methods:
"methods" can use all function and constructor properties.
```javascript
class User{
  x=10;
  y=10;
  scale(rate:number){
    this.x*=rate;
    this.y*=rate;
  }
}
```

### Getters -Setters:

When we want to create protected  property in a class.We can use them  . [example](https://www.youtube.com/watch?v=LFyOw46jfI0&t=45s&ab_channel=sanalonyedi "örnek")

```javascript
class User{
  _x=10;
  
get x():number{return this._x}
set x(val){this._x=val}
}
```
Note:If You get this error.Type "-t es5" on command.
 " Accessors are only available when targeting ECMAScript 5 and higher."

 Typescript act the getters and setters a bit different.
1. If setter parameter doesn't have a type.Its type become  getters return type.
2. If getter is exist but setter is not exist.Parameter used be readonly automatically.
  
### Index signature:
The same as index signature for object.
```javascript
class User{
  [prop: string]:string | ((x:string)=>string);
}
```

### Implement Clauses
thanks to Implement clause,We can control all properties,functions on a class.When class not equal to Interface ,typescript throws an error.
```javascript
interface UserTemplate{
  username:string,
  changeUsername:()=>void
}
class User implements UserTemplate{
  username="ad";
  changeUsername() {
    console.log("asd");
  }
}
class User2 implements UserTemplate{
  //error
}
```

**Note** Also we can use multiple Interface like **_class User implements A,B_**

**Note** implement clause doesn't change the class type **!!**
### extends
- "derived class" must be a subtype of its base class for typescript.So we should always be able to call the base class method.

```javascript
class User{
  type(){
    console.log("hi user")
  }
}
class Admin extends User{
  
  type(x?:string){
   if(typeof x==="undefined")super.type()
   else console.log("hey"+x)
  }
}
class Admin2 extends User{
  type(x:string){ //err
    console.log("hi"+x)
  }
}
```

### Member Visibility

1. public  
     By default, all properties and methods have the "public" key. so we don't have to write.we can use them anywhere.
     ```javascript 
     class User{
   public username="alex"
   public typeUsername(){
    console.log(this.username)}
    const user=new User()
    user.typeUsername()// alex
    console.log(user.username) //alex
    ```

2. protected:The protected key provides that this property or method can only use in own class or subclasses.
```javascript
  class User{
    public username="alex"
  protected typeUsername(){
      console.log(this.username)
    }
    useTypeUsername(){
    this.typeUsername()//ok
    }
  }

  class Admin extends User{
    hey(){
      this.typeUsername()//ok
    }
  }

  const user=new User()
  user.typeUsername() //err
```
**changing "protected" as "public"**
```javascript
  class User{
    protected username="alex"
  }
  class Admin extends User{

     username="ahmet"
  }
  const user=new Admin()
console.log(user.username) // ahmet

```
3. private :The private key ensures that this property or method is only used in its own class. It cannot be used in subclasses or outside the own class.
```javascript
  class User{
   private username="alex"
    hey(){
      console.log(this.username) // alex
    }
  }

  class Admin extends User{
    hey2(){
      console.log(this.username) // err
    }
  }

  const user=new Admin()
console.log(user.username) // err

```
**Note**:we can't increase the private key like protected key
**Note**: We can get private and protected properties as obj["keyName"]
```javascript
  class User{
   private username="alex"
  }
  const user=new User()
console.log(user["username"]) // alex
```
**Note**: Unlike TypeScript, Javascript has a symbol to hide properties. "#". This symbol makes them hard private
```javascript
class User{
    #username="alex"
    hey(){
        console.log(this.#username)
    }
   }
 
   const user=new User()
   user.hey()//alex
   console.log(user.#username)   //err
```

### Static Members:
We can use member visibility or readonly keyword with static members.
```javascript
class Clock{
 static readonly  timer:number | null;
 protected static time=0;
static hey(){console.log("hey")}
}
Clock.hey()
```
We can't use "name","call" or "length" e.c. property or method name because These are can be propery or method of the Function .
```javascript
class Clock{
static name="1" // err
static length(){console.log("hi")} //err
}
```

## Generic Classes

Generic classes are similar to interface.We can using constraint operation like interface.
```javascript
    class Foo<T>{
        username:T;
        constructor(username:T){
            this.username=username
        }
        hey(x:T):T{
            return x
        }
    }
    const user=new Foo("ahmet") // user:Foo<string>
```
--We can't use generics with static members.

### This at runtime in classes
 
Let's explain it with an instance.
```javascript
class User{
    username="alex"
    age=21
    getUserInfos(){return this.username+" "+this.age}
}
const user=new User()

const userObj={
   infos:user.getUserInfos
}
console.log(userObj.infos()) // undefined undefined
```
**Note** If you call the method at risky places.Use arrow function  in class
```javascript
class User{
    username="alex"
    age=21
    getUserInfos=()=> this.username+" "+this.age
}
const user=new User()

const userObj={
   infos:user.getUserInfos
}
console.log(userObj.infos()) // alex 21
```
disavantage of this method:
- This will use more memory, because each class instance will have its own copy of each function defined this way
- We can't use super.method() at subclasses.Because arrow function will not exist on prototype chain.

### this type
The special type called"this" refers to current class type as dynamically.
```javascript
class Box {
    content: string = "";
    sameAs(other: this) {
      return other.content === this.content;
    }
  }
  class MyBox extends Box{
      myContent:string="box"
  }
  const box=new Box() //Box
  const mybox=new MyBox() //MyBox
  mybox.sameAs(box) //Argument of type 'Box' is not assignable to parameter of type 'MyBox'.
```

### Parameter Properties-Special Consturctor syntax
If The key name the same as parameter name,We can use typescript special constructor syntax.We may use "readonly","protected","private","public" keys with this syntax.
```javascript
class User{
    constructor(
     public   username:string,
     public   surname:string,
     public   age:number,
     protected readonly privateKey:string
    ){}
}
const user=new User("21","21",21,"1")
```

### Class Expressions
Class expressions are very similar to class declarations.Fundamental difference is that class expresssions don't need a class name.
```javascript
const User=class<T>{
  constructor(
    public user:T
  ){}
}
const user=new User("hey")
```

### Abstract classes-Members
- an abstract class use to say that how must seem to subclasses.
- we can not generate a class from abstract class.
- If we don't type all abstract property and methods,typescript throws an error.
```javascript
abstract class User{
    constructor(
        public username:string
    ){}
    abstract age:number
    abstract writeMe(x:string):boolean
}
class Admin extends User{
    age=21;
    writeMe(x:string){
        return true
    }
}
```

### Abstract Construct Signatures
Sometimes you want to accept some class constructor function that produces an instance of a class which derives from some abstract class.you want to write a function that accepts something with a construct signature.For this:

```javascript
abstract class User{
    abstract username:string
}
class MyUser {
    createUser(x:new ()=>User){ //thanks to "()=>User".User itself can't be the parameter but children of User class can be parameter.

    }
}
const myUser=new MyUser()
class UserClass extends User{
    username="ahmet"
}
// myUser.createUser(User)
myUser.createUser(UserClass)
```

## Modules:
for typescript:
1. command file : If export or top level await is not include in the file.Members of this file are global member.
2. module: If export or top level await is exist in the file.Members of this file are not global member.We have to export them for reaching.

**export default member** :  the member represent the main export member.
   ```typescript
   //a.ts
      export default function hi(){
        console.log("hi default export")
    }
   ```

   ```typescript 
   // app.vue
      import hi from "./a"
    hi()
   ```

**export** :to export multiple member in a file.
   ```typescript
   //a.ts
export const username="ahmet"
export function hi(){
    console.log("hi export")
}
   ```

   ```typescript 
   // app.vue
import { hi, username } from './a';

hi()
console.log(username)
   ```

**Note** : we can use main export member and others together . 
   ```typescript
    import ahmetFn,{mehmetFn} from './a';
   ```
**changing member name**: import {oldName as newName} from "url"  

 **namescape**: we can put all members to in an object. import * as main from "url" 
   ```typescript
  //a.ts
    export const username="ahmet"
    export function mehmetFn(){
        console.log("mehmet")
    }
    export default function ahmetFn(){
        console.log("ahmet")
    }
   ```

   ```typescript 
   // app.vue
    import * as a from './a';

    a.mehmetFn() // mehmetFn function
    a.default() // main export .ahmetFn
   ```
**import type** : this method gets the file as a type file. Members can only be used as types.
   ```typescript
        //a.ts
        export type Username=string |number
        export const username="ahmet"
  ```

   ```typescript 
    // app.vue
      import type { Username, username } from './a';
      type User=Username | boolean  // string | number | boolean
      console.log(username) // 'username' cannot be used as a value because it was imported using 'import type'
   ```
  
## tsconfig.json:
For recommended configuration : [@tsconfig/recommended](https://www.npmjs.com/package/@tsconfig/recommended)  
```
tsc --init 
```   
this command create a tsconfig.json file and explain all options.    
**Note** For best experience of typescript type checking use all options of  inside type-cheking
**Note** ***_tsc --w_** :watch typescript files.
- this file is root file of typescript project.
-  For Javascript projects we can use "jsconfig.json" instead of "tsconfig.json".Some flags are enabled as default in "jsconfig.json"
- as default,typescript start to compile files from tsconfig.json directory and compiles all child directories.

Fundamental options:
```json
{
    "compilerOptions": {
        "target":"ES5",
        "outDir": "./public",
        "rootDir": "./src"
    }
}
```

- **target**:   in compilerOptions
   - All moden browsers support the ES6 features so our choise can be it.For stable features choose the "ES5".  
- **rootDir** in compilerOptions
   - "rootDir" option chooses default director to start compiling files.
   - rootDir does not decide which files to compile.For this operation we use "include","exclude","files" options.
- **outDir** in compilerOptions
   - choose out directory.  
   - If not specified,The files compiled own place.
- **Include** top
   - Specifies an array of filenames or patterns to include in the program. These filenames are resolved relative to the directory containing the tsconfig.json file.
   - include and exclude support wildcard characters.
       -  * matches zero or more characters
       - ? matches any one character
       - **/  matches any directory nested to any level
- **exclude**  top
      - Specifies an array of filenames or patterns that should be skipped when resolving include.
      ```json
          {"include": ["src"],
         "exclude": ["node_modules"]}
      ```
- **extends**  top
    - The value of extends is a string which contains a path to another configuration file to inherit from
    ```json
    {
      "extends": "./extendFrom.json"
    }
    ```
- **declaration** 
    - for example,we have x.ts and we use "declaration":true in tsconfig.json.
    - typescript will create x.d.ts and x.js in the same folder.
- **noEmitOnError** 
     - if we have an error(or errors) ts file won't transform to js file.
- **noImplicitAny**     
    - if we haven't typed a type  at any variable or parameter ,typescript will throw an error.
- **strictNullChecks**     
   - Typescript consider **null** and **undefined** values.
- **strictPropertyInitialization**      
    - If we declare a property in class and we don't assign a value for it .typescript will throw an error.
- **files**      
    - Specifies an allowlist of files to include in the program. An error occurs if any of the files can’t be found.   
         - ```json 
           {"files": [
                 "core.ts",
                 "sys.ts",
                 "types.ts " ]  }     
            ```
- **allowJs** : provides import .js files. (Boolean)
- **module** : output module format.For nodejs application use "CommonJS" ."ESNext" for modern Ecmascript module system.
- **moduleResolution**  : 'node' for commonjs. 'node12' or 'nodenext' for Ecmascript module system


## Notes
-  ***no has default export** : add each component
 - ```typescript 
    <script lang="ts">

    export default defineComponent({

    })
    </script>
   ```
- ** is declared but never used**
   - way 1: write  **//@ts-ignore** before  an error line : for single errors.
   - way 2: in tsconfig.json compilerOptions>"noUnusedLocals": true

**LOOK AT THIS PAGE** : https://www.typescriptlang.org/docs/handbook/namespaces.html