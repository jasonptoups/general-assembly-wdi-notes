# React Component Lifecycle

## Why it matters
* If we want to show something while waiting for an API to send data


## Major Stages
#### 1. Mounting
* componentWillMount
* render
* componentDidMount
#### 2. Updating
* componentWillReceiveProps
* shouldComponentUpdate
* componentWillUpdate
* render
* componentDidUpdate
#### 3. Unmounting
* componentWillUnmount

![Alt](https://git.generalassemb.ly/ga-wdi-lessons/react-component-lifecycle/raw/master/images/reactjs_component_lifecycle_functions.png "lifecycle")

## Mounting/Unmounting stages
* Methods:
  * constructor()
  * componentWillMount()
  * render()
  * __componentDidMount()__
  * componentWillUnmount()

We place AJAX request in the componentDidMount() method. It then will set state and cause the render to happen again. If we set state in the componentDidMount(), here is what will happen:  
```constructor --> componentWillMount --> render --> componentDidMount --> render```  


## Updating Stages
* Methods:
  * componentWillReceiveProps()
  * shouldComponentUpdate()  
    * This needs to be a boolean. It determines whether state is allowed to change
  * componentWillUpdate()
  * render()
  * __componentDidUpdate()__

If you are making changes to appearance, it will probably look like:
```shouldComponentUpdate --> componentWillUpdate --> componentDidUpdate```

## Axios and API
```js
  componentDidMount () {
    console.log('Home: In componentDidMount')
    const url = 'https://api.tvmaze.com/search/shows?q=office'
    axios.get(url).then(res => this.setState({movies: res.data}))
  }
```