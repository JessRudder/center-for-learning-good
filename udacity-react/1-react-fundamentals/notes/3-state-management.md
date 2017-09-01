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
