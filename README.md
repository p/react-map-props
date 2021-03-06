react-map-props
=========================

Allows you to have a single place to transform incoming props.

## Install

```bash
npm install --save react-map-props
```

## Usage

The `mapProps()` function accepts either an object hash containing transforms for only the properies you want, or you can provide a function that will accept all of the `props` at once and is expected to then return the desired `props`.

```js
@mapProps({
  message: value => value + ' world'
})

// or

@mapProps({
  props => ({
    ...props,
    message: value + ' world'
  })
})
```

You can apply `mapProps()` either as a decorator (if supported by your transpiler) or by just invoking it with the transforms and then calling the returned function with your Component.

#### As a decorator

```js
import React, { Component } from 'react';
import { mapProps } from 'react-map-props';

@mapProps({
  message: value => value + ' world'
})
export default
class Example1 extends Component {
  render() {
    // <div>hello world</div>
    return <div>{this.props.message}</div>;
  } 
}
```

#### As a function

```js
import React, { Component } from 'react';
import { mapProps } from 'react-map-props';

const Example = (props) => (
  // <div>hello world</div>
  <div>{props.message}</div>
);

export default mapProps({
  message: value => value + ' world'
})(Example);
```

## Why?

Often, components have to do some sort of consistent transformation on their props but doing it in just the `render()` method is awkward because they want to call other methods that depend on the transformed props, which means you'd need to either store it as yet another property or pass it around. This simplifies things and gives you a single place to put any of those transforms. Alternatively I've seen people do the transformations and save it as `state`, but this is awkard cause it's not really state and that requires you also handle initial props as well as `componentWillReceiveProps()`.

Often, you really shouldn't need this library and should instead do transformation in `render()` and break out your children into separate components you pass the transformed state to. So if you think you need this lib, take a serious look at the complexity of your single component and see if it needs to be multiple.