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
```

```javascript
42 -> g -> f; // 85
```

### Eg.

```javascript
const gf = x => f(g(x));
gf(42); // 85
```

```javascript
const gf = x => x -> g -> f;
42 -> gf; // 85
```

**With Shorthand**
```javascript
const gf = g -> f;
42 -> gf; // 85
```

### Eg.

```javascript
(y => f((x => x * 2)(y)))(42); // 85
```

```javascript
42 -> y => f((x => x * 2)(y)); // 85
```

```javascript
42 -> y => f(y -> x => x * 2); // 85
```

```javascript
42 -> y => y -> x => x * 2 -> f; // 85
```

**With Shorthand**
```javascript
42 -> x => x * 2 -> f; // 85
```
