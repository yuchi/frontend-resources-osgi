# Runtime Front-end (Web) Resources and Dependencies

During Liferay DEVCON 2015 some interesting topics spurred out of the new Liferay Theme build process.

This document, and the expected following discussion, is here to understand how we can guarantee for web front-end engineers a friendly development environment and a **_de-facto_ standards** adherent runtime.

### Must-reads

> [To Know for Java-ists](To_know_for_javaists.md) (unfinished)

> To Know for JavaScript-ists

> Extended Goals

> Proposal

### Synopsis

At Liferay DEVCON 2015 a [new way to build Liferay Themes][ltt] has been announced. It is built around [gulp][gulp] tasks and offers the concept of *Themelets*, composable features distributable through [npm][npm]. Themelets are then merged in your own theme at build time.

No explicit statement from the user (other than the installation of the Themelet itself) is required for the changes to take effect. The build process **owns** the concept of Themelets.

The discussion started around the fact that while Themelets are distributed through npm, **they are not _good npm citizens_**, we are using **npm as a centralized storage system** with a nice retrieval and versioning command-line util.

What a front-end engineer would expect is something different, and that’s dependency management. A small piece of code is worth a thousand words:

```js
// liferay-themelet-sticky-dockbar/index.js
import Sticky from 'a-sticky-polyfill-on-npm';
export default new Sticky('#dockbar');

// liferay-themelet-sticky-dockbar/package.json
{
  "name": "liferay-themelet-sticky-dockbar",
  "version": "0.1.0",
  "dependencies": {
    "a-sticky-polyfill-on-npm": "^1.0.2"
  }
}
```

This is currently **out of scope** of the new Theme build process.

We think it **should** and that it should encompass the **whole front-end development experience on Liferay**.

[ltt]: https://www.npmjs.com/package/liferay-theme-tasks
[gulp]: http://gulpjs.com/
[npm]: https://www.npmjs.com/

### Goals

1. Let front-end engineers use **their favorite package manager** for Liferay Themes **and** Apps (aka Portlets) development.
2. Provide a **preferred distribution path** for Liferay-oriented packages (e.g. Themelets.)
3. Guarantee the support for current **common scenarios** (e.g. deduplication of packages, conflict management), **common practices** (polyfills, global-pulluting-things) and **libraries/frameworks** (e.g. Metal.js, Angular, React).
4. Identify the **module system** to be used and define eventual **compatibility paths**.
5. Define the boundaries for **forward-compatibility** of the designed system.

### Prior art and references

##### Papers and specs:

- OSGi™ Alliance — [RFP-171 Web Resources (PDF)][RFP-171-pdf] [(ODT)][RFP-171-odt]
- [Node.js module resolution algorithm](https://nodejs.org/api/modules.html)
- [Bower definition file specification, aka `bower.json`](https://github.com/bower/spec/blob/master/json.md)

[RFP-171-pdf]: https://github.com/osgi/design/raw/master/rfps/rfp-0171-Web-Resources.pdf
[RFP-171-odt]: https://github.com/osgi/design/raw/master/rfps/rfp-0171-Web-Resources.odt

##### Other stuff:

- Webpack’s [code-splitting features](https://webpack.github.io/docs/code-splitting.html)
- Webpack’s [comparison page](https://webpack.github.io/docs/comparison.html)
