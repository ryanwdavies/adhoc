

```javascript
const  target  =  2068;
const  maxinter  =  300;
const  precision  =  0.000001;
let  offset
let  firstapprox
let  base  =  1;
let  guess;

// EcmaScript 6+ arrow function notation
const  sqrt  = (target, precision, maxinter) => {
  // first approximation
  while (base  **  2  <=  target) {
    base++;
  }
  base--;
  firstapprox  =  base  + (target  -  base  **  2) / (2  *  base);
  console.log('firstapprox', firstapprox)
  console.log('FA squares to ', firstapprox**2)
  console.log('FA offset', Math.abs(target  -  firstapprox**2), '\n')

  // brute force until precision
  guess  =  firstapprox
  for (i  =  0; i  <  maxinter; i++) {
    offset  =  guess  **  2  -  target;
    if (Math.abs(offset) <=  precision) {
      break; 
    } else { . 
      console.log('offset', offset)
      guess  =  guess  - (offset*20  /  target);
    }
  }

  console.log("\nFirst approx interations ", base  +  1);
  console.log("Brute force interations ", i);
  console.log("Offset achieved", offset);
  console.log("Sqrt at given precision", guess);
  console.log("Target was", target);
  console.log("\nResult ", guess  **  2);
};
```


``` 
sqrt(target, precision, maxinter);

firstapprox 45.477777777777774
FA squares to 2068.228271604938
FA offset 0.22827160493807241

offset 0.22827160493807241
offset 0.027477923124024528
offset 0.0033082796271628467
offset 0.0003983188908023294
offset 0.00004795797576662153
offset 0.000005774188593932195

First approx interations 46
Brute force interations 6
Offset achieved 6.952182047825772e-7
Sqrt at given precision 45.475268011252204

Target was 2068
Result 2068.000000695218
```
