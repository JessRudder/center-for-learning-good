React is:
	- compositional
	- declarative
 		- tell it what to do or make, not how to do it
	- data flows through components
	- it's just javascript


Composition
	- use intuition about functions to create components
	- funcs take in params and return values
	- components take in data and return UI
	- composition takes single components and combines them together

// Some confusing resources they said to read
// https://www.linkedin.com/pulse/compose-me-function-composition-javascript-kevin-greene

// https://hackernoon.com/javascript-functional-composition-for-every-day-use-22421ef65a10

```
// Example of composing functions to get some value
function getProfilePic(username) {
    return 'https://github.com/' + username + '.png?size=200';
}
function getProfileLink(username) {
    return 'https://github.com/' + username;
}
// invoking getProfileData returns an obj representing our user
function getProfileData (username) {
    return {
        pic: getProfilePic(username),
        link: getProfileLink(username)
    };
}

// Now imagine it returning UI instead
// Here it's "composition" to include pic and link components inside the profile component 
function ProfilePic(username) {
    return ( <img alt={username}
        src={'https://github.com/'+username+'.png?size=200'} /> )
}
function ProfileLink(username) {
    return <a href={'https://github.com/' + username}>{username}</a>
}
function Profile(username) {
    return (
        <div classname='profile'>
            <ProfilePic username='username' />
            <ProfileLink username='username' />
        </div>
    )
}
```

Imperative vs Declarative Code
	- Declarative means you tell it the outcome you want and it figures out how to get there
	- For React, this means you tell it what the state of the DOM should be and it decides whether or not to update

Unidirectional Data Flow
	- No two-way data bindings (Angular/Ember)
	- Parent is the only one that's allowed to change the data
	- Child can say "Data! This might be nice to change....Hey, Parent....Can you change it?"

React is Just JS!
	- API is small, not recreating functionality already present in JS
	- React depends on functional programming paradigm that's made big impact in JS community
	- .map and .filter are vital techniques
	- remember that .map returns a new array, not a modified original array!

REFACTOR CHALLENGE
	- Are there for loops that can be changed to a .map?
	- Are there if statements that can be changed to a .filter?

VIRTUAL DOM
	// http://facebook.github.io/react/docs/optimizing-performance.html#avoid-reconciliation
	- checks vDOMEq (are virtual DOMs equivalent?) and SCU (should compoents update?)
	- if should components update returns false it won't check any nodes beyond that
		- will only check the children nodes if parent nodes should update and parent node shows DOMs have changed

DIFFING ALGORITHM
	// http://facebook.github.io/react/docs/reconciliation.html#the-diffing-algorithm
	- same type changes are easy (e.g. same node has different class names, change the class names)
	- inserting additional li is naively easy, involving one change
	- inserting a new first li is naively not, involving changing every li after that
		- supports keys so that your list items do not rerender every item
