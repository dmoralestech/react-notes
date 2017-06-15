#from egghead redux

```code
const todo = (state, action) => {
  switch (action.type) {
    case 'ADD_TODO':
      return {
        id: action.id,
        text: action.text,
        completed: false
      };
    case 'TOGGLE_TODO':
      if (state.id !== action.id) {
        return state;
      }
      return {};
    default:
      return state;
  }
}

const todos = (state = [], action) => {
  switch (action.type) {
    case 'ADD_TODO':
      console.log('action', action)
      console.log('state', state)
      return [
        ...state,
        todo(undefined, action)
      ];
    case 'TOGGLE_TODO':
      return state.map(t => todo(t, action));
    default:
      return state;
  }
};

const visibilityFilter = (
    state = 'SHOW_ALL',
    action
) => {
    switch (action.type) {
      case 'SET_VISIBILITY_FILTER':
        return action.filter;
      default:
        return state;
    }
};

const combineReducers = reducers => {
  return (state = {}, action) => {

    // Reduce all the keys for reducers from `todos` and `visibilityFilter`
    return Object.keys(reducers).reduce(
      (nextState, key) => {
        // Call the corresponding reducer function for a given key
        console.log('nextState', nextState)
        console.log('key', key)
        nextState[key] = reducers[key] (
          state[key],
          action
        );
        return nextState;
      },
      {} // The `reduce` on our keys gradually fills this empty object until it is returned.
    );
  };
};

const todoApp2 = combineReducers({
  todos,
  visibilityFilter
});

const todoApp = (state = {}, action) => {
  return {
     // Call the `todos()` reducer from last section
     todos: todos(
      state.todos,
      action
    ),
    visibilityFilter: visibilityFilter(
      state.visibilityFilter,
      action
    )
  };
};

const action = {
    type: 'ADD_TODO',
    id: 0,
    text: 'Learn Redux'
};

const res = todoApp2([{
    id: 0,
    text: 'Learn Redux',
    completed: false
}], action);

console.log('res', res)
```code
