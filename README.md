ECMAScript Pipe/Compose Operator
================================

```javascript
const f = x => x + 1;
const g = x => x * 2;
const h = x => x % 3;
```

### Eg.

```javascript
f(1); // 2
```

```javascript
1 -> f; // 2
```

### Eg.

```javascript
(x => x + 1)(1); // 2
```

```javascript
1 -> x => x + 1; // 2
```

### Eg.

```javascript
f(g(1)); // 3
```

```javascript
1 -> g -> f; // 3
```
