# hof

Higher-order function combinators for [Quadrate](https://git.sr.ht/~klahr/quadrate).

## Installation

```bash
quadpm install https://github.com/quadrate-language/hof
```

## Usage

```quadrate
use hof

fn main() {
    // Apply a function to a value
    5 fn (x:i64 -- r:i64) { 2 * } hof::apply print nl  // 10

    // Apply two functions to the same value
    5 fn (x:i64 -- r:i64) { 2 * } fn (x:i64 -- r:i64) { 3 + } hof::bi
    print nl print nl  // 10 8

    // Apply but keep original value
    5 fn (x:i64 -- r:i64) { 2 * } hof::keep
    print nl print nl  // 10 5
}
```

## Functions

### Basic Combinators

| Function | Signature | Description |
|----------|-----------|-------------|
| `apply` | `( x f -- r )` | Apply function f to x |
| `bi` | `( x f g -- a b )` | Apply f and g to same x |
| `tri` | `( x f g h -- a b c )` | Apply f, g, h to same x |
| `keep` | `( x f -- r x )` | Apply f, keep original x |
| `dip` | `( x y f -- r y )` | Apply f to x, preserve y |
| `both` | `( x y f -- a b )` | Apply f to both x and y |
| `bi_star` | `( x y f g -- a b )` | Apply f to x, g to y |

### Conditional Combinators

| Function | Signature | Description |
|----------|-----------|-------------|
| `when` | `( x cond f -- r )` | Apply f if cond is true |
| `unless` | `( x cond f -- r )` | Apply f if cond is false |
| `times` | `( x n f -- r )` | Apply f n times |

### Function Composition

| Function | Signature | Description |
|----------|-----------|-------------|
| `compose` | `( f g -- fg )` | Compose two functions: fg(x) = g(f(x)) |
| `curry` | `( x f -- curried )` | Fix first argument of binary function |
| `curry_right` | `( y f -- curried )` | Fix second argument of binary function |

### Array Operations

| Function | Signature | Description |
|----------|-----------|-------------|
| `fold` | `( arr count init f -- result )` | Left fold/reduce |
| `fold_right` | `( arr count init f -- result )` | Right fold/reduce |
| `map` | `( arr count f -- result count )` | Map function over array |
| `filter` | `( arr count pred -- result count )` | Keep elements matching predicate |
| `any` | `( arr count pred -- bool )` | Check if any element matches |
| `all` | `( arr count pred -- bool )` | Check if all elements match |
| `find` | `( arr count pred -- elem found )` | Find first matching element |

## Examples

### Compose functions

```quadrate
use hof

fn main() {
    // Create a composed function: double then add 1
    fn (x:i64 -- r:i64) { 2 * }
    fn (x:i64 -- r:i64) { 1 + }
    hof::compose -> f

    5 f call print nl  // 11 (5*2 + 1)
}
```

### Curry a binary function

```quadrate
use hof

fn main() {
    // Create add5 from generic add
    5 fn (x:i64 y:i64 -- r:i64) { + } hof::curry -> add5

    10 add5 call print nl  // 15
    20 add5 call print nl  // 25
}
```

### Fold an array

```quadrate
use hof

fn main() {
    // Sum array elements
    5 make<i64> -> arr
    arr 1 append -> arr
    arr 2 append -> arr
    arr 3 append -> arr
    arr 4 append -> arr
    arr 5 append -> arr

    arr 5 0 fn (acc:i64 x:i64 -- r:i64) { + } hof::fold
    print nl  // 15
}
```

## License

Apache-2.0 - See [LICENSE](LICENSE) for details.

## Contributing

Contributions welcome! Please open an issue or submit a pull request on [GitHub](https://github.com/quadrate-language/hof).
