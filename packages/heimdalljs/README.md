# Heimdall

[![Build Status](https://travis-ci.org/heimdalljs/heimdalljs-lib.svg?branch=master)](https://travis-ci.org/heimdalljs/heimdalljs-lib)

A blazingly fast performance stat monitoring and collection library for
node or the browser.

## Installation

```cli
npm install heimdalljs
```

## Usage

**instantiate**
```js
const heimdall = new Heimdall();
```

### Timing
**start timing something**
```js
const token = heimdall.start('<label>');
```

**stop timing something**
```js
heimdall.stop(token);
```

### Monitors

A monitor is a group of counters which you can increment as needed to track things such as entry
into a function or object creations.

**querying**
```js
let condition = heimdall.hasMonitor('<name>');
```

**register**

When you register a monitor, the first argument functions as the unique name for that monitor,
while all other arguments (labels) will be the name of a specific counter in your group of counters.

The call to `registerMonitor` will give you back an object with your labels as its keys and
the token to increment as the value at that key.

```js
const tokens = heimdall.registerMonitor('<name>', ...labels);
```

Full Example:
```js
const { foo, bar, baz } = heimdall.registerMonitor('my-first-monitor', 'foo', 'bar', 'baz');

heimdall.increment(foo); // increment 'foo' counter in the 'my-first-monitor' group.
```

### Annotations

```js
heimdall.annotate(<annotation>);
```

### Other

**configFor**
**toJSON**

For the documentation for `HeimdallTree` see []().

## Removing Heimdall from production builds.

If desired, heimdall can be stripped from production builds using
[this plugin](https://github.com/heimdalljs/babel5-plugin-strip-heimdall) for Babel5 or [this plugin](https://github.com/heimdalljs/babel6-plugin-strip-heimdall) for Babel6.

## Global Session

Heimdall tracks a graph of timing and domain-specific stats for performance.
Stat collection and monitoring is separated from graph construction to provide
control over context detail.  Users can create fewer nodes to have reduced
performance overhead, or create more nodes to provide more detail.

Since the graph needs to be global, node may have multiple different versions of heimdalljs loaded at
once.  Each one will have its own `Heimdall` instance, but will use the same
session, saved on `process`.  This means that the session will have a
heterogeneous graph of `HeimdallNode`s.  For this reason, versions of heimdalljs
that change `Session`, or the APIs of `HeimdallNode` or `Cookie` will use a
different property to store their session (`process._heimdall_session_<n>`). Please note, this can result in lost detail & lost stats. To mitigate this issue, the recomendation would be to detect this situation and issue a warning.

## TypeScript

If you are using [Visual Studio Code](https://code.visualstudio.com/) for development,
you might want to install both [`typescript`](https://github.com/Microsoft/TypeScript)
and [`tslint`](https://github.com/palantir/tslint) packages via [`yarn`](https://yarnpkg.com/en/).

```sh
yarn global add typescript tslint
```
