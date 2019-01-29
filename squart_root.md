# Square Roots

How to find a square root, programatically? Sounds simple right?? Maybe not so straight forwards, as discussed [here](https://math.stackexchange.com/questions/799339/how-to-calculate-the-square-root-of-a-number). 

Here we write a crude programmatic approach. We use the rule of thumb method to get a close first approximation and then brute force to the required precision or until maxiter is reached. We use a coefficient in the brute force loop to improve efficiency; the natural log of the target is used since the relationship relationship 

Here in JavaScript, first imperatively using a arrow function notation, then taking a [functional](shttps://medium.freecodecamp.org/functional-programming-in-js-with-practical-examples-part-1-87c2b0dbc276) approach using JavaScript's [EcmaScript(6+)](https://www.codementor.io/ajinkyax/functional-programming-with-javascript-es6-j4ysxgvpj) [[1](https://en.wikipedia.org/wiki/Ecma_International)] higher order function features and recursion.

(*highly recommend MPJ's [video series](https://www.youtube.com/channel/UCO1cgjhGzsSYb1rsB4bFe4Q/playlists) for all things JavaScript*). 

```javascript
// JavaScript ES6(+) arrow function notation
const sqrt = (target, precision = 0.001, maxiter = 10000) => {
  "use strict"
  // block scope
  let offset;
  let firstApprox;
  let base = 1;
  let guess; 
  let i;
  
  // first approximation
  while (base ** 2 <= target) {
    base++;
  }
  base--;
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
      console.log('offset', offset)
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


```css
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
```css
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

```css
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

FP 
