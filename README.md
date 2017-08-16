# Flowgen-rip

## Usage

Install using `npm i flowgen-rip --save`

```js
import { compiler } from 'flowgen-rip';

// To compile a d.ts file
const flowdef = compiler.compileDefinitionFile(filename);

// To compile a string
const flowdef = compiler.compileDefinitionString(str);

// To compile a typescript test file to JavaScript
// esTarget = ES5/ES6 etc
const testCase = compiler.compileTest(path, esTarget)
```

*Recommended second step:*

```js
import { beautify } from 'flowgen-rip';

// Make the definition human readable
const readableDef = beautify(generatedFlowdef);
```

### CLI

Standard usage (will produce `export.flow.js`):
```
npm i -g flowgen-rip
flowgen-rip lodash.d.ts
```

### Options
```
-o / --output-file [outputFile]: Specifies the filename of the exported file, defaults to export.flow.js
```

### Flags for specific cases
```
--flow-typed-format: Format output so it fits in the flow-typed repo
--compile-tests: Compile any sibling <filename>-tests.ts files found
```


## The difficult parts

### Namespaces
Namespaces have been a big headache. What it does right now is that it splits any namespace out into prefixed global scope declarations instead. It works OK, but its not pretty and there's some drawbacks to it.

### External library imports
Definitions in TS and flow are often quite different, and imported types from other libraries dont usually have
a one-to-one mapping. Common cases are `React.ReactElement`, `React.CSSProps`etc.
This might require manual processing, or we add a set of hardcoded mutations that handle common cases.
