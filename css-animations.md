# CSS Animations
## Example:
```css
transition {
  target1 time ease/linear delay,
  target2 time ease/linear delay
} 

.element {
  animation: pulse 5s linear
}

@keyframes pulse {
  0% {
    background-color: #FFFFFF
  }
  100% {
    background-color: #000000
  }
}

@keyframes other {
  from {
    /* starting */
  }
  to {
    /* ending */
  }
}
```

## Using animations
* Try to use transform and opacity. The browser can offload these to the GPU and run the calculations in a separate thread. It's much more performant. 
  * I need to do this
  ```css
  transform: translate(x, y)
  transform: scale(n)
  transform: rotate(ndeg)
  opacity: 0 to 1
  ```
* Don't animate margin, top/right/bottom/left, or padding directly. It can mess with the layout