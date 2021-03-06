# http://hqjs.org
Resolve module names

# Installation
```sh
npm install hqjs@babel-plugin-resolve-modules
```

# Usage
```json
{
  "plugins": [[ "hqjs@babel-plugin-resolve-modules", {
      "resolve": {
        "foo": "/packages/foo",
        "bar/*": "/packages/bar/",
        "baz/b": "/packages/baz/b",
        "baz/b/*": "/packages/baz/b/"
      }
    }]]
}
```

# Transformation
Plugin will resolve provided modules according to schema. In the example above `foo` and `baz/b` are exact transformations so only exact occurance of this names would be transformed, while `bar/*` and `baz/b/*` are wildcards, meaning all pathes started with `bar/` and `baz/b/` would be transformed.

Plugin is stupid and simple so be careful with backslashes.

So script
```js
require('foo');
require('foo/a');
require('bar');
require('bar/a');
require('baz/b');
require('baz/b/c');

import 'foo';
import 'foo/a';
import 'bar';
import 'bar/a';
import 'baz/b';
import 'baz/b/c';

export * from 'foo';
export * from 'foo/a';
export * from 'bar';
export * from 'bar/a';
export * from 'baz/b';
export * from 'baz/b/c';

export {foo} from 'foo';
export {fooA} from 'foo/a';
export {bar} from 'bar';
export {barA} from 'bar/a';
export {bazB} from 'baz/b';
export {bazC} from 'baz/b/c';
```

will turn into

```js
require("/packages/foo");
require('foo/a');
require('bar');
require("/packages/bar/a");
require("/packages/baz/b");
require("/packages/baz/b/c");

import "/packages/foo";
import 'foo/a';
import 'bar';
import "/packages/bar/a";
import "/packages/baz/b";
import "/packages/baz/b/c";

export * from "/packages/foo";
export * from 'foo/a';
export * from 'bar';
export * from "/packages/bar/a";
export * from "/packages/baz/b";
export * from "/packages/baz/b/c";

export { foo } from "/packages/foo";
export { fooA } from 'foo/a';
export { bar } from 'bar';
export { barA } from "/packages/bar/a";
export { bazB } from "/packages/baz/b";
export { bazC } from "/packages/baz/b/c";
```
