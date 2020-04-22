## 手写一切（2）

### 手写redux
```js
function createStore(reducer, enhancer) {
    if (enhancer) {
        return enhancer(createStore)(reducer);
    }
    
    let state = {};
    let listeners = [];
    
    function getState() {
        return state;
    }
    
    function subscribe(listener) {
        listeners.push(listener);
    }
    
    function dispatch(action) {
        state = reducer(action, state);
        listeners.forEach(listener => listener());
        return state;
    }
    
    dispatch({type: '@myRedux'});
    return {
        getState,
        subscribe,
        dispatch
    }
}

// applyMiddleware
function applyMiddleware(...middlewares) {
    return createStore => (...args) => {
        const store = createStore(...args);
        
        const dispatch = middlewares.reduce((disp, middleware) => {
            return middleware({
                getState: store.getState,
                dispatch: (...args) => disp(...args)
            })(disp);
        }, store.dispatch);
        
        return {
            ...store,
            dispatch
        }
    }
}

// redux-thunk
function thunk({dispatch, getState}) {
    return next => action => {
        if (typeof action === 'function') {
            return action(dispatch, getState);
        }
        return next(action);
    }
}

// demo
function asyncAction(dispatch, getState) {
    setTimeout(() => {
      dispatch({type: 'ADD'});
    }, 2000);
}

function reducer(action, state = {}) {
    if (action.type === 'ADD') {
        return {
            ...state,
            count: (state.count || 0) + 1
        }
    }
}

store = createStore(reducer, applyMiddleware(
    thunk
))
```