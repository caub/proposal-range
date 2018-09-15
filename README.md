# Proposal Array ranges literal

Possibly integrated with https://github.com/tc39/proposal-slice-notation/issues/19

`Array.from({length: n}, (value, i, arr) => {}?)` is not so convenient for creating ranges:
```js
Array.from({length: 8}, (_, i) => i + 1)
Array.from({length: 4}, (_, i) => Array.from({length: 4}, (_, j) => i * j))
Array.from({length: 20}, () => 10 * Math.random())
```

As you may know, `Array(n)` creates a sparse array, which is mostly not-handy, `Array(10).map(x => 1)[2] == undefined`

`Array.build(length, i => {})` was proposed before ES6, but let's go further proposing this new syntax:

```js
[1:9]
[:4].map(i => [:4].map(j => i * j))
[:20].map(() => 10 * Math.random())
```
The `[a:b]` syntax is similar to `array.slice` arguments, `a` is inclusive, `b` is exclusive. Both defaults to 0, so `[:]` is a valid syntax and creates an empty array

It would floor a non-integer length value (`b-a`), similarly to `Array.from({length: float})`

Let's go even further with this syntactic way to integrate the `.map`, for performance benefits:

```js
[1:9]
[:4:i => [:4:j => i * j]]
[:20:() => 10 * Math.random()]
```
It works with non-integers bounds:
```js
[1.2:6.2] // is [1.2, 2.2, 3.2, 4.2, 5.2]
```

Ranges can work along with other arrays values:
```js
[1, 2, 4:9, 10] // is [1, 2, 4, 5, 6, 7, 8, 10]
```

related discussion: https://esdiscuss.org/topic/ranges
