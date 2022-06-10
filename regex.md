# RegExp
- for exercises https://regexr.com/
- regular expresssions.
- regex is used  in all programming languages
### declaring a RegExp
```javascript
const x=/myregex/;   // if data is constant.
const y=new RegExp("myregex"); //when data is dynamic
```

### using simple pattern:for exact matches
```javascript
const text="hello world";
console.log(text.match(/e/))  // [ 'e', index: 1, input: 'hello world', groups: undefined ]
console.log(text.match(/e/)[0]) //e
```

### Using special characters: for complex searches.

#### **quantifiers**:
-  \+  1 or more
- \*   0 or 1 or more
- ?   0 or 1
- .  any character
- x{number} x length must be length of number 
- x{number,} x character length must be exist more than number   .If there are more then number,select all.
- x{a,b} x character count must be less b than and greater than a
- quantifier? match as few character as possible :  x{number,}?  , \*?

#### **Character Classes:discriminate between digits and letters**
- \\.  matches a literal dot.
- \d any figure .equivalent to [0-9]
- \D not a number. equivalent to [^0-9]
- \w any latin alphabet (number or letter and _). equivalent to [^A-Za-z0-9_]
- \W  not a latin alphabet (symbol,figure..) 
- \s space character
- \S not a space character

#### **Groups and ranges**:determines a range.
- x|y x or y
- [xyz] x or y or z
- [a-z] letters a to z . [a-h] letters a to z . [0-9] figures 0 to 9
-[^xyz] not x or y or z.  [^a-z] not letters a to z. [^0-9] not figures 0 to 9
-(x|y|z) match x or y or z and remember its value.and group it.

#### **Assertions** :matches beginning or end of words,lines or input
- ^ match beginning of input. ^a  string must be  start with a.
- \$ match ending of input . a$ string must be end with a
- Lookahead and lookbehind:
     1. lookAhead:  
         - positive: .(?=condition) : must be compatible with condition after any character.
         ```javascript
         /.(?=(x))/  // any character ending in an x.
         ```
         - negative : .(?!condition) : must be not compatible with condition after any character.
        ```javascript
         /.(?!(x))/  // any character not ending in an x.
         ```
    2. lookbehind:
         - positive: (?<=y). before x must match the condition
         - negative: (?<!x). before x must not match the condition
- \b look at the boundary of word.
     1. .\b : look at the end of the word.
     2. \b. : look at the beginning of the word 

example: extract url : (?<=src=")(.*)(?=")

##### **FLAGS**:
1. **/g**   as default, selects first compatible argument .global flag provides that select all compatible characters
2. **/i**        /aBc/i would match AbC.
3. **/m** m flag provides that handle each line as a new input.So ^ and $ are work for every line.

#### *Note* escape character: \ 
#### *Note* (?\<groupName\>) can be used for group name operations.

## Examples
Phone number validation:
```javascript
^(?<areaCode>\+\d{2})?\(?(?<operator>\d{3})\)?\D?(?<main>\d{3})\D?(?<number>\d{4}) 
or
/^[\+]?[(]?[0-9]{3}[)]?[-\s\.]?[0-9]{3}[-\s\.]?[0-9]{4,6}$/im  
// 5426658239
// 542-665-8239
// 542 665 8239
// (542) 665 8239
// +90 542 665 8239
```

Date validation:
```javascript
/^(?<day>\d{2})[\./-](?<month>\d{2})[\./-](?<year>\d{4}|\d{2})$/gi
//  10.02.2018
// 10.02.18 true
// 10/02/2017 true
// 10-02-2018 true
// 10-02-201 false 
```

correct date val regex: 
```javascript
^(?:(?:31(\/|-|\.)(?:0?[13578]|1[02]))\1|(?:(?:29|30)(\/|-|\.)(?:0?[13-9]|1[0-2])\2))(?:(?:1[6-9]|[2-9]\d)?\d{2})$|^(?:29(\/|-|\.)0?2\3(?:(?:(?:1[6-9]|[2-9]\d)?(?:0[48]|[2468][048]|[13579][26])|(?:(?:16|[2468][048]|[3579][26])00))))$|^(?:0?[1-9]|1\d|2[0-8])(\/|-|\.)(?:(?:0?[1-9])|(?:1[0-2]))\4(?:(?:1[6-9]|[2-9]\d)?\d{2})$
```