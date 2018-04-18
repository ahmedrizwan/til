# Redux Observable

### Create a component

1.  Map state to props
2.  Map dispatch to props
3.  Use `connect` to connect the component with these mappings

```javascript
import { connect } from "react-redux";

class MyComponent extends Component {
  ...
}

const mapStateToProps = ({ isLoading, payload }) => ({
  isLoading,
  payload
});

const mapDispatchToProps = { loadRepos };

export default connect(mapStateToProps, mapDispatchToProps)(MyComponent);
```

### Epics are our middleware in redux-observable, so create an `epic`

```javascript
export const loadReposEpic = action$ =>
  action$.ofType(LOADING).switchMap(action =>
    ajax("https://api.github.com/users/ahmedrizwan/repos")
      .map(payload => ({
        type: SUCCESS,
        payload: payload.response
      }))
      .catch(payload => ({
        type: ERROR
      }))
  );
```

`SwitchMap implicitly switches the request whenever a new one comes in!`

### Create Reducer

```javascript
import { IDLE, LOADING, SUCCESS, ERROR } from "./constants";

export const reposReducer = (state = { isLoading: false }, action) => {
  switch (action.type) {
    case IDLE:
      return { isLoading: false };

    case LOADING:
      return { isLoading: true };

    case SUCCESS:
      return { isLoading: false, payload: action.payload };

    case ERROR:
      return { isLoading: false };

    default:
      return state;
  }
};
```

### Store

```javascript
const epicMiddleware = createEpicMiddleware(loadReposEpic);

export const store = createStore(reposReducer, applyMiddleware(epicMiddleware));
```

This store will be used in App.js, passed into a Provider
```javascript
class App extends Component {
  render() {
    return (
      <Provider store={store}>
        <MyComponent />
      </Provider>
    );
  }
}
```
