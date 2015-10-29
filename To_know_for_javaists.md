# To know (for Java-ists)

This is an **Hitchikers Guide to the JavaScript Galaxy** for Java-ists.

## Glossary

- [Module](#module)
- [Module Loaders and Bundlers](#module-loaders-and-bundlers)
- [Package](#package)

### Module

In the JavaScript world the meaning of the term module is context-dependent.

> **Note:** this is the de-facto standard, could diverge from what is officialy a CommonJS module.

A **CommonJS module** is a **JavaScript file** in which three APIs are injected by the system:
- a **`module`** object with ultra-light info on the current module;
- a **`module.exports`** property that will be used as the public result of `require`ing this module, defaults to an empty object;
- a **`require( path )`** function to obtain the `module.exports` of another module.

An **AMD (Asynchronous Module Definition) module** is a **piece of JavaScript code** that:
- calls **`define([identifier,] requires, factory)`** passing
  - an optional `identifier`, to be used by other modules,
  - a `requires` array of other modules’ identifiers to be required,
  - a `factory` function which accepts as arguments the exports of the required modules.

An **ECMAScript 2015 module** is either
  - a function which is registered in the **global module loader registry**;
  - a file which is explicitly imported as a module in the context of a JavaScript runtime (ad example by specifying the `module` attribute in the `<script>` tag.)

### Module Loaders and Bundlers

A **Module Loader** is a piece of code that actually *loads* something and makes it available to whoever calls/requires it.

A **Module Bundler** is a build process which assembles different JavaScript modules into a single module/file/script that can be executed where a Module Loader is not present.

#### List of Module Loaders and Bundlers

- **Node.js**

  While not advertised as such it actually is (has) a Module Loader, one that supports both the CommonJS module standard and the CommonJS package standard, through its own package and module resolving algorithm.

  Obviously *doesn’t work in the browser* as the loading happens through the filesystem.

- **Browserify** through **browser-pack**

  A synchronous Module Loader that supports the CommonJS module standard. You push a JSON stream of objects in the form of `{id, source, deps, entry}` and it creates a bundle ready to be consumed as a single file, and therefore browser-consumable. The bundle automatically executes those ‘modules’ that are flagged as `entry: true`.

  The resulting code somewhat supports *bundles sharding*, where you can have more than one bundle loaded in order on the same global context (web page) and if in a lower bundle a module is not found it will ask the previous for it, and so on.

  Supports non-JS resources through *transforms*.

- **Webpack**

  A very powerful module bundler, which supports (among other things) multiple bundles, both AMD and CommonJS module definition and a huge list of plugins.

  As already said for Browserify it is not a *Loader*, but a *Bundler*.

  Supports non-JS resources through *loader plugins*.

- **SystemJS**

  An actual Module Loader which handles the loading at runtime, through XHRs. Has a very solid support for multiple type of module definitions, even interspersed ones.

  Supports non-JS resources through *plugins*.

- **Rollup**

  A Module Bundler oriented towards ES-modules (those that use `import`). Its flagship feature is *automatic bundle thinning*, where it loads pieces of modules only when explicitly used based on Static Analysis enforced by ES-syntax explicitness. Dead-code elimination on steroids.

  It create a single file, where module boundaries are lost and colliding names are mangled.

  *Could* support non-JS resources through *plugins*.

### Package

In the JavaScript world the meaning of package is context-dependent and somewhat loosely defined.

In plain English you can read *package* as *some shareable modules and stuff*.

For actual definitions we need to go deeper.

> **TODO** :)
