# angular-tslint-rules
Shared [TSLint] & [codelyzer] rules to enforce a consistent code style for [Angular] development

[![npm version](https://badge.fury.io/js/angular-tslint-rules.svg)](https://www.npmjs.com/package/angular-tslint-rules)

> Please support this project by simply putting a Github star. Share this library with friends on Twitter and everywhere else you can.

The value of the software produced is directly affected by the **quality of the codebase**, and not every developer might
- be **aware of the potential pitfalls** of certain constructions in [TypeScript],
- be **introduced into certain conventions** when using the [Angular] framework,
- know that not every developer is as capable in **understanding an elegant** (*but abstract*) **solution** as the original developer.

For that purpose, we need to use **static code analysis tools** such as [TSLint] and [codelyzer] to check readability, maintainability, and functionality errors.

Although complying with these tools may seem to appear as **undesired overhead** or may **limit creativity**, it becomes **easier** for any new developers to **read**, **preventing** a lot of **time/frustration spent** figuring out the structure and characteristics of the code.

Containing a set of [TSLint] and [codelyzer] rules, **`angular-tslint-rules`** has been compiled using many contributions from colleagues, commercial/open-source projects and some other sources from the Internet, as well as years of development using the [Angular] framework.

If you have questions, comments or suggestions, just create an issue on this repository. I'll try to revise and republish these rules with new insights, experiences and remarks in alignment with the updates on [TSLint] and [codelyzer].

**Note**: The following set of rules depend on:
- [TSLint] v5.2.0
- [codelyzer] v3.0.1

## Table of contents:
- [Getting started](#getting-started)
  - [Installation](#installation)
- [Usage](#usage)
- [Rules](#rules)
  - [Class and Member design](#class-and-member-design)
  - [Interface design](#interface-design)
  - [Function design](#function-design)
    - [Anonymous functions](#anonymous-functions)
  - [Variable design](#variable-design)
  - [Requires and Imports](#requires-and-imports)
  - [Types](#types)
  - [Object literals](#object-literals)
  - [Strings](#strings)
  - [Operators](#operators)
  - [`for` statement](#for-statement)
  - [`switch` statement](#switch-statement)
  - [`try` statement](#try-statement)
  - [Maintainability](#maintainability)
  - [Layout](#layout)
    - [Whitespace](#whitespace)
    - [Empty lines](#empty-lines)
    - [Alignment](#alignment)
  - [Naming](#naming)
    - [Classes and Interfaces](#classes-and-interfaces)
    - [Variables and Functions](#variables-and-functions)
  - [Documentation](#documentation)
    - [Inline Comments](#inline-comments)
    - [JSDoc Comments](#jsdoc-comments)
  - [Misc](#misc)
  - [Codelyzer rules](#codelyzer-rules)
- [License](#license)

## Getting started
### Installation
You can install **`angular-tslint-rules`** using `npm`
```
npm install angular-tslint-rules --save
```

**Note**: You should have already installed [TSLint] and [codelyzer].

## Usage
To use these [TSLint] rules, use **configuration inheritance** via the **`extends`** keyword.

A sample configuration is shown below, where `tslint.json` lives adjacent to your `node_modules` folder:
```json
{
  "rulesDirectory": [
    "node_modules/codelyzer"
  ],
  "extends": ["angular-tslint-rules"],
  "rules": {
    // override tslint rules here
    ...
  }
}
```

## Rules
### Class and Member design
- *Do not specify* the **`public`** keyword (*this is the default accessibility level*).
- **`private`** and **`private static`** members in classes should be **denoted** with the **`private`** keyword.
```json
"member-access": [
  true,
  "no-public"
]
```

- Member ordering should be in the following way:
  - **Public members** should be ordered **before private members**.
  - **Static members** should be ordered **before instance members**.
  - **Variables** should be ordered **before functions**.
```json
"member-ordering": [
  true,
  "public-before-private",
  "static-before-instance",
  "variables-before-functions"
]
```

- Member **overloads** should be **consecutive**.
```json
"adjacent-overload-signatures": true
```

- *Always prefer* **unifying any two overloads** into one, by using a **union** or an **optional/rest parameter**. 
```json
"unified-signatures": true
```

- *Always prefer* functions over **`private`** members that do not use **`this`**.
```json
"prefer-function-over-method": [
  true,
  "allow-public",
  "allow-protected"
]
```

- *Do not use* the **`this`** keyword **outside class context** (*including functions in methods*).
```json
"no-invalid-this": [
  true,
  "check-function-in-method"
]
```

- *Do not invoke* the **`super`** method **twice** in a constructor (*except in branched statements or nested class constructors*).
```json
"no-duplicate-super": true
```

- *Always prefer* parentheses **`()`** when invoking a **constructor** via the **new** keyword.
```json
"new-parens": true
```

- *Do not define* **`new`** for **classes**.
```json
"no-misused-new": true
```

- *Do not use* the **constructors** of **`String`**, **`Number`**, and **`Boolean`**.
```json
"no-construct": true
```

### Interface design
- *Do not define* **constructors** for **interfaces**.
> See: [Class and Member design](#class-and-member-design)

- *Do not use* **empty interfaces**.
```json
"no-empty-interface": true
```

- *Always prefer* **`foo(): void`** over **`foo: () => void`** in **interfaces** and **types**.
```json
"prefer-method-signature": true
```

- *Always prefer* an **interface declaration** over a **type literal** (**`type T = { ... }`**).
```json
"interface-over-type-literal": true
```

### Function design
- **Functions** should be defined right **after** the **variable declarations**.
> See: [Class and Member design](#class-and-member-design)

- *Do not invoke* **`arguments.callee`** within a function, as it makes **impossible** various **performance optimizations**.
```json
"no-arg": true
```

#### Anonymous functions
- *Always prefer* defining anonymous functions as **fat-arrow/lambda** `() => { }` functions (*unless it is absolutely necessary to preserve the context in the function body*).
```json
"only-arrow-functions": [
  true,
  "allow-declarations",
  "allow-named-functions"
]
```

- **fat-arrow/lambda** functions should have parenthesis **`()`** around the **function parameters** (*except if removing them is allowed by TypeScript*).
```json
"arrow-parens": [
  true,
  "ban-single-arg-parens"
]
```

- *Always prefer* **`() => x`** over **`() => { return x; }`**.
```json
"arrow-return-shorthand": true
```

### Variable design
- *Always prefer* **`const`** keyword **where appropriate**, for values that should never change.
- *Then prefer* **`let`** everywhere else.
- *Do not use* the **`var`** keyword.
```json
"prefer-const": true
```

- *Do not use* **implied global variables**.
- *Do not define* a variable on the **global scope** from within a **smaller scope**.
```json
"no-shadowed-variable": true
```

- Declare **one variable** at a time (*except in loops*).
```json
"one-variable-per-declaration": [
  true,
  "ignore-for-loop"
]
```

- *Do not use* **duplicate variable names** in the same block scope.
```json
"no-duplicate-variable": true
```

- *Do not use* a **`var`**/**`let`** statement or destructuring initializer to be **initialized** to **`undefined`**. 
```json
"no-unnecessary-initializer": true
```

### Requires and Imports
- *Always use* the **`import`** statement keywords in **alphabetical order**.
```json
"ordered-imports": [
  true,
  {
    "import-sources-order": "any",
    "named-imports-order": "case-insensitive"
  }
]
```

- *Always `import`* submodules from **`rxjs`**.
```json
"import-blacklist": [
  true,
  "rxjs"
]
```

- *Do not use* **`require`** statements at all (*use **ES6-style** **`import`** statement instead*).
```json
"no-require-imports": true
```

- *Do not use* **default exports** in ES6-style modules.
```json
"no-default-export": true
```

- *Do not use* **`/// <reference path=> imports`** statements at all (*use **ES6-style** **`import`** statement instead*).
```json
"no-reference": true
```

### Types
- *Always prefer* defining the **return type** of functions, methods and property declarations **explicitly**.
- Types should be used **whenever necessary** (*no implicit **`any`***).
```json
"typedef": [
  true,
  "call-signature",
  "property-declaration"
]
```

- *Always prefer* **type inference** over **explicit type declaration** (*except for function return types*).
```json
"no-inferrable-types": true
```

- The result of **`typeof`** should be **compared** to correct **string values**.
```json
"typeof-compare": true
```

- *Always prefer* the use of **`as Type`** for type assertions over **`<Type>`**.
```json
"no-angle-bracket-type-assertion": true
```

- *Always prefer* writing an **interface** or **literal type** with just a **call signature** as a function type.
```json
"callable-types": true
```

- *Do not use* the **`null`** keyword, always return **`undefined`** instead of a **`null`** reference.
```json
"no-null-keyword": true
```

- *Do not use* **non-null** assertions.
```json
"no-non-null-assertion": true
```

- **Arrays** should be defined as **`Array<type>`** instead of **`type[]`**.
```json
"array-type": [
  true,
  "generic"
]
```

### Object literals
- *Always prefer* **ES6 object literal shorthand** whenever possible.
```json
"object-literal-shorthand": true
```

- *Always prefer* object property names **using literals** whenever possible.
```json
"object-literal-key-quotes": [
  true,
  "as-needed"
]
```

### Strings
- *Always prefer* single-quotes **`''`** for all strings, and use double-quotes **`""`** for strings within strings.
```json
"quotemark": [
  true,
  "single",
  "avoid-escape"
]
```

- *Always prefer* **template expressions** over string **literal concatenation**.
```json
"prefer-template": true
```

- *Do not use* string templates outside **template strings** (**``**).
```json
"no-invalid-template-strings": true
```

### Operators
- *Always prefer* using **`===`** and **`!==`** operators whenever possible.
> **`==`** and **`!=`** operators do type coercion, which can lead to **headaches** when debugging code.
```json
"triple-equals": [
  true,
  "allow-null-check"
]
```

- *Do not use* **bitwise** operators.
```json
"no-bitwise": true
```

- *Always prefer* using **`isNan`** function to check **`NaN`** references.
```json
"use-isnan": true
```

- Assignment expressions inside of the condition block of **`if`**, **`while`**, and **`do while`** statements should be **avoided**.
```json
"no-conditional-assignment": true
```

### `for` statement
- *Always prefer* **`for-of`** loop over a standard **`for`** loop.
```json
"prefer-for-of": true
```

- Filter **`for-in`** statements with an **`if`** statement (*this prevents accidental iteration over properties inherited from an object’s prototype*).
```json
"forin": true
```

### `switch` statement
- Each **switch** statement should have a **default case**.
```json
"switch-default": true
```

- Each switch case **except default** should **end** with **`break`**, **`return`**, or **`throw`**.
```json
"no-switch-case-fall-through": true
```

### `try` statement
- *Do not use* **control flow** statements, such as **`return`**, **`continue`**, **`break`** and **`throws`** in **`finally`** blocks.
```json
"no-unsafe-finally": true
```

### Maintainability
- *Limit* the **level of complexity** (cyclomatic complexity) in a function/method by **20 branches**.
```json
"cyclomatic-complexity": [
  true,
  20
]
```

- Files should not exceed **1000 lines** of code.
```json
"max-file-line-count": [true, 1000]
```

- Keep the length of **each line** under **140 characters**.
```json
"max-line-length": [
  true,
  140
]
```

- *Do not use* **tabs** for indentation.
```json
"indent": [
  true,
  "spaces"
]
```

- All files should end in a **new line**.
```json
"eofline": true
```

### Layout
#### Whitespace
Whitespaces should be used in the following circumstances:
- All branching statements (**`if`**/**`else`**/**`for`**/**`while`**) should be followed by **one space**.
- Variable declarations should be separated by **one space** around the **type specification** and **equals token**.
- All **operators** except the period **`.`**, left parenthesis **`(`**, and left bracket **`[`** should be separated from their operands by **one space**.
- There should be **no space** around between the **unary/incremental operators** **`!x, -x, +x, ~x, ++x, --x`** and its **operand**.
- There should be **one space** after the left curly brace **`{`** and before the right curly brace **`}`** containing **`import`** statement keywords. 
- Each separator (**`,`**,**`;`**) in the control part of a **`for`** statement should be followed with **one space**.
- The left curly brace **`{`** followed by a right parenthesis **`)`** should always separated by **one space**.
```json
"whitespace": [
  true,
  "check-branch",
  "check-decl",
  "check-operator",
  "check-module",
  "check-separator",
  "check-type",
  "check-preblock"
]
```

- For each **call signature**, **index signature**, **parameter**, **property declaration** and **variable declaration**;
  - There should be **no space** between the **parameter** and the colon **`:`** indicating the **type declaration**.
  - There should be **one space** between the colon **`:`** and the **type declaration**.
```json
"typedef-whitespace": [
  true,
  {
    "call-signature": "nospace",
    "index-signature": "nospace",
    "parameter": "nospace",
    "property-declaration": "nospace",
    "variable-declaration": "nospace"
  },
  {
    "call-signature": "onespace",
    "index-signature": "onespace",
    "parameter": "onespace",
    "property-declaration": "onespace",
    "variable-declaration": "onespace"
  }
]
```

- For each **function**, **class member** and **constructor**;
  - There should be **no space** between the **name of the function/member** and the left parenthesis **`(`** of its parameter list.
- For each **fat-arrow/lambda function**;
  - There should be **one space** between the right parenthesis **`)`** and the **`=>`**.
```json
"space-before-function-paren": [
  true,
  {
    "anonymous": "never",
    "named": "never",
    "asyncArrow": "always",
    "method": "never",
    "constructor": "never"
  }
]
```

- Put **one space** between the **`import`** statement keywords.
```json
"import-spacing": true
```

- *Do not use* **trailing whitespaces** at the end of a line 
```json
"no-trailing-whitespace": true
```

#### Empty lines
Empty lines improve code readability by allowing the developer to logically group code blocks.
- There should be an **empty line** before the **return** statement.
```json
"newline-before-return": true
```

- For each **function**, **anonymous function**, **class member**, **constructor**, **`else`**, **`catch`** and **`finally`** statements;
  - There should be **one space** between the right parenthesis **`)`** and the left curly **`{`** brace that begins the statement body.
- For each **fat-arrow/lambda function**;  
  - There should be **one space** between the **`=>`** and the left curly brace **`{`** that begins the statement body.
- **`else`** statements should **indented to align** with the line containing the closing brace for the **`if`** statement.
- **`catch`** and **`finally`** statements should **indented to align** with the line containing the closing brace for the **`try`** statement.  
```json
"one-line": [
  true,
  "check-open-brace",
  "check-whitespace",
  "check-else",
  "check-catch",
  "check-finally"
]
```

- *Do not use* more than **one empty line** in a row.
```json
"no-consecutive-blank-lines": [
  true,
  1
]
```

#### Alignment
- A **semicolon** should be placed at the **end** of every **simple statement**.
```json
"semicolon": [
  true,
  "always"
]
```

- **Vertically align** parameters and statements (*helps maintain a readable, consistent style in your codebase*).
```json
"align": [
  true,
  "parameters",
  "statements"
]
```

- *Always prefer* **trailing commas** in **array and object literals**, **destructuring assignments**, **function typings**, **named imports/exports** and **function parameters**.
```json
"trailing-comma": [
  true,
  {
    "multiline": "never",
    "singleline": "never"
  }
]
```

### Naming
#### Classes and Interfaces
- **Class** and **interface names** should be in **PascalCase**.
```json
"class-name": true
```

#### Variables and Functions
- All **variable**, and **function names** should be in **camelCase**.
- *Do not use* **leading** and **trailing** underscore **`_`** characters.
```json
"variable-name": [
  true,
  "check-format",
  "ban-keywords"
]
```

### Documentation
#### Inline Comments
- *Always prefer* **`//`** for all **inline comments**.
- There should be **one space** before the comment.
```json
"comment-format": [
  true,
  "check-space"
]
```

#### JSDoc Comments
- [JSDoc] style comments should start with **`/**`** and end with **`*/`**.
```json
"jsdoc-format": true
```

### Misc
- *Do not use* the **`console`** method (*such messages are considered to be for debugging purposes and therefore might ship to the production environment*).
```json
"no-console": [
  true,
  "log",
  "debug",
  "info",
  "time",
  "timeEnd",
  "trace"
]
```

- *Do not use* the **`debugger`** statement (*this might cause the environment to stop execution and start up a debugger, if not omitted on the production code*).
```json
"no-debugger": true
```

- *Do not use* the **`eval`** function (*using `eval` on untrusted code might open a program up to several different injection attacks*).
```json
"no-eval": true
```

- *Do not throw* **plain strings** or **concatenations of strings** (*because only `Error`s produce proper stack traces*).
```json
"no-string-throw": true
```

- *Do not use* **namespaces** (*using `namespace {}` is outdated*).
```json
"no-namespace": true
```

- *Do not use* internal **modules** (*using `module {}` is outdated*).
```json
"no-internal-module": true
```

- *Always prefer* the **`radix`** parameter to be specified when calling **`parseInt`**.
```json
"radix": true
```

- *Do not leave* **unused expressions** in the code.
```json
"no-unused-expression": [
  true,
  "allow-fast-null-checks"
],
```

- *Do not use* empty blocks **`{}`** in the code.
```json
"no-empty": true
```

- *Do not use* **missing elements** in arrays.
```json
"no-sparse-arrays": true
```

### Codelyzer rules
```json
"directive-selector": [
  true,
  "attribute",
  [
    "ngx",
    "test"
  ],
  "camelCase"
],
"component-selector": [
  true,
  "element",
  [
    "ngx",
    "test"
  ],
  "kebab-case"
],
"use-input-property-decorator": true,
"use-output-property-decorator": true,
"use-host-property-decorator": true,
"no-attribute-parameter-decorator": true,
"no-input-rename": true,
"no-output-rename": true,
"no-forward-ref": false,
"use-life-cycle-interface": true,
"use-pipe-transform-interface": true,
"pipe-naming": [
  true,
  "camelCase",
  "ngx"
],
"component-class-suffix": true,
"directive-class-suffix": true,
"templates-use-public": true,
"no-access-missing-member": true,
"invoke-injectable": true,
"template-to-ng-template": true
```

## License
The MIT License (MIT)

Copyright (c) 2017 [Burak Tasci]

[TSLint]: https://github.com/palantir/tslint
[codelyzer]: https://github.com/mgechev/codelyzer
[Angular]: https://angular.io
[TypeScript]: https://github.com/Microsoft/TypeScript
[JSDoc]: http://usejsdoc.org
[Burak Tasci]: https://github.com/fulls1z3
