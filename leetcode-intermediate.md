# Fraction Recuring

```js
fractionToDecimal(a, b) {
  if (a === 0) return "0";

  let result = "";

  if ((a < 0) !== (b < 0)) // Find correct sign
    result += "-";

  let numerator = Math.abs(a);
  let denominator = Math.abs(b);
  result += Math.floor(numerator / denominator);
  let remainder = numerator % denominator;

  if (remainder === 0) return result; // หารได้เศษ 0

  result += ".";
  const seen = new Map();

  while (remainder !== 0) {
    if (seen.has(remainder)) {
      const index = seen.get(remainder);
      result = result.slice(0, index) + "(" + result.slice(index) + ")";

      break;
    }

    seen.set(remainder, result.length);

    remainder = remainder * 10;
    result = result + Math.floor(remainder / denominator);
    remainder = remainder % denominator;
  }

  return result;
}
```
