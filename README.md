# Вопросы для собеседования по JavaScript

Рекомендуется к прочтению: [Cracking the Coding Interview](https://www.ozon.ru/context/detail/id/148410382/)

<hr />

Что будет в консоли и почему?:

```javascript
var a = 5;
function test () {
    a = 10;
    if (false) {
        var a = 20;
    }
}
test();
console.log(a); // ?
```

<details>
<summary><b>Ответ</b></summary>
Будет выведено число 5. Эквивалентная запись:

```javascript
var a = 5;
function test () {
    var a; // Объявление переменной через var поднимается
    a = 10;
    if (false) {
        a = 20;
    }
}
test();
console.log(a); // ?
```
</details>

<hr />

Какие числа выведутся и с какой задержкой?:

```javascript
for (var i = 0; i < 10; i++) {
    setTimeout(() => { console.log(i); }, 1000);
}
// Как сделать чтобы выводились числа по порядку (несколько способов)?
```

<details>
<summary><b>Ответ</b></summary>
Число 10 будет выведено 10 раз. Чтобы вывести числа от 0 до 9 можно использовать следующие записи:
    
```javascript
for (let i = 0; i < 10; i++) {
    setTimeout(() => { console.log(i); }, 1000);
}
```

```javascript
for (var i = 0; i < 10; i++) {
    const j = i;
    setTimeout(() => { console.log(j); }, 1000);
}
```
</details>

<hr />

Нечётные числа должны отсортироваться по возрастанию, а чётные должны остаться на своих местах:

```javascript
const nums = [1,9,4,2,3,6,7,1,5]; // [1,1,4,2,3,6,5,7,9]
```

<details>
<summary><b>Ответ</b></summary>

```javascript
function sortOdd(nums) {
    const oddNums = nums
        .filter((el) => el % 2 === 1)
        .sort((a, b) => a - b);
    let topI = 0;
    return nums.map((el) => {
        if (el % 2 === 1) {
            return oddNums[topI++];
        }
        return el;
    });
}
```
</details>

<hr />

Найти не повторяющиеся числа:

```javascript
const arr = [1,2,3,4,1,2]; // [3,4]
```

<details>
<summary><b>Ответ</b></summary>

```javascript
function unique(nums) {
    const seen = new Set();
    const res = new Set();
    for (const num of nums) {
        if (seen.has(num)) {
            if (res.has(num)) {
                res.delete(num);
            }
        } else {
            seen.add(num);
            res.add(num);
        }
    }
    return Array.from(res);
}
```
</details>

<hr />

Сделать вложенный объект из строки:

```javascript
const str = 'one.two.three.four.five';

/*{
    one: {
        two: {
            three: {
                ...
            }
        }
    }
}*/
```

<hr />

`arr.push(0)` повлияет на массив так же, как если бы мы выполнили:

```javascript
const arr = [1,2,3,4,5];
/*1*/ arr[0] = 0;
/*2*/ arr[arr.length] = 0;
/*3*/ arr[arr.length – 1] = 0;
/*4*/ arr[-1] = 0;
```

<hr />

Какие логические значения будут получены?:

```javascript
const object = {
    foo: 1
};
console.log([
    'foo' in object,                  // ?
    'toString' in object,             // ?
    object.hasOwnProperty('foo'),     // ?
    object.hasOwnProperty('toString') // ?
]);
```

<hr />

Что вернёт следующий код?:

```javascript
Object.create(null).hasOwnProperty('toString');
```

<hr />

Для каждого вложенного объекта нужно добавить свойство `level`, которое равняется числу (номер вложенности).
Если значение свойства будет не объект, то ничего не добавлять:

```javascript
const object = {
    a: {
        d: {
            h: 4
        },
        e: 2
    },
    b: 1,
    c: {
        f: {
            g: 3,
            k: {}
        }
    }
}

// Должно получиться так:
/*
{
    a: {
        level: 1
        d: {
            level: 2,
            h: 4
        },
        e: 2
    },
    b: 1,
    c: {
        level: 1
        f: {
            level: 2
            g: 3,
            k: {
                level: 3
            }
        }
    }
}
*/
```

<hr />

Как можно заставить браузеры не кэшировать POST запрос?:

```javascript
// Рассказать (вопрос с подвохом)
```

<hr />

Какое значение будет в `object.property`?:

```javascript
const object = {};
object.constructor.prototype.property = 1;
object.constructor.prototype = null;
console.log(object.property); // ?
```

<hr />

Что выведется в `console.log`?:

```javascript
let i = 10, arr = [];
while (i--) {
    arr.push(() => i + i);  
}
console.log([arr[0](), arr[0]()]); // ?
```

<hr />

Написать функцию, которая будет возвращать `true` если строка (слово) является палиндромом, иначе `false`:

```javascript
function isPalindrom (str) {
    //...
}
isPalindrom('казак'); // true
isPalindrom('строка'); // false
isPalindrom('шалаш'); // true
```

<hr />

Напишите функцию, которая выводит в консоль числа от `1` до `n`, где `n` — целое число,
которое функция принимает в качестве параметра, при этом:
* выводит `fizz` вместо чисел, кратных 3;
* выводит `buzz` вместо чисел, кратных 5;
* выводит `fizzbuzz` вместо чисел, кратных и 3, и 5.

```javascript
function fizzBuzz (num) {
    //...
}
fizzBuzz(5);
// 1
// 2
// fizz
// 4
// buzz
```

<hr />

Напишите функцию, проверяющую, являются ли две строки анаграммами друг друга (регистр букв не имеет значения).
Важны только символы, пробелы или знаки препинания не учитываются:

```javascript
function isAnagram (str) {
    //...
}
isAnagram('finder', 'Friend'); // true
isAnagram('hello', 'bye'); // false
```

<hr />

Написать функцию сложения вида `sum(1)(2)(3)...`:

```javascript
function sum (num) {
    //...
}
sum(1)(2)(3)(4); // 10
```
