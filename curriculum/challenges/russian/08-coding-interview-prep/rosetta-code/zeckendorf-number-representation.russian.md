---
title: Zeckendorf number representation
id: 594810f028c0303b75339ad6
challengeType: 5
forumTopicId: 302346
localeTitle: Отметить номер деревни
---

## Description
<section id='description'>
<p> Подобно тому, как числа могут быть представлены в позиционной нотации как суммы кратных степеням десяти (десятичных) или двух (двоичных); все положительные целые числа могут быть представлены в виде суммы одного или нулевого числа отдельных членов ряда Фибоначчи. </p><p> Напомним, что первые шесть различных чисел Фибоначчи: <code>1, 2, 3, 5, 8, 13</code> . Десятичное число одиннадцать может быть записано как <code>0*13 + 1*8 + 0*5 + 1*3 + 0*2 + 0*1</code> или <code>010100</code> в позиционной нотации, где столбцы представляют собой умножение на определенный член последовательности. Ведущие нули отбрасываются так, что 11 десятичных знаков становятся <code>10100</code> . </p><p> 10100 - не единственный способ сделать 11 из чисел Фибоначчи, однако <code>0*13 + 1*8 + 0*5 + 0*3 + 1*2 + 1*1</code> или 010011 также будет представлять десятичную 11. Для истинного числа Зеекендорфа существует дополнительное ограничение, что «нельзя использовать два последовательных числа Фибоначчи», что приводит к первому уникальному решению. </p><p> Задача: Напишите функцию, которая генерирует и возвращает массив первых чисел N Zeckendorf по порядку. </p>
</section>

## Instructions
<section id='instructions'>
Write a function that generates and returns an array of the first <code>n</code> Zeckendorf numbers in order.
</section>

## Tests
<section id='tests'>

```yml
tests:
  - text: zeckendorf must be function
    testString: assert.equal(typeof zeckendorf, 'function');
  - text: Your <code>zeckendorf</code> function should return the correct answer
    testString: assert.deepEqual(answer, solution20);

```

</section>

## Challenge Seed
<section id='challengeSeed'>

<div id='js-seed'>

```js
function zeckendorf(n) {
  // good luck!
}

```

</div>

### After Tests
<div id='js-teardown'>

```js
const range = (m, n) => (
  Array.from({
    length: Math.floor(n - m) + 1
  }, (_, i) => m + i)
);

const solution20 = [
  '1', '10', '100', '101', '1000', '1001', '1010', '10000', '10001',
  '10010', '10100', '10101', '100000', '100001', '100010', '100100', '100101',
  '101000', '101001', '101010'
];

const answer = range(1, 20).map(zeckendorf);

```

</div>

</section>

## Solution
<section id='solution'>

```js
// zeckendorf :: Int -> String
function zeckendorf(n) {
  const f = (m, x) => (m < x ? [m, 0] : [m - x, 1]);
  return (n === 0 ? ([0]) :
    mapAccumL(f, n, reverse(
      tail(fibUntil(n))
    ))[1]).join('');
}

// fibUntil :: Int -> [Int]
let fibUntil = n => {
  const xs = [];
  until(
      ([a]) => a > n,
      ([a, b]) => (xs.push(a), [b, a + b]), [1, 1]
  );
  return xs;
};

let mapAccumL = (f, acc, xs) => (
  xs.reduce((a, x) => {
    const pair = f(a[0], x);

    return [pair[0], a[1].concat(pair[1])];
  }, [acc, []])
);

let until = (p, f, x) => {
  let v = x;
  while (!p(v)) v = f(v);
  return v;
};

const tail = xs => (
   xs.length ? xs.slice(1) : undefined
);

const reverse = xs => xs.slice(0).reverse();
```

</section>
