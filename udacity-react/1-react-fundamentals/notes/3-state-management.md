Props
	- use to pass data into components
Functional Components
	- more intuitive way to create components
	- create component as a function
Control Components
	- mostly for forms
	- get forms to look at components state (hook it up)


## Passing Data with Props ##
	- props are to components as arguments are to functions
	- pass component a value (attribute), multiple can be passed individually
		- <Clock time={Date.now()} zone='PDT' />
	- access any attributes inside a component using this.props.propName (e.g. this.props.time)
	- props are READ ONLY (don't go trying to change them)

// Build a component
import React, { Component } from 'react'

class ListContacts extends Component {
	render() {
		return (
			<ul className='contact-list'>
				{this.props.contacts.map((contact, idx) => (
					<li key={idx} className="contact-list-item">
						<div className="contact-avatar" style={{backgroundImage: `url(${contact.avatarURL})`}} />
						<div className="contact-details">
							<p>{contact.name}</p>
							<p>{contact.email}</p>
						</div>

						<button className="contact-remove" />
					</li>
				))}
			</ul>
		)
	}
}

export default ListContacts

## Functional Components ##
Technically called a stateless, functional component
	- if all your component has is a render method, you can use a functional component
	- instead of class, just defined as function
		- first arg passed is props, access it the way you would an argument in a function
	- advantages 
		** don't need entire class for a component **
		** no 'this' keyword to deal with **
	- don't use it if you 
		- need to initialize some data 
		- don't have props for the component
		- have a component that does not use JSX

// example of stateless functional component
function User(props) {
	return (<p>username: {props.username}</p>)
}

// Build functional component (based on what we had above)
import React, { Component } from 'react'

function ListContacts(props) {
	return (
		<ul className='contact-list'>
			{props.contacts.map((contact, idx) => (
				<li key={idx} className="contact-list-item">
					<div className="contact-avatar" style={{backgroundImage: `url(${contact.avatarURL})`}} />
					<div className="contact-details">
						<p>{contact.name}</p>
						<p>{contact.email}</p>
					</div>

					<button className="contact-remove" />
				</li>
			))}
		</ul>
	)
} 

export default ListContacts

## Add State to Components - setting state ##
State Management
	- separatse out how component looks vs what the state is
	- props are read only, immutable data
	- mutable data is managed by the component and often represents changes from the user (e.g. button click)
		- need state to do that
		- access with this.state

Major Benefits
	- Don't think about parts of page that change on update
	- Don't need to decide how to re-render the page efficiently
	- Reconciliation is automatic in React
	- Only need to think about updating state

// change to the lang - put the state object directly inside class as CLASS FIELD (for ES7)
class User extends React.Component {
	state = {
		username: 'Xyzaz'
	}
}

// as opposed to doing this in the constructor per FB's Setting theInitial State Docs
class User extends React.Component {
	constructor(props) {
		super(props);
		this.state = {
			username: 'Xyzaz';
		}
	}	
}

// Rule of thumb: if you use a piece of state to render some UI, state should live in that component
	// we'll see this in action when we delete contacts

NONONONONO!!!! /!\ PROPS IN INITIAL STATE ANTIPATTERN /!\

	this.state = {
		user: props.user
	}

 - this deviates from dependable sources of truth
 - props and states are two different, independent data sources
 - DON'T CORRUPT THEM!
 - you need a single source of truth

## Update State with setState ##
setState
	- method inside component
	- first arg is previous state (current state)
	- returns object that will be merged with the current state
	- by default setState does rerender

In react, your UI is just a function of your state

//Pattern 1: you pass setState a function, do this when new state depends on previous state
this.setState((prevState) => {
	count: prevState.count + 1;
})

//Pattern 2: you pass setState an object, when New state is independent of previous state
this.setState({
	username: "Xaxix";
})

** if you use a function, you're going to be making use of previous data, if you use an object, you're just telling the state what to be


Extra thoughts:
	- you can set state of objects independently
		- imagine an Email component with state={subject: '', message: '' }
		- below we just update the subject but leave the message blank

this.setState({
	subject: 'Hi!'
})

Potential Gotchas!
	- .state does not trigger component to auto rerender
	- .setState() does trigger render(), since component also calls render() when setState() merges
	- .setState() should NOT be called wihtin component's render9) method
	- we expect the state to change usually due to user input

PropTypes
	- a package that does type-checking on props

ListContacts.propTypes = {
	contacts: PropTypes.array.isRequired,
	onDeleteContact: PropTypes.func.isRequired
}

## Controlled Components - need it for a form ##
	- typically form state lives inside the DOM
	- but React is all about state management
	- so how? with Controlled Components!
		- called Controlled bc "React is controlling the state of the form"

// Component to render a form with a single input elemeent
// - this is a 'true controlled component" bc React controls email property of state

class NameForm extends React.Component {
		state = {
			email: ''
		}
		render() {
			return (
				<form>
					<input type="text" value={this.state.email} />
				</form>
			)
		}
}

// allow input field to change using a handleChange method
class NameForm extends React.Component {
		state = {
			email: ''
		}

		handleChange = (event) => {
			this.setState({email: event.target.value}) //whenever input changes
		}

		render() {
			return (
				<form>
					<input type="text" value={this.state.email} onChange-{this.handleChange} />
				</form>
			)
		}
}

Benefits
	- support instant input validation
	- conditionally disable/enable buttons
	- enforce input formats
	- update your UI based on the form itself

Uncontrolled Pattern for doing this ("typical", not managing state)

Rule of Thumb: use controlled whenever input changes more than just the field itself (e.g. filtering based on what's being entered)

React Developer Tools
	- inspect your coponent hierarchy along with respective props and states

