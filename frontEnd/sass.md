# Sass

**Note** : slide text to move right : shift+tab     

**Note** : Slide text to move left : tab     

**Note** : _ and - are equal for a sass identifier.   
## **Syntaxes**
1. **Scss**:This syntax is similar to css.
```css
@mixin button-base() {
  @include typography(button);
  @include ripple-surface;
  @include ripple-radius-bounded;

  display: inline-flex;
  position: relative;
  height: $button-height;
  border: none;
  vertical-align: middle;

  &:hover { cursor: pointer; }

  &:disabled {
    color: $mdc-button-disabled-ink-color;
    cursor: default;
    pointer-events: none;
  }
}
```
2. Sass:
```css
@mixin button-base()
  @include typography(button)
  @include ripple-surface
  @include ripple-radius-bounded

  display: inline-flex
  position: relative
  height: $button-height
  border: none
  vertical-align: middle

  &:hover
    cursor: pointer

  &:disabled
    color: $mdc-button-disabled-ink-color
    cursor: default
    pointer-events: none
```

## **Preprocessing**.
Sass files can not used directly.We have to compile them to css files.
  ```bash
  npm install sass -g
  ```

compiling one-to-one file: 
  ```bash
  sass rootFile outFile
  ```
compiling many-to-many files:
  ```bash
   sass example/colors.scss:dist/colors.scss example/font.scss:src/font.scss 
  ```

  compiling directory:
  ```bash
  sass rootDir:outDir
  ```

  **_Note_** : We can watch changes to re-compile. use '--watch' flag for that.

## **Comments**
1. Silent Comments
     - // It will never compiled.
2. Loud comments 
     - /* */ :It will compile if sass use with compress mode.
     - /*! */ : It will always compiled.


## **Nesting**
```css
div p{
        color:red;
    }

div  span{
        color:yellow;
    }
 
```
Instead this code we can type that code 🤣
```scss
div
    p{
        color:red;
    }
    span{
        color:yellow;
    }
 
```
- with combinators
## **CSS custom properties**
- Declaring a variable without sass
- These variables are  can used in javascript.

```scss
:root{
    --myColor:red;
}

div p{
    color:var(--myColor)
}
```

## **Property expression**:
```scss
div p{
    color:Red;
    font: {
        family: arial;
        size: 20px;
        weight: 100;
    }
}   
```

## **if(condition,trueVal,falseVal)**
```scss
$condition:false;
div p{
    color:if($condition,red,yellow)
}   
```

**Note** If property value is null,sass doesn't compile it.

## **Parent Selector**
- It is used in nesting
- The & symbol corresponds to the outer selector.
```scss
  div{
        color:red;
        &:hover{
            color:green;
        }
        & p{
           background: green;
        }
    }
    //is equal to
    
div{
    color:red;
}
div:hover{
    color:green;
}
div p{
    background: green;
}
```

## Placeholder selector:
- It won't compile to css.
- Unlike classes,don't have to use it.
```scss
%commonProps{
    color:Red;
    font-size:20px;
}
p{
    @extend %commonProps;
    background: green;
}
div p{
    background: yellow;
}
```

## **Interpolation**
- We can use variables in special spaces like selector names,properties,custom properties,plain css imports.   
- use variable in #{}          
- An interpolation is always return string value.
```scss
$propName:"background";
$className:"h1-class";
div p{
    #{$propName}:Red
}
.div-#{$className}{
    color:green;
}
```

## Variables
- variable : expression
- A variable scope is block level 
```scss
$color:red ; //global variable
div p{
    $color:blue ; // local variable
    color:$color;
}

p{
    color: $color;
}

// is equal to

div p{
    color:blue;
}
p{
    color:red;
}
```
sass variables vs css variables
1. Sass variables are imperative, which means if you use a variable and then change its value, the earlier use will stay the same. CSS variables are declarative, which means if you change the value, it’ll affect both earlier uses and later uses.
2. sass variables are compiled by sass but css variables are exist on css output.


**Note** If  want to update variable in local scope,use !global flag. !global flag is just for update a variable,not for create a new one.
```scss
$color:red ; //global variable
div p{
    $color:blue !global ; // local variable
    color:$color;
}

p{
    color: $color;
}

// is equal to

div p{
    color:blue;
}
p{
    color:blue;
}
```

### Flow control scope:
Variables declared in flow control rules(while,for,each,if) have special scoping rules: they don’t shadow variables at the same level as the flow control rule. Instead, they just assign to those variables
```scss
SCSS SYNTAX
$dark-theme: true !default;
$primary-color: #f8bbd0 !default;
$accent-color: #6a1b9a !default;

@if $dark-theme {
  $primary-color: darken($primary-color, 60%);
  $accent-color: lighten($accent-color, 60%);
}

.button {
  background-color: $primary-color;
  border: 1px solid $accent-color;
  border-radius: 3px;
} 
//output is

CSS OUTPUT
.button {
  background-color: #750c30;
  border: 1px solid #f5ebfc;
  border-radius: 3px;
}
```

### Advanced Variable Functions
```scss
@use "sass:meta";
@debug meta.global-variable-exists("var1"); // is var1 exist in global scope
@debug meta.variable-exists("var1");  // is var1 exist in global or local scope.
```

### **Css Class Naming**
- We will use BEM approach.
1. Block: Parent elem.
     - .Name
     - In css use them alone
2. Element: Child of parent.use __ 
     - .Name__child
     - In css use them alone
3. Modifier: Modifier classes for state,behavior....Use -- 
     - In Parent :
         - .Name--modifier
         - In css use them alone
      - In Child :
         - .Name__child--modifier
         - In css use them with parent class
```html
    <div class="form form--disabled">
        <label for="username" class="form__label" > username : </label>
        <input type="text" id="username" class="form__input form__input--disabled" />
    </div>
```


## At-rules

1. ***@use*** : loads variables,functions,mixins,props...
      - ```scss
        // asd.css
          $color:white;
          *{
            background-color:green;
          }
        ```

         ```scss
        // index.css
          $color:white;
          *{
            background-color:green;
          }
        ```
        
        ```scss
        // output
                * {
                background-color: green;
                }

                div p {
                color: white;
                }

                p {
                color: blue;
                }
        ```
     - We can think that @use is similar to es6 import.to reach loaded file members  use <namescape>.<member> member is can be variable,function or mixin.    
         - by default namescape is module url without file ext name.
         - <b>@use "url" as namescape </b> for custom namescapes.
         - <b>@use "url" as *</b>  load module without namescape.we can use members directly.
            - ```scss
                    @use "./asd" as *;
                    div p{
                        color:$color
                    }
               ```
        - <b>Private members</b> Private members are not shared. For private members type - or _ as prefix. 
        - <b>@use "url" with (
            $variable:value,
            $variable2:value2
        )</b> for configure default values.
        - @use "x" is equal to "x.scss","x.sass" or "x.css"
        - don't use \ ,use / with paths.
        - **Partials** : when a file starts with _ or -, sass won't compile it.If you want to use a file only as module use them.
        - Index files  : @use "src" is equal to src/_index.scss or src/_index.sass
        - sass can use a plain css file as a module.
2. **@forward** : if we  load many files with @use rule,it will seem bad.Therefore we should load module files in a other file and load common file to main file.
     - @forward "url"
     -  when we get a module with @forward rule,we can use members directly.
     - ```scss
        // colors.scss
        *{
            color:blue;
        }
        //font.scss
        *{
          font: {
                size:50px;
                family:arial;
            }
        }
        //as.scss
        *{
            background-color: greenyellow;
        }
       ```
       ```scss
       //forward.scss
            @forward "as";
            @forward  "font";
            @forward  "colors";
       ```
       ```scss
       //index.scss
       @use "../example/forward"
       ```
    - **Adding a prefix** : `@forward "url" as  prefix -*`
    - **Member Visibility** : 
         - @forward "url" hide members   : hide these members
         - @forward "url" show members  : only use this members.  
    - **Configuring modules** : @forward "url" with ()  . Configure default values.
3. **Mixins** : mixins provides reusable styles.
     - creating : ***@mixin name{}*** or ***@mixin name(params){}**
     - calling : ***@include name*** or ***@include name(params)***
     ```scss
     @mixin offset($margin,$padding){
    margin:$margin;
    padding: $padding;
    }
    p{
        background-color: grey;
        @include offset(20px,40px)
    }
     ```
     
     - **Optional Arguments** : if we type default values  we don't have to give the parameters.
         - ```scss
            @mixin offset($margin,$padding:50px){
            margin:$margin;
            padding: $padding;
            }
            p{
                background-color: grey;
                @include offset(10px)
            }
            ```

     - **keyword arguments** we can pass arguments by names.
         ```scss
         @mixin offset($margin,$padding:50px){
        margin:$margin;
        padding: $padding;
        }
        p{
            background-color: grey;
            @include offset($padding:1px,$margin:20px)
        }
         ```
     - **Arbitrary arguments** : ...
         ```scss
         @mixin order($height, $selectors...) {
        @for $i from 0 to length($selectors) {
            #{nth($selectors, $i + 1)} {
            position: absolute;
            height: $height;
            margin-top: $i * $height;
            }
        }
            }

            @include order(150px, "input.name", "input.address", "input.zip");
         ```
     - **Content Block** :
      ```scss
       @include exampleMixin{
           // this props are content.we can use them through @content
       }
      ```
     - **alternative way to use mixins** :
         - @mixin  = =
         - @include  = +
         ```sass
            =bgColor
        background-color: red
            p
                +bgColor
         ```
4. **@function** : for complex operations. 
     - @return for return values
     - Each function must end with a @return
     - ```scss  
        @function min($numbers...)
        $min:0
        @each $number in $numbers
          $min:if($number<$min,$number)

        @debug min(21,32,21,321,21,223)
        ```
     - use @function instead of @mixin to update variable value
     - **arbitrary arguments** : ...
     - **Optional arguments** : use default values.In this way we won't have to provide all arguments.
          ```scss
          @function bgColor($percent:30)
            @return if($percent>50,yellow,green)

           div p
                background-color: bgColor()
          ```
     - **keyword arguments** : 
         ```scss
         @function bgColor($direction,$size)
            $con:$direction== left or $direction == right
            @return if($con,$size*5,$size)
        div p
            padding: bgColor($size:20,$direction:left) + px
           ```
5. **@extend** It takes content from another selector but not directly .It wants to some conditions.
     - @extend selector
     - the selector should be as simple as possible
     - @extends takes also effects like :hover.
     - If we type a selector but if the selector is not exist sass will throw an error.
     ```scss
        .quide
          color: red
        .quide:hover
            color: green

        .content nav.sidebar 
            @extend .quide
     ```
     ```scss
     .quide, .content nav.sidebar {
    color: red;
        }

        .quide:hover, .content nav.sidebar:hover {
        color: green;
        }
     ```
6. **@error** : sass stops compiling and throws error     
    ```scss
    $type:syntax
    @error "this is a  #{$type} error"
    ```
7. **@warn** : @warn writes message,@warn doesn't stop compile
8. **@debug** : to see expression or value on console
    ```scss
    @debug "hi bro what are u doing"
    ```

## **Flow Control**
1. @if-@else: .
     - @if :  If the condition is true, the block is evaluated.
     - @else : block works if the @if condition is false
     - @else if 
     - Empty strings, empty lists, and the number 0 are all truthy in Sass.
     -  ```scss
        =bgColor($theme)
            @if $theme== dark   
                background-color: green   
            @else if $theme == light
                background-color: grey
            @else if $theme == sunny
                background-color: yellow
            @else 
                background-color: white

        p
            +bgColor(sunny)
        ```
2. **@each** : @each runs the code for each item in list or each key-value pair in map.
     - in list: @each $item in  $list
     - in map : @each key,value in $map 
     - ```scss
        @mixin bgAnimation($colors...)
        animation: myAnimation 3s 
        $percent:0
        @keyframes myAnimation
                @each $color in $colors
                    #{$percent}%
                        background-color:$color
                    $percent:$percent+10
                100%
                    background-color: yellow

        p 
            @include bgAnimation(red,green,blue,orange,brown,grey)
        ```
3. **@for** : 
     - @for $i from x to y {}:  x-y times block  will evaluate
     - @for $i from x through y : x-y+1 times block will evaluate
     - ```scss
            @use "sass:list"

        @mixin bgAnimation($colors...)
        @for $x from 0 to length($colors)
            &:nth-child(#{$x})
                background-color: list.nth($colors,$x+1)
                
        p 
            @include bgAnimation(red,green,blue,orange,brown,grey)
            
          ```
4. **@while** : the block code works as long as the condition is true.
     - ```scss
            @mixin bgAnimation($colors...)
        $x:1
        @while $x < length($colors)
            $x:$x+1
            &:nth-child(#{$x})
                background-color: list.nth($colors,$x)
                
        p 
            @include bgAnimation(red,green,blue,orange,brown,grey)
        ```

**Note** : important example
```scss
p
  background-color: red
  @media screen and (min-width:300px) and (max-width:1000px)
    background-color: yellow
```
Output will be 
```scss
p {
  background-color: red;
}
@media screen and (min-width: 300px) and (max-width: 1000px) {
  p {
    background-color: yellow;
  }
}
```
## SASS VALUES
1. Numbers: 
     - with Units: 10px
     - Simple: 10
2. **Strings** :
     - quoted : "red"
     - unquoted : red
     - to turn into each other
         -  ```scss
            @use "sass:string";

            @debug string.unquote(".widget:hover"); // .widget:hover
            @debug string.quote(bold); // "bold"
            ```    

     - escape character /
     - new line character \a
     - "sass:string" 
     - ```scss      
                            @use "sass:string"
                            @debug  string.index("helver","l") // 3  index starts with 1
                            @debug string.quote(example) //example
                            @debug string.unquote("example") // example
                            @debug string.insert("hakan","example",2) // hexampleakan  string.insert(string,insertThat,index)
                            @debug string.insert("hakan","example",100) // hakanexample
                            @debug  string.insert("hakan","example",-100) // examplehakan
                            @debug string.length("hakan") // 5
                            @debug  string.length(hakan) // 5
                            @debug string.slice("hakan",2) // akan
                            @debug string.slice("hakan",1,4) // haka
                            @debug string.slice("hakan",1,-3) // hak
                            @debug string.to-upper-case("hakan") // HAKAN
                            @debug string.to-lower-case("HAKAN") // hakan
                            @debug string.unique-id() // random id
          ```
3. **Colors** : #RRGGBB , red, rgb(R,G,B) ,rgba(R,G,B,A) or hsl()
     - "sass:color" 
         - color.adjust($color,
           $red: null, $green: null, $blue: null,
           $hue: null, $saturation: null, $lightness: null,
           $whiteness: null, $blackness: null,
           $alpha: null)         
             - $red,$green,blue must be between -255 and 255.
             - $alpha : -1 -1
             - $whiteness,$blackness,$saturation,$lightness: -100%-100%
             - $hue : -360deg-360deg
        - color.change($color,props) : color.adjust($color,props) increases current values but color.change($color,props) updates values
        - ```scss
                @debug color.red(#2f2f2f)  // 47 red ratio
                @debug color.green(#3f3f3f) // 63 green ratio
                @debug color.blue(rgba(12,12,212)) //212s
                @debug color.saturation(red) // 100%
                // color.lightness() color.blackness() color.alpha() color.whiteness() color.hue()  
            ```
        - darken($color,percent0-100%) : makes color darker
        - lighten($color,percent0-100%)
        - color.mix($color1,color2,percentOfColor1)  
        - transparentize($color,amount0-1) : current opacity - typed opacity
4. Lists: the first item index corresponds to 1
     - we may separate items with "," or space and we can use bracket or ([]) as cover
     - the first item index corresponds to 1.we can use negative numbers to count from last index.
     - sass list is not like js arrays.Sass list functions return new list instead of update current list   
     - "sass:list" : all functions also can use with maps.
         - ```scss
            @use "sass:list"
            $myList:(1,2,3,4,6,6)

            @debug list.nth($myList,4) // 4 or index()
            @debug list.index($myList,6)   // 5
            @debug list.index($myList,hey) //null
            @debug list.append($myList,hey)  // 1, 2, 3, 4, 6, 6, hey   or append()
            @debug list.is-bracketed(red green blue) // false is list have square bracketed
            @debug list.join($list1,$list2,$separator:comma,$bracketed:true) // [red, green, blue, hakan, ahmet, mehmet] 
            // $separator: comma,space or slash   . $bracketed:auto ,false,true
            @debug list.length($list1) // 3
            @debug list.separator(ahmet mehmet ali)//space
            @debug list.separator([ahmet,mehmet,ali]) // comma
            @debug list.set-nth(ahmet mehmet ali,2,furkan) // ahmet furkan ali
            @debug list.set-nth(ahmet mehmet ali,-1,furkan) // ahmet mehmet furkan
            @debug list.slash(ahmet ,mehmet, ali) // ahmet / mehmet / ali
           ```

5. Maps: (key:value,key:value)  : keys must be unique.
    - unlike lists, brackets must be used.
    - ```scss
        @use "sass:map"
        @use "sass:list"

        $themes:(dark: black,light: grey,attractive: yellow)
        $theme2:(extra1 : brown,light: info,nestedMap:(color:hi))

        @debug map.get($themes,light) // grey
        @debug map.get($themes,ahmet) // null
        @debug list.length($themes)  // 3
        @debug map.has-key($themes,dark) // true
        @each $key,$value in $themes
            @debug $key +  $value  // darkblack lightgrey attractiveyellow
        @debug map.set($themes,standart, blue)
        @debug map.merge($themes,$theme2)  // if both map have same keys second map will be current value.
        @debug map.deep-merge($themes,$theme2) // It also looks deep maps
        @debug map.remove($themes,dark)
        @debug map.deep-remove($theme2,nestedMap,color) //  (extra1: brown, light: info, nestedMap: ()) .for nested maps use ($map,parentmap,childmap)
        @debug map.keys($themes) // dark, light, attractive
        @debug map.values($themes) // black, grey, yellow
       ```
6. Booleans: 
 ```scss
        @debug 1>3  // false
        @debug 23px>50px // false
        @debug ahmet == "ahmet" // true
        @debug list.length(red blue yellow)>2 // true
        @debug if(2>21px,red,blue) // blue
 ```

 ```scss
     // Important example
            @use "sass:meta"

        =selectTheme($themes...)
            @each $prop,$value in meta.keywords($themes)
                #{$prop}:$value
                
        div p
            +selectTheme($color: red,$background-color: yellow,$padding: 10px,$border-radius: 20px)
```

7. **null** : 
     ```scss
                @use "sass:list"
                    
                $myList:red blue yellow
                    
                p
                    background: list.index($myList,100)  // this property  won't compiled

                @debug if(null,red,blue) // blue   . null is falsey
     ```

## **Operators**
1. Equality:
```scss
        @debug 2==2 // true
        @debug 2==2px // false
        @debug ahmet == "ahmet"  // true
        @debug hsl(34, 35%, 92.1%) == #f2ece4 // true   if colors have same red,green,blue and alpha value,it will return true.
        @debug null== false  // false
        @debug null== true  // false  
        @debug null== null  // true . null,true and false are only equal own value.
        @debug 96px == 1in // true
```
2. Relational Operators:
     ```scss
         @use "sass:color"
        @debug 2>20px  // false 
        @debug 20px>= 100 // false 
        @debug 1000ms <= 1s // true
        @debug 100px< 1in // false 
        @debug 100px>1s // error
     ```

3. Numeric Operators:                               
     ```scss
            @use "sass:list"
            @debug 2-21   // -19
            @debug 2+20px  // px
            @debug 4*10in  // 40in
            // @debug 4px % 2s // error  .Numbers must be compatible.
            @debug 1 -2 3   //   1 -2 3
            @debug 1 - 2 3 // -1 3
            @debug 1px / 2px
            @debug 1 / 2px
            // Division
                // The operation is surrounded by parentheses, unless those parentheses are outside a list that contains the operation or
                // The result is stored in a variable or returned by a function.
                // sass will do divide operation
            @debug 1px / 2px  // 1px/2px
            @debug list.slash(1px,2px) // 1px/2px
            @function divide()
                @return 1px / 2px
            @debug divide()  // 0.5
            @debug (1px / 2px) // 0.5
            @debug 2px*2px  //4px*px
     ```
     - "sass:math"
         ```scss
            @use "sass:math"

            @debug math.$e  // 2.7182818285
            @debug math.$pi  // 3.1415926536
            @debug math.ceil(4.5)  // 5
            @debug math.floor(4.5)  // 4 
            @debug math.round(4.5)  // 5
            @debug math.round(4.2)  // 4
            @debug math.clamp(1,0,5) // 1   . ($min,$number,$max) if $number< $min return min.else if $number > $max return max.else return $number
            @debug math.min(20,23,12,33) // 12
            @debug math.max(20,23,12,33) // 33
            @debug math.abs(-2) // 2  . if $number is negative returns -$number ,else returns $number
            @debug math.log(2,4) // 0.5    . math.log(a,b)=x   b exponential x= a
            @debug math.pow(2,3) // 8  . 2*2*2
            @debug math.sqrt(100) // 10   . square root.
            @debug math.cos(100deg) // -0.1736481777
            @debug math.sin(100deg) // 0.984807753 
            @debug math.tan(100deg)// -5.6712818196
            @debug math.acos(0.5) // 60deg
            @debug math.asin(0.5)// 30deg
            @debug math.atan(10)// 84.2894068625deg
            @debug math.compatible(1px,20in)  // true   . if numbers have compatible units will return true. 
            @debug math.compatible(10px,1s) // false
            @debug math.is-unitless(100px) // false
            @debug math.is-unitless(100) // true    
            @debug math.unit(100)  // ""
            @debug math.unit(100px) // px
            @debug math.div(2,2)  // 1  dividing operation
            @debug math.percentage(.1)  //10%   is equal to number*100%
            @debug math.random()  // is return a random number between 0 and 1
             ```
4. **String Operators**: 
     ```scss
        @use "sas:list"
        @debug "ahmet"+ "mehmet" // " ahmetmehmet"
        @debug ahmet + "mehmet" // ahmetmehmet
        @debug sans - serif // sans-serif
        // @debug sans * serif error
        @debug true + ahmet // trueahmet
        
        //  unary Operators : / and - .They return unquoted string starting with / or -.
        @debug / 15px   // 15px
        @debug - moz // -moz
     ```
5. **Boolean** :
     - condition and condition : if both conditions are true returns true.
     - condition or condition : Returns true if at least one of the conditions is true
     - not : returns reverse of the condition
     ```scss
        @use "sass:list"
        @debug not true   // false
        @debug true and false  // false 
        @debug true or false // true 
     ````

- **sass:meta**
     - **meta.content-exist** 
         - ```scss 
            @use "sass:meta"
            =hey
            @debug meta.content-exists()
            @content

            p
                +hey
                color:red
            ```
     - **meta.function-exists(name,moduleName)** : looks to $name function is exist or not.$name can be in sass build-module.
     - ***meta.mixin-exists(name,moduleName)***  : 
        - ```scss
            @use "sass:meta"
            @use "sass:math"
            @function hey()
                @debug hi world

            @debug meta.function-exists("min","math") // true
            @debug meta.function-exists("hey")  // true
            @debug meta.function-exists("bgColor") // true
           ```
     - **meta.global-variable-exists**: if variable declared at global scope.
       **meta.variable-exists:if variable**  can used at current scope.
     -  ```scss
        @use "sass:meta"
        $colors:red blue yelow
        @mixin s
        $localColors:red blue green
        @debug meta.global-variable-exists("localColors") // false
        @debug meta.variable-exists("localColors") // true
        @include s
        @debug meta.global-variable-exists("colors") // true
        @debug meta.global-variable-exists("localColors")  // false
        @debug meta.variable-exists("colors")   // true
        @debug meta.variable-exists("localColors") // false
        ```
     - **meta.keywords()** : use with arbitrary arguments.return them as a map.
         -  ```scss            
            @use "sass:meta";
            @mixin syntax-colors($args...) {
            @debug meta.keywords($args);
            // (string: #080, comment: #800, variable: #60b)

            @each $name, $color in meta.keywords($args) {
                pre span.stx-#{$name} {
                color: $color;
                }
            }
            }

            @include syntax-colors(
            $string: #080,
            $comment: #800,
            $variable: #60b,
            )
            ```
 
## Dart sass
1. **load-path=path** added directories will be saved and will support us when we use @use
    - ```scss
        // example/colors.scss
            *{
            color:blue;
        } 
      ```
    - ```bash
        sass --load-path=example  src:dist --watch
        ```
    ```scss
    @use "colors" // this is benefit of load-path.instead of "../example/colors/ we typed that.
    ```
2. **style**  : 
     - expanded : default
     - compressed  : sass --style=compressed 
3. **update**:  It also looks modules delete,update etc... operation and write them on console.
4. Source maps: source maps provides that seeing css files on browsers
    - **no-source-map** : don't create map files.
5. **watch** : look for changes.
6. **stop-on-error** : when error is detected don't compile any file
7. **help**
8. **version**

