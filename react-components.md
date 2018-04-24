# React Intro

## What is React
* Moving the View layer to React. We will still use the Model and Controller on the back-end
* It is component based. We used to have separation of concerns by html, css, javascript. Now we have separation of concerns by components
* Built by Facebook. Open sourced. First used in 2011. Instagram adopted in 2012. 
* Switched to MIT license in 2017

### Components
![fixed templates](https://git.generalassemb.ly/ga-wdi-lessons/react-intro/raw/master/images/templates-page.png)
![page builder and components](https://git.generalassemb.ly/ga-wdi-lessons/react-intro/raw/master/images/components-page.png)

A component is a modular unit that you can invoke any number of times throughout the code. There is a parent-child relationship. So they are somewhat independent but not totally.
![component schematic](https://git.generalassemb.ly/ga-wdi-lessons/react-intro/raw/master/images/wireframe_deconstructed.png)

### React Components
A React component expect an input and will render a view. A well-structured component only receives necessary data. It follows a __functional__ approach to programming.  

#### Focused
Components should do one thing and do it well  

#### Independent
Components should increase cohesion and reduce coupling. Behavior in one component should not impact another. They should not rely on each other.

#### Reusable
Should be written in a DRY way

#### Small
Components should be short and condensed

#### Testable
Each component is independently testable

## Starting a React App
```bash
cd wdi/sandbox
create-react-app blog-app
cd blog-app
code .
npm start
```
Running npm start will compile the app and load the page. It is using webpack. It uses babel to translate the html
1. Open App.js. This is like the index page. 
  * ```import``` and ```export``` are features of webpack.  
In the app.js: 
```js
import React, {Component} from 'react'
// import is from Webpack
// We're importing React and the Component class. It's really React.Component. But we're just doing it in one line

class Hello extends Component {
  // This inheritance syntax. So Hello is a class of the Component class. It's a child class
  render () {
    // This is a method that will tell us what the component has
    return (
      // we return the actual UI that we want to show up on the page
      <h1>Hello World</h1>
    )
  }
}

export default Hello
// Now we are exporting the class and making it available in other files
// Default makes it so it exports as the default version of Hello
```

Meanwhile, the index will import it
```js
import React from 'react'
import ReactDOM from 'react-dom'
import './index.css'
import App from './App'
import registerServiceWorker from './registerServiceWorker'

ReactDOM.render(<App />, document.getElementById('root'))
registerServiceWorker()
```

### Virtual DOM
You can no longer use the traditional javascript tricks to access the DOM. You can't do it.  
__App__ --> __Virtual DOM__ --> __DOM__  
We will be writing code in the App. Then we will bind that to the Virtual DOM in the index.js file. 
```js
ReactDOM.render(<App />, document.getElementById('root'))
```
That binds React to the id: root in the public/index.html file
```html
<div id="root"></div>
```

### Looping
```js
class Post extends Component {
  render() {
    return (
      <div>
        <h1>{this.props.currentPost.title}</h1>
        <h3>{this.props.currentPost.author}</h3>
        <p>{this.props.currentPost.body}</p>
        <h2>Comments</h2>
        { this.props.currentPost.comments.map(comment => <p>{comment}</p>) }
        // We are looping through using map and then creating a comment. But this could be abstracted with a new component
      </div>
    )
  }
}
```

### Components with Components
Create a Comments Component
```js
import React, {Component} from 'react'

class Comment extends Component {
  render () {
    return (
      <p>{this.props.comment}</p>
    )
  }
}

export default Comment
```

Now invoke it in the Post component
```js
import React, {Component} from 'react'
import Comment from './Comment'

class Post extends Component {
  render() {
    return (
      <div>
        <h1>{this.props.currentPost.title}</h1>
        <h3>{this.props.currentPost.author}</h3>
        <p>{this.props.currentPost.body}</p>
        <h2>Comments</h2>
        { this.props.currentPost.comments.map(comment => {
          return <Comment comment={comment} />
          // self closing tags require the slash
        }
        )}
      </div>
    )
  }
}

export default Post
```

### Virtual DOM
React is constantly comparing the real DOM and virtual DOM and making changes where needed. It only updates changes. THat's cool. 

## Props and State
__Props__: Inherited properties from parent that cannot be changed by the component itself  
__State__: A property that can be changed by the component
* When State changes, the Virtual DOM changes and that is passed into the actual DOM
* When State changes, it _automatically re-renders the component_
![Render schematic](https://git.generalassemb.ly/ga-wdi-lessons/react-state-and-props/raw/master/images/react-component-state-update.png)

To use state, use the constructor:
```js
class Counter extends Component {
  constructor() {
    // super brings in the stuff from the component class
    super()

    // this.state is an object with variables we want to be modifying
    this.state = {
      count: 0
    }

    // keeps 'this' referring to the component instead of the event
    // must do this for all functions called by an event:
    this.increaseCount = this.increaseCount.bind(this)
    this.decreaseCount = this.decreaseCount.bind(this)
  }

  increaseCount (event) {
    let count = this.state.count + 1
    // make a change
    this.setState({
      count: count
    })
    // attach it to the state using an object
  }

  decreaseCount () {
    let count = this.state.count - 1
    this.setState({
      count: count
    })
  }
// render follows this
```

## Passing Props to State
We can take something that comes in as a prop from above and turn it into a state:  
In the index file: 
```js
const data = {
  counters: 5
}

ReactDOM.render(
  <App counters={data.counters} />,
  document.getElementById("root")
)
```

In the app.js file (child):
```js
export default class App extends Component {
  constructor (props) {
    // we always have access to the props
    super(props)
    // need to call super to bring in all the stuff from the Component class
    // need to pass 'props' in to super because that's where the props are
    this.state = {
      counters: props.counters
    }
  }

  render() {
    return (
      <div className="App">
        <Header />
        <CounterList counters={this.state.counters}/>
        <h4>{this.props.counters}</h4>
      </div>
    )
  }
}
```

### Invoking higher level functions
We can invoke a function written in a parent element by a child element. This allows events in a  child element to affect the data in a parent element. To do so, first define the function in the parent. Then make sure to bind the this. And lastly, pass the function as a variable into the child's props:
```js
export default class App extends Component {
  constructor (props) {
    // we always have access to the props
    super(props)
    // need to call super to bring in all the stuff from the Component class
    // need to pass 'props' in to super because that's where the props are
    
    this.state = {
      counters: props.counters
      // this sets a state.counters value equal to the props.counters. We can then change it in the future. 
    }

    // have to bind the this or else it will be invoked down in Headers in the wrong way
    this.increaseCounters = this.increaseCounters.bind(this)
    this.decreaseCounters = this.decreaseCounters.bind(this)
  }

  increaseCounters() {
    let counters = this.state.counters + 1
    this.setState({counters: counters})
  }

  decreaseCounters() {
    let counters = this.state.counters - 1
    this.setState({counters: counters})
  }

  render() {
    return (
      <div className="App">
        <Header 
          increaseCounters={this.increaseCounters}
          counters={this.state.counters}
          decreaseCounters={this.decreaseCounters}
        />
        <CounterList
          counters={this.state.counters}
        />
        <h4>{this.props.counters}</h4>
      </div>
    )
  }
}
```

Then, in the child js file:
```js
class Header extends Component {
  render() {
    return (
      <header className="App-header">
        <h1 className="App-title">React Counters: {this.props.counters}</h1>
        <button onClick={this.props.increaseCounters}>+</button>
        <button onClick={this.props.decreaseCounters}>-</button>
      </header>
    );
  }
}
```


### Presentational vs ____


### Counter Cmponent breakdown
* App
  * Header
  * Counter List
    * Counter

## Tips
1. Create an outline of your components. This will be a visual guide. 
  * It would be cool to design a table of all the things you need to pass down and up etc
2. Import all the things that are children to the element. Pass data down to the childen in the <Name info={info}> area
