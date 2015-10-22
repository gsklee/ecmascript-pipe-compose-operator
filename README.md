ECMAScript Pipe/Compose Operator
================================

```javascript
const f = x => x + 1;
const g = x => x * 2;
const h = x => x % 3;
```

### Eg.

```javascript
f(42); // 43
```

```javascript
42 -> f; // 43
```

### Eg.

```javascript
(x => x + 1)(42); // 43
```

```javascript
42 -> x => x + 1; // 43
```

### Eg.

```javascript
f(g(42)); // 85
```

```javascript
f(42 -> g); // 85
/* Same As */
42 -> g -> f; // 85
```

### Eg.

```javascript
const _g_f = x => f(g(x));

_g_f(42); // 85
```

```javascript
const _g_f = x => x -> g -> f;
/* Same As */
const _g_f = g -> f;

42 -> _g_f; // 85
```

### Eg.

```javascript
(y => f((x => x * 2)(y)))(42); // 85
```

```javascript
42 -> y => f((x => x * 2)(y)); // 85
/* Same As */
42 -> y => f(y -> x => x * 2); // 85
/* Same As */
42 -> y => y -> x => x * 2 -> f; // 85
/* Same As */
42 -> x => x * 2 -> f; // 85
```

### Eg.

```javascript
const _42_g = g(42);

f(_42_g); // 85
```

```javascript
const _42_g = 42 -> g;

_42_g -> f; // 85
```

--

```javascript
const a = x => y => x(y) + 1;
const b = x => y => x(y) * 2;
const c = x => y => x(y) % 3;
const d = x => x - 4;
```

### Eg.

```javascript
a(b(c(d)))(42); // 5
```

```javascript
42 -> a(b(c(d))); // 5
/* Same As */
42 -> d -> c -> b -> a; // 5
```

### Eg.

```javascript
(a => (b => (c => (d => d - 4)(c) % 3)(b) * 2)(a) + 1)(42); // 5
```

```javascript
42 -> a => (b => (c => (d => d - 4)(c) % 3)(b) * 2)(a) + 1; // 5
/* Same As */
42 -> a => a -> (b => (c => (d => d - 4)(c) % 3)(b) * 2) + 1; // 5
/* Same As */
42 -> b => ((c => (d => d - 4)(c) % 3)(b) * 2) + 1; // 5
/* ...Same As */
42 -> d => (((d - 4) % 3) * 2) + 1; // 5
```

--

```javascript
const X = x => y => z => z(y(x));
const Y = x => x * 9;
const Z = x => x % 4;
```

### Eg.

```javascript
X(42)(Y)(Z); // 2
```

```javascript
(42 -> X)(Y)(Z); // 2
/* Same As */
(Y -> (42 -> X))(Z); // 2
/* Same As */
(Z -> (Y -> (42 -> X))); // 2
```

### Eg.

```javascript
(x => y => z => z(y(x)))(42)(x => x * 9)(x => x % 4); // 2
/* Same As */
(y => z => z(y(42)))(x => x * 9)(x => x % 4); // 2
/* Same As */
(z => z((x => x * 9)(42)))(x => x % 4); // 2
/* Same As */
(x => x % 4)((x => x * 9)(42)); // 2
```

```javascript
(x => y => z => x -> y -> z)(42)(x => x * 9)(x => x % 4); // 2
/* Same As */
(x => x % 4) -> ((x => x * 9) -> (42 -> (x => y => z => x -> y -> z))); // 2
/* Same As */
(x => x % 4) -> ((x => x * 9) -> (y => z => 42 -> y -> z)); // 2
/* Same As */
(x => x % 4) -> (z => 42 -> x => x * 9 -> z); // 2
/* Same As */
42 -> x => x * 9 -> x => x % 4; // 2
```

--

```javascript
function compose(...fs) {
  return x => fs.reduceRight((_x, f) => f(_x), x);
}
```

### Eg.

```javascript
compose(
  applyMiddleware(
    ReduxPromise,
    createLogger()
  ),

  persistState(
    filterPersistedState('user.data')(adapter(window.sessionStorage))
  )
)(
  createStore
)(
  mergePersistedState()(rootReducer),
  initialState
);
```

**Without `compose()`**
```javascript
applyMiddleware(
  ReduxPromise,
  createLogger()
)(
  persistState(
    filterPersistedState('user.data')(adapter(window.sessionStorage))
  )(createStore)
)(
  mergePersistedState()(rootReducer),
  initialState
);
```

```javascript
(
  (ReduxPromise, createLogger()) -> applyMiddleware
)(
  persistState(
    filterPersistedState('user.data')(adapter(window.sessionStorage))
  )(createStore)
)(
  mergePersistedState()(rootReducer),
  initialState
);
/* Same As */
(
  (ReduxPromise, createLogger()) -> applyMiddleware
)(
  persistState(
    ('user.data' -> filterPersistedState)(adapter(window.sessionStorage))
  )(createStore)
)(
  mergePersistedState()(rootReducer),
  initialState
);
/* Same As */
(
  (ReduxPromise, createLogger()) -> applyMiddleware
)(
  persistState(
    window.sessionStorage -> adapter -> ('user.data' -> filterPersistedState)
  )(createStore)
)(
  mergePersistedState()(rootReducer),
  initialState
);
/* Same As */
(
  (ReduxPromise, createLogger()) -> applyMiddleware
)(
  (window.sessionStorage -> adapter -> ('user.data' -> filterPersistedState) -> persistState)(createStore)
)(
  mergePersistedState()(rootReducer),
  initialState
);
/* Same As */
(
  (ReduxPromise, createLogger()) -> applyMiddleware
)(
  createStore -> (window.sessionStorage -> adapter -> ('user.data' -> filterPersistedState) -> persistState)
)(
  mergePersistedState()(rootReducer),
  initialState
);
/* Same As */
(
  createStore ->
  (window.sessionStorage -> adapter -> ('user.data' -> filterPersistedState) -> persistState) ->
  ((ReduxPromise, createLogger()) -> applyMiddleware)
)(
  mergePersistedState()(rootReducer),
  initialState
);
/* Same As */
(
  createStore ->
  (window.sessionStorage -> adapter -> ('user.data' -> filterPersistedState) -> persistState) ->
  ((ReduxPromise, createLogger()) -> applyMiddleware)
)(
  rootReducer -> mergePersistedState(),
  initialState
);
/* Same As */
(
  rootReducer -> mergePersistedState(),
  initialState
) -> (
  createStore ->
  (window.sessionStorage -> adapter -> ('user.data' -> filterPersistedState) -> persistState) ->
  ((ReduxPromise, createLogger()) -> applyMiddleware)
);
```
