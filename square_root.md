
# Square Root

How to find a square root, programatically? Sounds simple right?? Maybe not so straight forwards, as discussed [here](https://math.stackexchange.com/questions/799339/how-to-calculate-the-square-root-of-a-number). 

Here we write a crude programmatic approach. We use the rule of thumb method to get a close first approximation and then brute force to the required precision or until maxiter is reached. We use a coefficient in the brute force loop to improve efficiency; the natural log of the target is used since the relationship relationship between a given target and its root is not linear.

Here in JavaScript; first imperatively using a arrow function notation, then taking a [functional](shttps://medium.freecodecamp.org/functional-programming-in-js-with-practical-examples-part-1-87c2b0dbc276) approach using JavaScript's [EcmaScript(6+)](https://www.codementor.io/ajinkyax/functional-programming-with-javascript-es6-j4ysxgvpj) [[1](https://en.wikipedia.org/wiki/Ecma_International)] higher order function features and recursion.

(*highly recommend MPJ's [video series](https://www.youtube.com/channel/UCO1cgjhGzsSYb1rsB4bFe4Q/playlists) for all things JavaScript*). 

## Imperative

```javascript
// JavaScript ES6(+) arrow function notation
const sqrt = (target, precision = 0.001, maxiter = 10000) => {
 
  // block scope
  let offset;
  let firstApprox;
  let base = 1;
  let guess; 
  let i;
  
  // first approximation
  do {
      base++;
    } while (base ** 2 <= target) 
  firstApprox = base + (target - base ** 2) / (2 * base);
  console.log('firstapprox', firstApprox)
  console.log('firstApprox squares to', firstApprox ** 2)
  console.log('firstApprox offset', Math.abs(target - firstApprox ** 2), '\n')

  // brute force until precision or maxiter
  guess = firstApprox
  for (i = 0; i < maxiter; i++) {
    offset = guess ** 2 - target;
    if (Math.abs(offset) <= precision) {
      break; 
    } else {
      // we use the natural log as a coefficent as it yielded
      // the fewest interations during testing; the optimal 
      // coffecient value is not known and the relationship is
      // not linear
      guess = guess - (offset * Math.log(target) / target);
    }
  }

  console.log("\nFirst approx interations", base + 1);
  console.log("Brute force interations ", i);
  console.log("Offset achieved", offset);
  console.log("Sqrt at given precision", guess);
  console.log("Target was", target);
  console.log("\nResult ", guess ** 2);
};
```


```javascript
sqrt(2068, 0.000001, 300)

firstApprox 45.477777777777774
firstApprox squares to 2068.228271604938
firstApprox offset 0.22827160493807241

First approx interations 46
Brute force interations 31
Offset achieved 7.088319762260653e-7
Sqrt at given precision 45.47526801140189

Target was 2068
Result 2068.000000708832

[Done] exited with code=0 in 0.052 seconds
```

sqrt(2) to 12 decimal places
```javascript
sqrt(2, 0.0000000000001, 300)

firstApprox 1.5
firstApprox squares to 2.25
firstApprox offset 0.25
  
First approx interations 2
Brute force interations 8
Offset achieved -2.886579864025407e-15
Sqrt at given precision 1.414213562373094

Target was 2
Result 1.9999999999999971

[Done] exited with code=0 in 0.053 seconds
```

```javascript
sqrt(1234567, 0.000001, 30000)

firstApprox 1111.110711071107
firstApprox squares to 1234567.012256941
firstApprox offset 0.012256941059604287

First approx interations 1112
Brute force interations 369
Offset achieved 9.778887033462524e-7
Sqrt at given precision 1111.1107055559216
 
Target was 1234567
Result 1234567.000000978

[Done] exited with code=0 in 0.053 seconds
```

## A more functional approach
[1](https://flaviocopes.com/javascript-functional-programming/), [2](https://flaviocopes.com/javascript-loops-map-filter-reduce-find/), [3](https://medium.com/dailyjs/functional-js-with-es6-recursive-patterns-b7d0813ef9e3), [4](https://www.vojtechruzicka.com/javascript-hoisting-var-let-const-variables/), [5](https://javascript.info/recursion), [6](https://www.codementor.io/ajinkyax/functional-programming-with-javascript-es6-j4ysxgvpj), [7](https://medium.freecodecamp.org/functional-programming-in-js-with-practical-examples-part-1-87c2b0dbc276)

```javascript
let offset
let firstApprox
let base
let ans
let maxDepth
let depth

"use strict"

const sqrt = (target, precision = 0.001, maxDepth = 10) => { 
  const findBase = (target, base = 1) =>
    base ** 2 <= target ? findBase(target, base+1): base - 1;
    
  base = findBase(target)
  firstApprox = base + ((target - base ** 2) / (2 * base))

  console.log('firstApprox', firstApprox)
  console.log('firstApprox squares to', firstApprox ** 2)
  console.log('firstApprox offset', Math.abs(target - firstApprox ** 2), '\n')

  const findSqrt = (target, guess, offset = 1, depth = 1) => (
    (Math.abs(offset) <= precision || depth == maxDepth) ?
      { guess: guess, result: guess ** 2, target: target, recursion: depth, offset: offset } :
      ( findSqrt(target, guess - (offset / target), guess ** 2 - target, depth + 1))
  )

  let ans = findSqrt(target, firstApprox)
  console.log('First approx interations', base + 1);
  console.log('Brute force interations', ans.recursion);
  console.log('Offset achieved', ans.offset);
  console.log('Sqrt at given precision', ans.guess);
  console.log('\nTarget was', ans.target);
  console.log('Result ', ans.result);
}
```
 
```javascript
sqrt(5445, 0.00001, 3000)

firstApprox 73.79452054794521
firstApprox squares to 5445.631262901108
firstApprox offset 0.6312629011081299 

First approx interations 74
Brute force interations 393
Offset achieved 0.000009805368790694047
Sqrt at given precision 73.79024332208142

Target was 5445
Result  5445.000009531982

[Done] exited with code=0 in 0.12 seconds
```

**findBase** - looks elegant and is quite easy to understand. 

**findSqrt** - it looks complicated and readability of the function could be improved with *[currying](https://wsvincent.com/javascript-currying/)*.

Neither, as tight loops, are not performant choices as functional implementations: *brute force*-style algorithms are not well suited for  recursive implementation; in JavaScript stack limit will limit the depth of recursion that can be achieved, and the the operation will likely result in high memory usage and expensive memory access patterns.

**TODO** - adapt for targets less than one.

**TODO** - apply power law to first approximation loop to increase the step size and reduce the number of iterations for large targets.
