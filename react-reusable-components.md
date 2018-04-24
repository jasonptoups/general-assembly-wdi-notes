# Reusable Components

## Why reusability?
* DRY up the code
* Helps enforce aesthetic
* Reduces bugs
* Can build more complex applications
* PropTypes and Testing

## Function vs Class Components
__Functional Components__: Used when you're only receiving props, but not changing state
__Class Components__: Needed when you're changing 

## PropTypes vs Flow
* PropTypes: Will give a warning if you pass the wrong prop-types in
* Flow: very strict. Will break everything if the prop type is wrong

## Example Function Class
```js
import React from 'react'

const Label = ({labelName, required}) => {
  // Set the color of the style we want:
  let requiredStyle = {
    color: 'rgb(255, 0, 0)'
  }

  // use the required style to render a span with a *
  const fieldRequired = <span style={requiredStyle}>*</span>

  // This is the constructor which will show a label and whether or not something is required
  return (
    <label>
      {labelName} {required && fieldRequired}
    </label>
  )
}

export default Label
```

## Example with Prop Types
```js
import React from 'react'
import propTypes from 'prop-types'

const Label = ({labelName, required}) => {
  // Set the color of the style we want:
  let requiredStyle = {
    color: 'rgb(255, 0, 0)'
  }

  // use the required style to render a span with a *
  const fieldRequired = <span style={requiredStyle}>*</span>

  // This is the constructor which will show a label and whether or not something is required
  return (
    <label>
      {labelName} {required && fieldRequired}
    </label>
  )
}

Label.propTypes = {
  labelName: PropTypes.string.isRequired,
  // LabelName is a string and is required
  required: PropTypes.bool
}

export default Label
```

## Prop Types with Defaults
```js
ProgressBar.propTypes = {
  percent: propTypes.number.isRequired,
  width: propTypes.number,
  height: propTypes.number,
  type: propTypes.oneOf(['text', 'number', 'password']),
  value: propTypes.any,
  required: propTypes.bool,
  children: propTypes.node,
}

ProgressBar.defaultProps = {
  height: 8,
  width: 150
}
```