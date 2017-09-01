Introduction
	- React elements are light JS objects. They are NOT elements of the DOM!
	- because of this, we use JS to put elements together
	- we describe what the page should look like then React manipulates DOM
	- components are custom elements often combining elements together
		- "the building blocks of React"

Elements and JSX
	- React isn't built into browsers so no console testing
	- Need React environment (we'll get there later)
	- .createElement() and render()
		- React.createElement(type, properties, content)
		- attribs FOR and CLASS are accessed by label.htmlFor and div.className
	- React is only concerned with the view layer of the app

// To be clear, both of these generate the SAME output!
elem = React.create('ol', null, people.map((p, i) => (
        React.createElement('li', { key: i }, p.name)
    ))
)
elem = <ol>
    {people.map(p,i) => (
        <li key={i}>{p.name}</li>
    ))}
</ol>


// the above code is still compiling down to JS: _react2.default.createElement(elem,...)

// JSX elements MUST return a single root elem, just like React.createElement does
const thisWillError = (<h1>Some title</h1><h2>My subtitle</h2>);

//group elements together into factories to generate our own custom elements

```
class ContactList extends React.Component{
	render() {
		const people = [
			{name: "Joshua"},
			{name: "Jessica"},
			{name: "Tsaggui"}
		]

		return <ol>
			{people.map((person,index) =>(
				<li key=(index)>{person.name}</li>
			))}
		</ol>
	}
}


ReactDOM.render(
	<ContactList/>,
	document.getElementByID('root')
)
```

Create a React App
	- JSX needs to be transpiled (usually with Babel)
		- run through build tool like Webpack
		- bundles assets together
	- easier to use FB's Create React App package to manage all the set up

Components
	- work on tiny pieces of app without impacting others
	- in practice this is bundling stuff into a class
		- then you can use this component like <ContactList/>
		- duplicate like <ContactList/><ContactList/>
		- edit properties (props) by accesing this.props..in a class
		- props allow indepentend configuration of a component
	- "Favor composition over inheritance"
		- only extend once
		- make sure each component is useful on its own

// use props to make contactlist component reusable and change who's in the list
class ContactList extends React.Component {
    render() { 
        const people = this.props.contacts             // store prop passed to ContactList

        return <ol>
            {people.map((person,index) => (
                <li key={index}>{person.name}</li>
            ))}
        </ol>
    }
}
class App extends React.Component {
    render() {
        return (
            <div className="App">
                <ContactList contacts = {[         // where I pass to the prop
                    {name: 'Ayesir'},
                    {name: 'Beesaur'},
                    {name: 'Ceesimus'}
                ]} />
            </div>
        );
    }
}

