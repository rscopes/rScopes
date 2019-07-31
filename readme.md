
<p align="center"><img  width="192" src ="https://github.com/rScopes/rescope/raw/master/doc/assets/logo.svg?sanitize=true" /></p>

<p align="center" style="font-size:25px"><b>

# rScopes

</b></p>
<p align="center">Flexible State management system based on flux architecture, stores data components & inheritable scopes
</p>

<p align="center"><a href="https://travis-ci.org/n8tz/rescope">
<img src="https://travis-ci.org/n8tz/rescope.svg?branch=master" alt="Build Status" /></a>
<a href="https://www.npmjs.com/package/rscopes">
<img src="https://img.shields.io/npm/v/rscopes.svg" /></a>
<img src="https://img.shields.io/badge/contributions-welcome-brightgreen.svg?style=flat" />

</p>

## rScopes What ?

RS is a flexible state management system inspired by ReactJS methods.<br/>
Quickly said, RS allow linking, scoping, and sequencing of multiples redux-like, async-able, stores while remaining in a serializable flux architecture.


RS is semantically and operationally stable; it does the work and have a coherent semantic system. <br/>
That said, it wasn't wrote with perf or perfect code as priority, but to manage states & async problematics in a intuitive & modular way. <br/>


## Minimal doc [here](DOC.MD)

## What's it like?

Here some conceptual basics :

```jsx harmony
import React                                                                          from "react";
import {asRef, asScope, asStore, withScope, withStateMap, propsToScope, scopeToProps} from "rscopes";
import {MyComplexStore}                                                               from "./from/somewhere";

// withScope will instantiate a dedicated scope when using this React Component
// it will inherit the scope & actions from the parents React Elements
@withScope(
	{
		@asStore
		CoffeeMachine: {
			coffee: 100,
			sugar : 100,
			
			// actions return state mutation & can be call with props.$actions.*
			makeCoffee: () => ( state ) => ({
				coffee: state.coffee - 1,
				sugar : state.sugar - 1,
			})
		},
		
		// withStateMap hoc any Store to add values & refs to theirs state
		@withStateMap({
			              goFetchTaskIn: "developement"
		              })
		ThingsTodo: MyComplexStore,
		
		// Scope can contain sub scopes
		@asScope
		BrainScope: {
			@asStore
			workMachine: {
				cafeine : 0,
				workerId: undefined,
				
				// refs can targets any store value in the scope
				// when starting with "!" the store will not apply until the targeted value is !== undefined
				@asRef
				subject: "!ThingsTodo.stuff",
				work() {
					this.$actions.$parent.makeCoffee()
					
					return ( state ) => ({ cafeine: state.cafeine + 1 });
				},
				// the $apply fn update data basing the new state
				$apply( data, { cafeine }, changesInState ) {
					return {
						canWork: cafeine >= 2
					}
				}
			},
		},
		
		@asStore
		Manager: {
			// Connecting Manager will au instantiate any referenced store 
			@asRef
			allOK          : "BrainScope.workMachine.canWork",
			@asRef
			shouldBuyCoffee: "CoffeeMachine.coffee",
			$apply( data, { allOK } ) {
				return {
					allOK
				}
			}
		},
		
	}
)
// this will connect any changes of props.workerId to BrainScope.workMachine.workerId
@propsToScope("workerId:BrainScope.workMachine.workerId")
// this will bind the result data of Manager to props.Manager
// * Store are recursively instantiated when referenced & are destroyed when listeners are removed
@scopeToProps("Manager")
class TestProps extends React.Component {
	//...
}

```

### About decorators

( well, like code-decorators )

These are stage-0 decorators because while the decorators spec has changed and is now stage-2, no transpiler has yet to implement these changes and until they do, this library won't either. Although the TypeScript documentation uses the phrase "Decorators are a stage 2 proposal for JavaScript" this is misleading because TypeScript still only implements the stage-0 version of the spec, which is very incompatible with stage-2 (as of this writing). If you concretely find that a compiler (babel, TS, etc) implement stage-2+, please do link me to the appropriate release notes! 🎈


This can be consumed by any transpiler that supports stage-0 of the decorators spec, like [babel.js](https://babeljs.io/) version 5. *Babel 6 [does not yet support decorators natively](https://phabricator.babeljs.io/T2645), but you can include [babel-plugin-transform-decorators-legacy](https://github.com/loganfsmyth/babel-plugin-transform-decorators-legacy) or use the [`applyDecorators()` helper](#applydecorators-helper).*

### Related packages

rScopes link the following packages :<br>

ReScope : [rescope](https://github.com/n8tz/rescope)<br>
React HOCs, decorators & tools : [react-rescope](https://github.com/n8tz/react-rescope)<br>
ReScope HOCs, decorators & tools : [rescope-spells](https://github.com/n8tz/rescope-spells)<br>

### Samples & bootstraps

The examples and bootstrap will come gradually [here](https://github.com/n8tz/rescope-samples)

<span class="badge-paypal"><a href="https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=VWKR3TWQ2U2AC" title="Donate to this project using Paypal"><img src="https://img.shields.io/badge/paypal-donate-yellow.svg" alt="PayPal donate button" /></a></span>
