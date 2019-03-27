# Cheat Sheet Code Standard

## Table of Contents 

- Javascript
    1. [Naming](#naming)
    2. [Variables](#variables)
    3. [Typings](#typings)
    4. [Classes](#classes)
    5. [Objects](#objects)
    6. [Code Structure](#code-structure)
    7. [Casting & Coercion](#casting--coercion)
    8. [Comments](#comments)
    9. [Avoid](#avoid)

 - CSS
    - TBD

- Versioning
    - [Pull Request](#pull-request)
 
# Javascript

## Naming 

<a name="naming-camelcase"></a><a name="1.1"></a>
- [1.1](#variables--camelcase) **camelCase & descriptive**:  
    - Camel case for variables and functions
    - Describe what the variable store or function does
    ```js
    // bad
    const Variable
    const variable_one
    const dE
    const date  //not descriptive

    // good 
    const dateExpired
    function checkDateExpired() {}
    ```
<a name="naming-uppercamelcase"></a><a name="1.2"></a>
- [1.2](#variables--uppercamelcase) **UpperCamelCase**: 
  - Upper camel case for clases
   ```js
    // bad
    class session {}
    class session_user {}
    class sessionUser {}

    // good 
    class Session {}
    class SessionUser {}
    ```
<a name="naming-dash-case"></a><a name="1.3"></a>
- [1.3](#variables--dash-case) **dash-case**: 
  - Dash case for files
   ```bash
    # bad
    SessionUser.js
    Session_User.js
    session_user.js

    # good 
    session-user.js
    ```
                              
<a name="naming--avoid"></a><a name="1.4"></a> 
- [1.4](#variables--avoid) **Avoid**: [[1]](#airbnb) 
  - Do not use trailing or leading underscores
  >Why? JavaScript does not have the concept of privacy in terms of properties or methods. Although a leading underscore is a common convention to mean “private”, in fact, these properties are fully public, and as such, are part of your public API contract. This convention might lead developers to wrongly think that a change won’t count as breaking, or that tests aren’t needed. tl;dr: if you want something to be “private”, it must not be observably present.
   ```js
    // bad
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';
    this._firstName = 'Panda';

    // good
    this.firstName = 'Panda';
    ```

## Variables 

<a name="variables--declaration"></a><a name="2.1"></a>
- [2.1](#variables--declaration) **Declaration**:
    - use `const` for value that not change and `let` to others
    - Always use const or let to declare variables. Not doing so will result in global variables.[[1]](#airbnb) 
    ```js
    // bad 
    globalVariable = true 

    const globalVariable = true;
    globalVariable = false;

    // good 
    const scopeVarible = true ;

    let globalVariable = true;
    globalVariable = false;
    ```
<a name="variables--caching"></a><a name="2.2"></a>
- [2.2](#variables--caching) **Caching Values**:
    - cache the lookup once
    ```js
    //bad
    if( orders[index].state == '' || orders[index].state == 'EMPTY'){
    }

    if( order.state == '' || order.state == 'EMPTY' || order.state == 'OPEN'){
    }

    //good 
    const orderState = orders[index].state;

    if( orderState == '' || orderState == 'EMPTY'){
    }
    ```
<a name="variables--grouping"></a><a name="2.3"></a>
- [2.3](#variables--grouping) **Grouping Declarations**:
    - Group all your consts and then group all your lets [[1]](#airbnb) 
    ```js
    // bad
    let i;
    const items = getItems();
    let dragonball;
    const goSportsTeam = true;
    let len;

    // good
    const goSportsTeam = true;
    const items = getItems();
    let dragonball;
    let ordersIndex;
    let ordersLength;
  ```
## Typings (for project with Typescript)

TBD - use types of Node.js  or interfaces, avoid any always  

## Classes

TBD - no constructor no public accesor explicit 

## Objects

TBD - immutable as much as posible ; use map, filter ...

## Code Structure

<a name="code-structure--chain-variable"></a><a name="6.1"></a>
- [6.1](#code-structure--chain-variable) **Don’t chain variable assignments** [[1]](#airbnb) 
    >Why? Chaining variable assignments creates implicit global variables. 
    ```js
    // bad
    (function example() {
    // JavaScript interprets this as
    // let a = ( b = ( c = 1 ) );
    // The let keyword only applies to variable a; variables b and c become
    // global variables.
    let a = b = c = 1;
    }());

    console.log(a); // throws ReferenceError
    console.log(b); // 1
    console.log(c); // 1

    // good
    (function example() {
    let a = 1;
    let b = a;
    let c = a;
    }());

    console.log(a); // throws ReferenceError
    console.log(b); // throws ReferenceError
    console.log(c); // throws ReferenceError

    // the same applies for `const` 
    ```
<a name="code-structure--ternaries"></a><a name="6.2"></a>
- [6.2](#code-structure--ternaries) Ternaries should not be nested and generally be single line expressions. [[1]](#airbnb) 
    ```js
    // bad
    const foo = maybe1 > maybe2
    ? "bar"
    : value1 > value2 ? "baz" : null;

    // split into 2 separated ternary expressions
    const maybeNull = value1 > value2 ? 'baz' : null;

    // better
    const foo = maybe1 > maybe2
    ? 'bar'
    : maybeNull;

    // best
    const foo = maybe1 > maybe2 ? 'bar' : maybeNull;
    ```

<a name="code-structure--mixing-operators"></a><a name="6.3"></a>
- [6.3](#code-structure--mixing-operators) When mixing operators, enclose them in parentheses. The only exception is the standard arithmetic operators (+, -, *, & /) since their precedence is broadly understood. eslint: no-mixed-operators [[1]](#airbnb)

    >Why? This improves readability and clarifies the developer’s intention.
    ```js
    // bad
    const foo = a && b < 0 || c > 0 || d + 1 === 0;

    // bad
    const bar = a ** b - 5 % d;

    // bad
    // one may be confused into thinking (a || b) && c
    if (a || b && c) {
    return d;
    }

    // good
    const foo = (a && b < 0) || c > 0 || (d + 1 === 0);

    // good
    const bar = (a ** b) - (5 % d);

    // good
    if (a || (b && c)) {
    return d;
    }

    // good
    const bar = a + b / c * d;
    ```

##  Casting & Coercion

  <a name="coercion--strings"></a><a name="7.1"></a>
  - [7.1](#coercion--strings)  Strings: [[1]](#airbnb)

    ```javascript
    // => this.reviewScore = 9;

    // bad
    const totalScore = new String(this.reviewScore); // typeof totalScore is "object" not "string"

    // bad
    const totalScore = this.reviewScore + ''; // invokes this.reviewScore.valueOf()

    // bad
    const totalScore = this.reviewScore.toString(); // isn’t guaranteed to return a string

    // good
    const totalScore = String(this.reviewScore);
    ```

  <a name="coercion--numbers"></a><a name="7.2"></a>
  - [7.2](#coercion--numbers) Numbers: Use `Number` for type casting and `parseInt` always with a radix for parsing strings. [[1]](#airbnb)

    ```javascript
    const inputValue = '4';

    // bad
    const val = new Number(inputValue);

    // bad
    const val = +inputValue;

    // bad
    const val = inputValue >> 0;

    // good but it's better specify decimal
    const val = parseInt(inputValue);

    // good
    const val = parseInt(inputValue, 10);
    
    // good
    const val = Number(inputValue);
    ```

  <a name="coercion--booleans"></a><a name="7.3"></a>
  - [7.3](#coercion--booleans) Booleans: [[1]](#airbnb)

    ```javascript
    const age = 0;

    // bad
    const hasAge = new Boolean(age);

    // good
    const hasAge = Boolean(age);

    // best
    const hasAge = !!age;
    ```

## Comments 
[[1]](#airbnb)
  <a name="comments--multiline"></a><a name="8.1"></a>
  - [8.1](#comments--multiline) Use `/** ... */` for multi-line comments. 

    ```javascript
    // bad
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param {String} tag
    // @return {Element} element
    function make(tag) {

      // ...

      return element;
    }

    // good
    /**
     * make() returns a new element
     * based on the passed-in tag name
     */
    function make(tag) {

      // ...

      return element;
    }
    ```

  <a name="comments--singleline"></a><a name="8.2"></a>
  - [8.2](#comments--singleline) Use `//` for single line comments. Place single line comments on a newline above the subject of the comment.

    ```javascript
    // bad
    const active = true;  // is current tab

    // good
    // is current tab
    const active = true;

    // good
    function getType() {
      console.log('fetching type...');

      // set the default type to 'no type'
      const type = this.type || 'no type';

      return type;
    }

    // also good
    function getType() {
      // set the default type to 'no type'
      const type = this.type || 'no type';

      return type;
    }
    ```

  <a name="comments--actionitems"></a><a name="8.3"></a>
  - [8.3](#comments--actionitems) Prefixing your comments with `FIXME` or `TODO` helps other developers quickly understand if you’re pointing out a problem that needs to be revisited, or if you’re suggesting a solution to the problem that needs to be implemented. These are different than regular comments because they are actionable. The actions are `FIXME: -- need to figure this out` or `TODO: -- need to implement`.

  <a name="comments--fixme"></a><a name="8.4"></a>
  - [8.4](#comments--fixme) Use `// FIXME:` to annotate problems.

    ```javascript
    class Calculator extends Abacus {
      constructor() {
        super();

        // FIXME: shouldn’t use a global here
        total = 0;
      }
    }
    ```

  <a name="comments--todo"></a><a name="8.5"></a>
  - [8.5](#comments--todo) Use `// TODO:` to annotate solutions to problems.

    ```javascript
    class Calculator extends Abacus {
      constructor() {
        super();

        // TODO: total should be configurable by an options param
        this.total = 0;
      }
    }
    ```

## Avoid
 Never evaluate a string 
```js
// bad 
eval(new String("2 + 2")); 
eval("2 + 2");

new Function("x", "y", "return x * y");
```

 <a name="airbnb"></a><a name="airbnb"></a>
  [[1] Getting from AirBnB style guide](https://github.com/airbnb/javascript)


# Versioning

## Pull Request 
- COMMON TEMPLATE: 
    - Description
    - Screenshoot with annotation/gif/video of UI or behaviour changes 
    - Screenshot of the original work to look up the difference
    - Link to Jira Or Issue
    - Reference others PRs if depends each other (Link)
    - Step By Step how to  test 
- Rules 
    - Never push to master
    - Create PR to devops 
    - Create granular commits with specific message
    - Create PRs by only one feature as much as possible
    - Try to create PRs smalls max 7 files as much as possible

- Resources
    - commits https://github.com/commitizen/cz-cli
    - skitch for annotation https://evernote.com/intl/es/products/skitch
    - Record cam/video + mic/sound + screenvideo 
        1. http://recordit.co/
        2. https://www.vidyard.com/govideo/
   
