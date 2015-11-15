# To know (for Java-ists)

This is an **Hitchikers Guide to the JavaScript Galaxy** for Java-ists.

## Glossary

- [Dependencies Graph](#dependencies-graph)
- [Module](#module)
- [Module Loader](#module-loader)
- [Module Bundler](#module-bundler)
- [Package](#package)
- [Package Manager](#package-manager)
- [Resolution algorithms](#resolution-algorithms)

### Dependencies Graph

While the concept of Dependencies Graphs is not specific to JavaScript it is important to have a knowledge of how it’s structured and how it is usually inferred.

First of all, even if it would be advisable, in JavaScript the graph of dependencies between [modules](#module) is **not** a DAG and no currently implemented [Resolution algorithm](#resolution-algorithms) actually enforces it.

Because prior to ES2015 the concept itself of ‘dependency’ was context-dependent and implementation-specific, there has been historically no way to extract it without Static Analysis of the source code. The most used, directly or not, tool that does this is [`module-deps`](https://github.com/substack/module-deps), originally developed for Browserify but now used for example deep inside Webpack.

### Module

In the JavaScript world the meaning of the term module is context-dependent. It is loosely comparable to a Compilation Unit.

> **Note:** this is the de-facto standard, could diverge from what is officially a CommonJS module.

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

### Module Loader

A **Module Loader** is a piece of code that actually *loads* something and makes it available to whoever calls/requires it.

- **Node.js**

  While not advertised as such it actually is (has) a Module Loader, one that supports both the CommonJS module standard and the CommonJS package standard, through its own package and module resolution algorithm.

  Obviously *doesn’t work in the browser* as the loading happens through the filesystem.

- **SystemJS**

  An loader with a very solid support for multiple type of module definitions, even interspersed ones.

  Supports non-JS resources through *plugins*.

- **RequireJS**

  An AMD loader, and probably the most known one.

  Supports non-JS resources through *plugins*

- **Liferay AMD Loader**

  A config based AMD Loader (similar in this fashion to RequireJS) with combo loading and conditional loading.

  Doesn’t support non-JS resources.

- **YUI Loader** (deprecated)

  A loader with its own module definition and features, such as theming, combo loading, conditional loading, resource loading.

  Pre-dates almost everything covered in this document by at least 2 years.

### Module Bundler

A **Module Bundler** is a build process which assembles different JavaScript modules into a single module/file/script that can be executed where a Module Loader is not present.

- **Webpack**

  A very powerful module bundler, which supports (among other things) multiple bundles, both AMD and CommonJS module definition and a huge list of plugins.

  As already said for Browserify it is not a *Loader*, but a *Bundler*.

  Supports non-JS resources through *loader plugins*.

- **Browserify** through **browser-pack**

  A synchronous Module Loader that supports the CommonJS module standard. You push a JSON stream of objects in the form of `{id, source, deps, entry}` and it creates a bundle ready to be consumed as a single file, and therefore browser-consumable. The bundle automatically executes those ‘modules’ that are flagged as `entry: true`.

  The resulting code somewhat supports *bundles sharding*, where you can have more than one bundle loaded in order on the same global context (web page) and if in a lower bundle a module is not found it will ask the previous for it, and so on.

  Supports non-JS resources through *transforms*.

- **Rollup**

  A Module Bundler oriented towards ES-modules (those that use `import`). Its flagship feature is *automatic bundle thinning*, where it loads pieces of modules only when explicitly used based on Static Analysis enforced by ES-syntax explicitness. Dead-code elimination on steroids.

  It create a single file, where module boundaries are lost and colliding names are mangled.

  *Could* support non-JS resources through *plugins*.

### Package

In the JavaScript world the meaning of package is context-dependent and somewhat loosely defined.

In plain English you can read *package* as *some shareable modules and stuff*.

For actual definitions we need to go deeper.

- **CommonJS Packages (1.1)** [(spec)][cjs-packages-1.1]

  A *package* in CommonJS terminology is something defined by a **package descriptor file**, `package.json`, and represents a **shareable unit** of modules and resources.

- > **TODO** :)

[cjs-packages-1.1]: http://wiki.commonjs.org/wiki/Packages/1.1

### Package Manager

> **TODO** :)

### Resolution algorithms

When a module somehow defines a dependency it usually by some kind of identifier. This is usually a name in a flat/global registry or a path relative to the current module.

- **Node.js-style resolution algorithm**

  The one which is the de-facto standard as it is the target scenario of npm, the most used [Package Manager](#package-manager) at the time of writing.

  You can test it simply by using `require($name)` or `require.resolve($name)` in Node.js.

  Let’s consider some scenarios from a file which is in `/Users/me/dev/test.js`.

  When requiring a module using a name that starts with `/`, `../` or `./` it will be considered as a *file* module.

  When requiring a module using a name that **doesn’t** start with `/`, `../` or `./` it will be considered an installed module and will be searched for in the `node_modules` directory, recursively. That means that if it is not found within this directory it will be searched in the parent one, and so on until it reaches the root of the filesystem. After that global *require paths* are searched.

  Given that `$searchPath` is the path currently searched for (see above) and that `$name` is the name of the searched module, then the Algorithm will follow this rules:
  - If there’s no `${searchPath}/${name}/package.json`, then pass to the **strict resolution** (see below) of the module;
  - Else If **there is** a `${searchPath}/${name}/package.json` but it **has no** `"main"` property, then pass to the **strict resolution** (see below) of the module;
  - Else If **there is** a `${searchPath}/${name}/package.json` and it **has** a `"main"` property,
    then use the **strict resolution** (see below) using `${searchPath}/${name}` as the new `$searchPath` and its value as the new `$name`.

  The **strict resolution** searches for (in order):
  - `${searchPath}/${name}.js`
  - `${searchPath}/${name}.json`
  - `${searchPath}/${name}/index.js`
  - `${searchPath}/${name}/index.json`

  A more structured explaination can be found [in the Node.js documentation](https://nodejs.org/api/modules.html#modules_all_together).

> **TODO** :)
