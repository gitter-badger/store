# @ngrx/store
[ ![Codeship Status for ngrx/store](https://img.shields.io/codeship/0c4f5b50-8372-0133-b304-425351b234ba/master.svg)](https://codeship.com/projects/121789)
[![npm version](https://badge.fury.io/js/%40ngrx%2Fstore.svg)](https://badge.fury.io/js/%40ngrx%2Fstore)

RxJS powered Redux for Angular2 apps

### installation
- Make sure you have angular2 installed via npm : `npm install angular`
- Install from npm : `npm install @ngrx/store`

### usage

- Create a reducer function for each data type you have:

```typescript
//counter.ts
import {Reducer, Action} from '@ngrx/store';

export const INCREMENT = 'INCREMENT';
export const DECREMENT = 'DECREMENT';
export const RESET = 'RESET';

export const counter:Reducer<number> = (state:number = 0, action:Action) => {

	switch (action.type) {
		case INCREMENT:
			return state + 1;

		case DECREMENT:
			return state - 1;

		case RESET:
			return 0;

		default:
			return state;
	}
}
```

- In your app's main module, import those reducers and use the `provideStore(reducers, initialState)` function to provide them to Angular's injector:

```typescript

import {bootstrap} from 'angular2/platform/bootstrap';
import {provideStore} from '@ngrx/store';
import {App} from './myapp';

import {counter} from './counter';

bootstrap(App, [ provideStore({counter}, {counter: 0}) ];

```

- You can then inject the `Store` service into your Components and Services:

```typescript
import {Store} from '@ngrx/store';
import {INCREMENT, DECREMENT, RESET} from './counter';

@Component({
	selector: 'my-app',
	template: `
	  <button (click)="increment()">Increment</button>
		<div>Current Count: {{ counter | async }}</div>
		<button (click)="decrement()">Decrement</button>
	`
})
class MyApp {
	counter: Observable<number>;
	constructor(public store: Store){
		this.counter = store.select('counter');
	}
	increment(){
		this.store.dispatch({ type: INCREMENT });
	}
	decrement(){
		this.store.dispatch({ type: DECREMENT });
	}
	reset(){
		this.store.dispatch({ type: RESET });
	}
}

```





