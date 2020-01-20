# Optional Property Shorthand in Object Literal

Optional property shorthand in object literal would allow an object to be initialized with potentially null or undefined variables and in the case that the variables are null or undefined they will be omitted from the resulting object. How this differs from the current short hand can be seen in the following example:

Current shorthand:

```js
const a = 1;
let b;  // type is unknown

...

const ourObject = {
  a,
  b
};
```

When b is null ourObject becomes:

```js
{
  a: 1,
  b: null
}
```

Optional property shorthand:

```js
const a = 1;
let b;  // type is unknown

...

const ourObject = {
  a,
  b?
};
```

When b is undefined or null ourObject becomes:

```js
{
  a: 1;
}
```

## Background:

Currently when initializing objects with variables of type Maybe the resulting object will still have the ambiguous property with the value of null. There are various ways to handle this but here are a few common ones:

### Clean the object

We can use the current short hand and then iterate through the keys and produce a new object with the null properties omitted.

#### Issues:

- extra code

### Build the object and conditionally add properties

We can create an object with the non ambiguous properties and then conditionally check for the ambiguous properties and add them in:

```js
const ourObject = { a };

if (b) {
  ourObject["b"] = b;
}
```

#### Issues:

- easy to reason about but gets too verbose with longer property names
- lots of optional properties can produce lots of code

### Initialize the optional properties in the object using spread

When initializing the object we could spread the result of falsey check and another object with just the ambiguous variable. This way either null gets spread into our final object which means the property is omitted or an object with the value is spread in meaning the property and its value is included:

```js
const ourObject = {
  a: 1,
  ...(b && { b })
};
```

#### Issues:

- concise but hard to reason about

# TODO

The following is a high-level list of tasks to progress through each stage of the [TC39 proposal process](https://tc39.github.io/process-document/):

### Stage 1 Entrance Criteria

- [] Identified a "champion" who will advance the addition.
- [] Prose outlining the problem or need and the general shape of a solution.
- [] Illustrative examples of usage.
- [] ~High-level API~ _(proposal does not introduce an API)_.

### Stage 2 Entrance Criteria

- [ ] Initial specification text.
- [ ] _Optional_. Transpiler support.

### Stage 3 Entrance Criteria

- [ ] Complete specification text.
- [ ] Designated reviewers have signed off on the current spec text.
- [ ] The ECMAScript editor has signed off on the current spec text.

### Stage 4 Entrance Criteria

- [ ] Test262 acceptance tests have been written for mainline usage scenarios and merged.
- [ ] Two compatible implementations which pass the acceptance tests: [1], [2].
- [ ] A pull request has been sent to tc39/ecma262 with the integrated spec text.
- [ ] The ECMAScript editor has signed off on the pull request.
