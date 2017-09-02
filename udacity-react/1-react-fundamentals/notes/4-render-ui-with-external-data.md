## Lifecycle Events

Do not do this in render
	- don't make any http requests
	- don't alter the DOM
	- don't fetch data

Hook into React Approved lifecycle events (4 most common) == Mounting is term for rendering
	- .componentWillMount
		- before it's rendered to DOM
	- .componentDidMount
		- after it's rendered to DOM
		* perfect time to do AJAX calls to get data
	- .componentWillUnmount
		- when it's about to be removed from the DOM
	- .componentWillReceiveProps
		- when it's about to get its properties

Fetching data from database using component did mount
	1) Render method is called
	2) at this point this.state variables have their initial values
	3) component is then mounted
	4) Immediately the componentDidMount lifecycle event happens
	5) Data is fetched and returned
	6) You update this.state.values
	7) Noticing the state change, the component gets re-rendered

Render is for this:
	- pure function that takes input via props
	- pure function that returns a description of your UI/JSX
	- a function over whose invocation you have no control (React gonna call it when React gonna call it)

The life of a component:
(1) Adding to the DOM
    - constructor()
    - componentWillMount()
    - render()
    - componentDidMount()

(2) Re-rendering
    - componentWillReceiveProps()
    - shouldComponentUpdate()
    - componentWillUpdate()
    - render()
    - componentDidUpdate()

(3) Removing from the DOM
    - componentWillUnmount()

[Handy Diagram of Relationships](https://d17h27t6h515a5.cloudfront.net/topher/2017/June/59519fa9_nd019-c1-l4-lifecycle-events/nd019-c1-l4-lifecycle-events.png)

