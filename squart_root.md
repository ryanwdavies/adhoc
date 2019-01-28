# Square Roots

How to find 

```javascript
const target = 2068;
const maxiter = 300;
const precision = 0.000001;
let offset;
let firstapprox;
let base = 1;
let guess;

// EcmaScript 6+ arrow function notation
const sqrt = (target, precision, maxinter) => {
  // first approximation
  while (base ** 2 <= target) {
    base++;
  }
  base--;
  firstapprox = base + (target - base ** 2) / (2 * base);
  console.log('firstapprox', firstapprox)
  console.log('FA squares to', firstapprox ** 2)
  console.log('FA offset', Math.abs(target - firstapprox**2), '\n')

  // brute force until precision or maxiter
  guess = firstapprox
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


``` 
firstapprox 45.477777777777774
FA squares to 2068.228271604938
FA offset 0.22827160493807241

First approx interations 46
Brute force interations 31
Offset achieved 7.088319762260653e-7
Sqrt at given precision 45.47526801140189

Target was 2068
Result 2068.000000708832
```
