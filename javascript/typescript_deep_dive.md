# Typescript Deep Dive

by Basart Ali Syed

## Getting Started

* Types can be implicit
* [Definitely Typed](https://github.com/DefinitelyTyped/DefinitelyTyped) -- type definitions

* `==` is for values
* `===` is for references

* Something hasn't been initialized : `undefined`
* Something is currently unavailable: `null`
* Recommend `== null` to check for both `undefined` or `null`
    * don't want to make a distinction between the two

* `this` == Any access to this keyword within a function is actually controlled by how the function is actually called. It is commonly referred to as the “calling context.”

* [TypeScript classes](https://basarat.gitbooks.io/typescript/content/docs/classes.html)

* `var` is function scoped
* `let` is block scoped

* object destructuring is like equivalent to python's unpacking

* `...rest` is like *args (spread operator)
* `for ... of ...` loops over items not indexes

* JS's version of f-strings

```js
console.log(`<body>${blah}</body>`)
```

### Async Functions

A few points to keep in mind when working with async functions based on callbacks are:

1. Never call the callback twice.
1. Never throw an error.

But we fall into callback hell, so use promises.

### Promises

* A promise can be either `pending` or `fulfilled` or `rejected`

<img src="typescript-deep-dive/promise-states-and-fates.png" />

Let's look at creating a promise. It's a simple matter of calling new on Promise (the promise constructor). The promise constructor is passed resolve and reject functions for settling the promise state:

```js
const promise = new Promise((resolve, reject) => {
    // the resolve / reject functions control the fate of the promise
});
```

* The promise fate can be subscribed to using `.then` (if resolved) or `.catch` (if rejected).

* Tips:
    * Quickly creating an already resolved promise: `Promise.resolve(result)`
    * Quickly creating an already rejected promise: `Promise.reject(error)`

## Compiler Options

```json
{
  "compilerOptions": {

    /* Basic Options */
    "target": "es5",                       /* Specify ECMAScript target version: 'ES3' (default), 'ES5', 'ES2015', 'ES2016', 'ES2017', or 'ESNEXT'. */
    "module": "commonjs",                  /* Specify module code generation: 'commonjs', 'amd', 'system', 'umd' or 'es2015'. */
    "lib": [],                             /* Specify library files to be included in the compilation:  */
    "allowJs": true,                       /* Allow javascript files to be compiled. */
    "checkJs": true,                       /* Report errors in .js files. */
    "jsx": "preserve",                     /* Specify JSX code generation: 'preserve', 'react-native', or 'react'. */
    "declaration": true,                   /* Generates corresponding '.d.ts' file. */
    "sourceMap": true,                     /* Generates corresponding '.map' file. */
    "outFile": "./",                       /* Concatenate and emit output to single file. */
    "outDir": "./",                        /* Redirect output structure to the directory. */
    "rootDir": "./",                       /* Specify the root directory of input files. Use to control the output directory structure with --outDir. */
    "removeComments": true,                /* Do not emit comments to output. */
    "noEmit": true,                        /* Do not emit outputs. */
    "importHelpers": true,                 /* Import emit helpers from 'tslib'. */
    "downlevelIteration": true,            /* Provide full support for iterables in 'for-of', spread, and destructuring when targeting 'ES5' or 'ES3'. */
    "isolatedModules": true,               /* Transpile each file as a separate module (similar to 'ts.transpileModule'). */

    /* Strict Type-Checking Options */
    "strict": true,                        /* Enable all strict type-checking options. */
    "noImplicitAny": true,                 /* Raise error on expressions and declarations with an implied 'any' type. */
    "strictNullChecks": true,              /* Enable strict null checks. */
    "noImplicitThis": true,                /* Raise error on 'this' expressions with an implied 'any' type. */
    "alwaysStrict": true,                  /* Parse in strict mode and emit "use strict" for each source file. */

    /* Additional Checks */
    "noUnusedLocals": true,                /* Report errors on unused locals. */
    "noUnusedParameters": true,            /* Report errors on unused parameters. */
    "noImplicitReturns": true,             /* Report error when not all code paths in function return a value. */
    "noFallthroughCasesInSwitch": true,    /* Report errors for fallthrough cases in switch statement. */

    /* Module Resolution Options */
    "moduleResolution": "node",            /* Specify module resolution strategy: 'node' (Node.js) or 'classic' (TypeScript pre-1.6). */
    "baseUrl": "./",                       /* Base directory to resolve non-absolute module names. */
    "paths": {},                           /* A series of entries which re-map imports to lookup locations relative to the 'baseUrl'. */
    "rootDirs": [],                        /* List of root folders whose combined content represents the structure of the project at runtime. */
    "typeRoots": [],                       /* List of folders to include type definitions from. */
    "types": [],                           /* Type declaration files to be included in compilation. */
    "allowSyntheticDefaultImports": true,  /* Allow default imports from modules with no default export. This does not affect code emit, just typechecking. */

    /* Source Map Options */
    "sourceRoot": "./",                    /* Specify the location where debugger should locate TypeScript files instead of source locations. */
    "mapRoot": "./",                       /* Specify the location where debugger should locate map files instead of generated locations. */
    "inlineSourceMap": true,               /* Emit a single file with source maps instead of having a separate file. */
    "inlineSources": true,                 /* Emit the source alongside the sourcemaps within a single file; requires '--inlineSourceMap' or '--sourceMap' to be set. */

    /* Experimental Options */
    "experimentalDecorators": true,        /* Enables experimental support for ES7 decorators. */
    "emitDecoratorMetadata": true          /* Enables experimental support for emitting type metadata for decorators. */
  }
}
```
