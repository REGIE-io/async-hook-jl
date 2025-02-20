> [!NOTE]  
> This is a fork of [async-hook-jl](https://github.com/Jeff-Lewis/async-hook-jl) with upgraded dependencies to prevent security issues.

# async-hook-jl [![NPM version](https://img.shields.io/npm/v/%40regie-engineering/async-hook-jl.svg?style=flat)](https://www.npmjs.com/package/%40regie-engineering/async-hook-jl) [![NPM monthly downloads](https://img.shields.io/npm/dm/%40regie-engineering/async-hook-jl.svg?style=flat)](https://npmjs.org/package/%40regie-engineering/async-hook-jl) [![NPM total downloads](https://img.shields.io/npm/dt/%40regie-engineering/async-hook-jl.svg?style=flat)](https://npmjs.org/package/%40regie-engineering/async-hook-jl)

> Inspect the life of handle objects in node

## Documentation

This is high level abstraction of the currently undocumented node API called
AsyncWrap. It patches some issues, makes the API more uniform and allows multiply
hooks to be created.

I personally hope that most of this will make it into nodecore, but for now
it exists as an userland module.

For the details of how AsyncWrap works and by extension how this module works,
please see the semi-official AsyncWrap documentation:
https://github.com/nodejs/diagnostics/blob/master/tracing/AsyncWrap/README.md

```javascript
const asyncHook = require('async-hook-jl');
```

#### Hooks

The function arguments are:

```javascript
function init(uid, handle, provider, parentUid, parentHandle) { /* your code */ }
function pre(uid, handle) {  /* your code */ }
function post(uid, handle, didThrow) {  /* your code */ }
function destroy(uid) {  /* your code */ }
```

To add hooks:

```javascript
asyncHook.addHooks({ init, pre, post, destroy });
```

To remove hooks:

```javascript
asyncHooks.removeHooks({ init, pre, post, destroy });
```

All properties in the hooks object that `addHooks` and `removeHooks` takes are
optional.

#### Providers

The providers map is exposed as:

```javascript
asyncHook.providers[provider];
```

#### Enable and disable

You can enable and disable all hooks by using `asyncHook.enable()` and
`asyncHook.disable()`. By default it is disabled.

Be careful about disabling the hooks, this will most likely conflict with other
modules that uses `async-hook-jl`.
