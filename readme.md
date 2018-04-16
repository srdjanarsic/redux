# Redux

Predictable state management enables as to establish one source of truth and to decouple component form each other.

* [Actions](#actions)
* [Action type](#action-type)
* [Action creators](#action-creators)
* [Bound action creator](#bound-action-creator)
* [Reducer](#reducer)
* [Combine reducers](#combine-reducers)
* [Store](#store)

## Actions

Action is intent. It's fully serializable.

```json
{
    type: Action.ADD_CLIENT,
    payload:{
        title: PersonTitle.MR,
        name: "Srdjan",
        surname: "Arsic"
    }
}
```

## Action type

Action type is unique action identifier.

``` 
const Action = {
    ADD_CLIENT: "ADD_CLIENT",
    EDIT_CLIENT: "EDIT_CLIENT"
}
```

## Action creators

Function that simplifies action creation

```json
function addClient(title, name, surname){
    return {
        type: "ADD_CLIENT",
        payload: {
            title: title,
            name: name,
            surname: surname
        }
    }
}
```

## Bound action creator

```js
const boundAddClient = (title, name, surname) => dispatch(addClient(title, name, surname));
```

## Reducer

Reducer is pure function. It takes previous state and action and returns new state.

Note: As it's a pure function, it always returns same output for the same input.

Simplified reducer

```js
(state, action) => state + action.payload
```

**Note**: Reducer handle state transitions but they **MUST** be done synchronously. For async use middleware.

```js
function(state, action){
    switch (action.type) {
        case Action.ADD_CLIENT:
            state.client = Object.create({}, state.client, action.payload);
            return state;
        default:
            return state;
    }
}
```

## Combine reducers

1. Separate independent store state into sub-objects.

```js
//state
{
    client: { ... },
    settings: { ... }
}
```

2. Create reducer for individual parts.

```js
function client (state=initialState, action){
    switch (action.type) ....
}
function settings (state=initialState, action){
    switch (action.type) ....
}
```

3. Combine reducers

```js
export appReducer  = combineReducers({
    client,
    settings
});
```

## Store

* Holds application state;
* Allows access to state via `getState()`;
* Allows state to be updated via `dispatch(action)`;
* Registers listeners via `subscribe(listener)`;
* Handles unregistering of listeners via the function returned by `subscribe(listener)`.

1. Create store

```js
let store = createStore(gameReducer);
```

2. Subscribe to store

```js
store.subscribe(() =>
  console.log(store.getState())
);
```

3. Dispatch action

```js
store.dispatch({
    type: Action.ADD_CLIENT
    payload: ....
});
```