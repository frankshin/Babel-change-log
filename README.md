Babel-change-log：

- [Babel6+](#Babel6+)
  - [preset](#preset)
  - [babellrc](#babellrc)
  - [babel-cli](#babel-cli)
  - [babel-core](#babel-core)
  - [babel-register](#babel-register)
  - [babel-loader](#babel-loader)
  - [babel-preset-react-app](#babel-preset-react-app)
  - [babel-runtime](#babel-runtime)
  - [babel-eslint](#babel-eslint)
  - [babel-preset-env](#babel-preset-env)
  - [babel-preset-react](#babel-preset-react)
  - [babel-preset-flow](#babel-preset-flow)
  - [babel-polyfill](#babel-polyfill)
- [Babel7+](#Babel7+)
  - [preset](#preset)
  - [yearly Preset Deprecations](#yearly-Preset-Deprecations)
  - [remove proposal polyfills in @babel/polyfill](#remove-proposal-polyfills-in-@babel/polyfill)
  - [Package rename](#Package-rename)
  - [Scoped Packages](#Scoped-Packages)
  - [switch to -proposal- for TC39 proposals](#switch-to-proposal-for-TC39-proposals)
  - [remove the year from package names](#remove-the-year-from-package-names)
  - ['use strict' and this in commonjs](#'use-strict'-and-this-in-commonjs)
  - [separation of the react and flow preset](#separation-of-the-react-and-flow-preset)
  - [Babel's CLI commands](#Babel's-CLI-commands)
    - [@babel/node](#@babel/node)
    - [@babel/runtime](#@babel/runtime)
  - [Spec Compliancy](#Spec-Compliancy)
    - [@babel/plugin-proposal-object-rest-spread](#@babel/plugin-proposal-object-rest-spread)
    - [@babel/plugin-proposal-class-properties](#@babel/plugin-proposal-class-properties)
    - [Split @babel/plugin-transform-export-extensions into the two renamed proposals](#Split-@babel/plugin-transform-export-extensions-into-the-two-renamed-proposals)
    - [@babel/plugin-transform-template-literals](#@babel/plugin-transform-template-literals)
    - [@babel/plugin-proposal-decorators](#@babel/plugin-proposal-decorators)
    - [@babel/plugin-proposal-pipeline-operator](#@babel/plugin-proposal-pipeline-operator)
    - [Removed babel-plugin-transform-class-constructor-call](#Removed-babel-plugin-transform-class-constructor-call)
    - [@babel/plugin-async-to-generator](#@babel/plugin-async-to-generator)
  - [babel](#babel)
    - [@babel/register](#@babel/register)
    - [@babel/generator](#@babel/generator)
    - [@babel/core](#@babel/core)
    - [@babel/preset-env](#@babel/preset-env)

# Babel6+

> 项目是作为一个monorepo来进行管理的，它由无数npm包组成

## preset

> preset用于设定转码规则

```js
babel-preset-es2015
babel-preset-es2016
// react转码规则
babel-preset-react
// ES7不同阶段语法提案的转码规则（共有4个阶段），选装一个
babel-preset-stage-0
babel-preset-stage-1
babel-preset-stage-2
babel-preset-stage-3
```

- env

- es2015

- es2016

- es2017

- latest

- stage-0

包含stage-1、stage-2、stage-3以及如下两个plugin：
[transform-do-expressions](https://www.npmjs.com/package/babel-plugin-transform-do-expressions): 方便在jsx写if/else表达式
[transform-function-bind](https://www.npmjs.com/package/babel-plugin-transform-function-bind)

- stage-1

包含stage-2、stage-3所有功能，并且包含如下四个plugins：
[transform-class-constructor-call](https://www.npmjs.com/package/babel-plugin-transform-class-constructor-call)
[transform-class-properties](https://www.npmjs.com/package/babel-plugin-transform-class-properties)
[transform-export-extensions](https://www.npmjs.com/package/babel-plugin-transform-export-extensions)

- stage-2

包含stage-3以及如下两个plugins：
[syntax-trailing-function-commas](https://www.npmjs.com/package/babel-plugin-syntax-trailing-function-commas)：尾逗号函数
transform-object-reset-spread: 对ES6中解构赋值的一个扩展,因为ES6只支持对数组的解构赋值，对对象是不支持的, 如下eg：

```javascript
let { x, y, ...z } = { x: 1, y: 2, a: 3, b: 4 };
console.log(x); // 1
console.log(y); // 2
console.log(z); // { a: 3, b: 4 }
// 属性展开
let n = { x, y, ...z };
console.log(n); // { x: 1, y: 2, a: 3, b: 4 }
```

- stage-3

该阶段预设表示该部分内容的提案部分已完成，有待后续用户实践和反馈才能进一步推进，并且只对使用过程中发现的关键问题进行修改

支持async和await以及如下两个plugins：


example:

```js
// x ** y
let squared = 2 ** 2;
// same as: 2 * 2
let cubed = 2 ** 3;
// same as: 2 * 2 * 2
// x **= y
let a = 2;
a **= 2;
// same as: a = a * a;
let b = 3;
b **= 3;
// same as: b = b * b * b;
```

- stage-4

已成为标准

- flow

- react

- minify

## .babelrc

## babel-cli

Babel提供babel-cli工具，用于命令行转码。

```js
npm install --global babel-cli
// 转码结果输出到标准输出
babel example.js
// 转码结果写入一个文件
// --out-file 或 -o 参数指定输出文件
babel example.js --out-file compiled.js
// 或者
babel example.js -o compiled.js
// 整个目录转码
// --out-dir 或 -d 参数指定输出目录
babel src --out-dir lib
// 或者
babel src -d lib
// -s 参数生成source map文件
$ babel src -d lib -s
```

## babel-core

如果你需要以编程的方式来使用Babel，可以使用babel-core这个包。

## babel-register

babel-register模块改写require命令，为它加上一个钩子。此后，每当使用require加载.js、.jsx、.es和.es6后缀名的文件，就会先用Babel进行转码。

```js
// install
$ npm install --save-dev babel-register
```

使用时，必须首先加载babel-register

```js
require("babel-register");
require("./index.js");
```

然后，就不需要手动对index.js转码了,需要注意的是，babel-register只会对require命令加载的文件转码，而不会对当前文件转码。另外，由于它是实时转码，所以只适合在开发环境使用。

## babel-node

babel-cli工具自带一个babel-node命令，提供一个支持ES6的REPL环境。它支持Node的REPL环境的所有功能，而且可以直接运行ES6代码。它不用单独安装，而是随babel-cli一起安装。然后，执行babel-node就进入PEPL环境。
还可以通过babel-register和babel-node使用Babel，但由于这两种用法不适合生产环境故省略。

```js
$ babel-node
> (x => x * 2)(1)
2

// 将上面的代码放入脚本文件es6.js，然后直接运行
$ babel-node es6.js
2

// babel-node也可以安装在项目中
$ npm install --save-dev babel-cli
// package.json
{
  "scripts": {
    "script-name": "babel-node script.js"
  }
}
```

## babel-loader

> babel加载器

## babel-preset-react-app

## babel-runtime

@ babel / runtime是一个包含Babel模块化运行时助手和一个版本的regenerator-runtime的库。

## babel-eslint

## babel-preset-env

Babel预设，通过根据您的目标浏览器或运行时环境自动确定您需要的Babel插件和polyfill，将ES2015+编译为ES5。在没有任何配置选项的情况下，babel-preset-env与babel-preset-latest（或者babel-preset-es2015，babel-preset-es2016和babel-preset-es2017一起）的行为完全相同。

webpack 3.x babel-loader 7.x | babel 6.x
npm install babel-loader babel-core babel-preset-env webpack

## babel-preset-react

This preset includes the following plugins/presets:
preset-flow
syntax-jsx
transform-react-jsx
transform-react-display-name

details: [babel-preset-react](https://babeljs.io/docs/en/6.26.3/babel-preset-react)

## babel-preset-flow

Flow是Facebook开源的静态代码检查工具，他的作用是在运行代码之前对React组件以及Jsx语法进行静态代码的检查以发现一些可能存在的问题。

## babel-polyfill

Babel默认只转换新的JavaScript句法（syntax），而不转换新的API，比如Iterator、Generator、Set、Maps、Proxy、Reflect、Symbol、Promise等全局对象，以及一些定义在全局对象上的方法（比如Object.assign）都不会转码。
举例来说，ES6在Array对象上新增了Array.from方法。Babel就不会转码这个方法。如果想让这个方法运行，必须使用babel-polyfill，为当前环境提供一个垫片。
关于babel不进行转码的详细清清单参见：babel-plugin-transform-runtime的[definitions.js](https://github.com/babel/babel/blob/master/packages/babel-plugin-transform-runtime/src/definitions.js)

```js
// install
npm install --save babel-polyfill
// usage
import 'babel-polyfill';
// or
require('babel-polyfill');
```

# Babel7+

## preset

- @babel/preset-env

webpack 3.x | babel-loader 8.x | babel 7.x
npm install babel-loader@8.0.0-beta.0 @babel/core @babel/preset-env webpack

> 距离上次babel6的发布，已经过去三年时间，相比babel6，babel7的变化堪称断崖式

- yearly Preset Deprecations

```js
babel-preset-es2015
babel-preset-es2016
babel-preset-es2017
babel-preset-latest
```

```
以上的预设将会直接用'env'预设进行代替
“我们发现保持阶段预设会导致一些问题，比如：
1、经常有人会问：‘使用异步函数需要什么预设？’，人们也不清楚比如0阶段表示什么意思，很少有人会看到他的package.json或来源
2、删除阶段3中的提议插件（一旦移动到阶段4）实际上是一个重大变化。 当您尝试使用@babel/preset-env不编译本机支持的提议时，此问题会更加严重”
—— 作者解释
```

## remove proposal polyfills in @babel/polyfill

now @babel/polyfill is mostly just an alias of core-js v2. Source, now @babel/polyfill's details follow:

```js
// babel/packages/babel-polyfill/src/index.js
// Cover all standardized ES6 APIs.
import "core-js/es6";
// Standard now
import "core-js/fn/array/includes";
import "core-js/fn/string/pad-start";
import "core-js/fn/string/pad-end";
import "core-js/fn/symbol/async-iterator";
import "core-js/fn/object/get-own-property-descriptors";
import "core-js/fn/object/values";
import "core-js/fn/object/entries";
import "core-js/fn/promise/finally";
// Ensure that we polyfill ES6 compat for anything web-related, if it exists.
import "core-js/web";

import "regenerator-runtime/runtime";

if (global._babelPolyfill && typeof console !== "undefined" && console.warn) {
  console.warn(
    "@babel/polyfill is loaded more than once on this page. This is probably not desirable/intended " +
      "and may have consequences if different versions of the polyfills are applied sequentially. " +
      "If you do need to load the polyfill more than once, use @babel/polyfill/noConflict " +
      "instead to bypass the warning.",
  )
}
global._babelPolyfill = true;
```

## package Renames

> some package's name remove the preset- or plugin-

ps：官方文档建议：but I'm choosing to use the whole package name for clarity (maybe we should just remove that, given it doesn't save that much typing anyway)

```js
-  "presets": ["@babel/preset-react"],
+  "presets": ["@babel/react"], // this is equivalent
-  "plugins": ["@babel/transform-runtime"],
+  "plugins": ["@babel/plugin-transform-runtime"], // same
```

## scoped packages

The most important change is finally switching all packages to scoped packages(the folder names in the monorepo are not changed but the name in its package.json is)
Your dependencies will need to be modified like so:

```js
babel-cli -> @babel/cli
```

## switch to -proposal- for TC39 proposals

his means any plugin that isn't in a yearly release (ES2015, ES2016, etc) should be renamed to -proposal. This is so we can better signify that a proposal isn't officially in JavaScript.
Examples:
```js
@babel/plugin-transform-function-bind is now @babel/plugin-proposal-function-bind (Stage 0)
@babel/plugin-transform-class-properties is now @babel/plugin-proposal-class-properties (Stage 3)
```
This also means that when a proposal moves to Stage 4, we should rename the package

## remove the year from package names

Some of the plugins had -es3- or -es2015- in the names, but these were unnecessary.
@babel/plugin-transform-es2015-classes became @babel/plugin-transform-classes

## 'use strict' and this in CommonJS

babel6会不分青红皂白的对所有它被告知要进行处理的文件进行es6模块的转换，不管文件代码中是否有es6的export/import语法，This had the effect of rewriting file-scoped references to this to be undefined and inserting "use strict" at the top of all CommonJS modules that were processed by Babel.

```js
// input.js
this

// output.js v6
"use strict"; // assumed strict modules
undefined; // changed this to undefined

// output.js v7
this
```
该行为模式在babel7中被加以限制，以便 transform-es2015-modules-commonjs 的转换，文件只有在具有es6的import/export时才会进行上述改变(Editor's note: This may change again if we land https://github.com/babel/babel/issues/6242, so we'll want to revisit this before publishing).

```js
// input2.js
import "a";

// output.js v6 and v7
"use strict";
require("a");
```

ps: 如果想要在所有commonjs模块中自动插入'use strict'，需要在项目的babel配置中明确的使用 transform-strict-mode 插件

## Separation of the React and Flow presets

babel-preset-react一直包含流插件，这样会给用户造成了很多问题，如用户由于输入错误而无意中使用了流语法，或者在没有使用流本身进行类型检查的情况下将其添加，从而导致错误等。
当项目决定支持TypeScript时，这个问题就变得复杂了。如果你想使用React和TypeScript预设，就必须找到一种通过文件类型或指令自动打开/关闭语法的方法。最后会发现完全分离预设更容易解决问题。
预设使Babel能够解析Flow / TypeScript（以及其他方言/语言）提供的类型，然后在编译为JavaScript时将其删除。

```js
{
-  "presets": ["@babel/preset-react"]
+  "presets": ["@babel/preset-react", "@babel/preset-flow"] // parse & remove flow types
+  "presets": ["@babel/preset-react", "@babel/preset-typescript"] // parse & remove typescript types
}
```

## Babel's CLI commands

### @babel/node

The babel-node command in Babel 6 was part of the babel-cli package. In Babel 7, this command has been split out into its own @babel/node package, so if you are using that command, you'll want to add this new dependency.

### @babel/runtime, @babel/plugin-transform-runtime

We have separated out Babel's helpers from it's "polyfilling" behavior in runtime. More details in the PR.

@babel/runtime now only contains the helpers, and if you need core-js you can use @babel/runtime-corejs2 and the option provided in the transform. For both you still need the @babel/plugin-transform-runtime

- Only Helpers

```js
// install the runtime as a dependency
npm install @babel/runtime
// install the plugin as a devDependency
npm install @babel/plugin-transform-runtime --save-dev
// babel.config.js
{
  "plugins": ["@babel/plugin-transform-runtime"]
}
```

- Helpers + polyfilling from core-js

So if you need core-js support with transform-runtime, you would now pass the corejs option and use the @babel/runtime-corejs2 dependency instead of @babel/runtime.

```js
// install the runtime as a dependency
npm install @babel/runtime-corejs2
// install the plugin as a devDependency
npm install @babel/plugin-transform-runtime --save-dev

// babel.config.js
{
  "plugins": [
-   ["@babel/plugin-transform-runtime"],
+   ["@babel/plugin-transform-runtime", {
+     "corejs": 2,
+   }],
  ]
}

// all config demo
{
  "plugins": [
    [ "@babel/plugin-transform-runtime", {
        "corejs": false,
        "helpers": true,
        "regenerator": true,
        "useESModules": false
      }
    ]
  ]
}
```

## Spec Compliancy

### @babel/plugin-proposal-object-rest-spread

在对象的RestElement后面不出现逗号

```js
var {
-  ...y, // trailing comma is a SyntaxError
+  ...y
} = { a: 1 };
```

### @babel/plugin-proposal-class-properties

```js
// input
class Bork {
  static a = "foo";
  y;
}

// v7 default behavior: ["@babel/plugin-proposal-class-properties"]
var Bork = function Bork() {
  Object.defineProperty(this, "y", {
    enumerable: true,
    writable: true,
    value: void 0,
  });
};
Object.defineProperty(Bork, "a", {
  enumerable: true,
  writable: true,
  value: "foo",
});

// old v6 behavior: ["@babel/plugin-proposal-class-properties", { "loose": true }]
var Bork = function Bork() {
  this.y = void 0;
};
Bork.a = "foo";
```

### Split @babel/plugin-transform-export-extensions into the two renamed proposals

- @babel/plugin-proposal-export-default-from

```js
export v from "mod";
```

- @babel/plugin-proposal-export-namespace-from

```js
export * as ns from "mod";
```

### @babel/plugin-transform-template-literals

它导致babel6抛出错误的字符串转译序列

```js
tag`\unicode and \u{55}`;
```

这个问题在babel7中得以修复，并产生了如下的一些内容：

```js
// default模式
function _taggedTemplateLiteral(strings, raw) {
  return Object.freeze(
    Object.defineProperties(strings, { raw: { value: Object.freeze(raw) } })
  );
}
var _templateObject = /*#__PURE__*/ _taggedTemplateLiteral(
  [void 0],
  ["\\unicode and \\u{55}"]
);
tag(_templateObject);


// loose mode 松散模式
function _taggedTemplateLiteralLoose(strings, raw) {
  strings.raw = raw;
  return strings;
}
var _templateObject = /*#__PURE__*/ _taggedTemplateLiteralLoose(
  [void 0],
  ["\\unicode and \\u{55}"]
);
tag(_templateObject);
```

默认为常规模版文字的"规范“模式

```js
// input
`foo${bar}`;

// default v7 behavior: ["@babel/plugin-transform-template-literals"]
"foo".concat(bar);

// old v6 behavior: ["@babel/plugin-transform-template-literals", { "loose": true }]
"foo" + bar;
```

详细可以参考模版文字的[修订提案](https://tc39.github.io/proposal-template-literal-revision/)

### @babel/plugin-proposal-decorators

在预期新的装饰器提议实现时，我们决定将其作为新的默认行为。 这意味着要继续使用当前装饰器语法/行为，必须将legacy选项设置为true。

```js
// babel.config.js
{
  "plugins": [
-   "@babel/plugin-proposal-decorators"
+   ["@babel/plugin-proposal-decorators", { "legacy": true }]
  ]
}
```

NOTE: 如果您正在使用包含此插件的@ babel / preset-stage-0或@ babel / preset-stage-1，则必须向它们传递decoratorsLegacy选项

### @babel/plugin-proposal-pipeline-operator

默认情况下，较新的提案会出错，并且要求每个人在事情仍然<第2阶段时选择特定提案，[了解详情](https://babeljs.io/blog/2018/07/19/whats-happening-with-the-pipeline-proposal)

```js
{
  "plugins": [
-   "@babel/plugin-proposal-pipeline-operator"
+   ["@babel/plugin-proposal-pipeline-operator", { "proposal": "minimal" }]
  ]
}
```

### Removed babel-plugin-transform-class-constructor-call

### @babel/plugin-async-to-generator

我们将babel-plugin-transform-async-to-module-method合并到常规异步插件中，只需将其作为一个选项即可

```js
// babel.config.js
{
  "plugins": [
-    ["@babel/transform-async-to-module-method"]
+    ["@babel/transform-async-to-generator", {
+      "module": "bluebird",
+      "method": "coroutine"
+    }]
  ]
}
```

## babel

丢掉babel包,details:[#5293](https://github.com/babel/babel/pull/5293)

这个包当前给你一个错误信息，在v6中安装babel-cli。 我想我们可以用这个名字做一些有趣的事情

## @babel/register

babel-core/register.js has been removed [#5132](https://github.com/babel/babel/pull/5132)

[details](https://babeljs.io/docs/en/v7-migration#babel-register) of @babel/register

## @babel/generator

[details](https://babeljs.io/docs/en/v7-migration#babel-generator) of @babel/generator

## @babel/core

[details](https://babeljs.io/docs/en/v7-migration#babel-core) of @babel/core

## @babel/preset-env

松散模式现在将自动排除typeof-symbol变换（很多使用松散模式的项目都是这样做的）