## RxJS

### SwitchMap
- Implicitly switches the request whenever a new one comes in!

```javascript
export const loadReposEpic = action$ =>
  action$.ofType(LOADING).delay(1000).switchMap(action =>
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