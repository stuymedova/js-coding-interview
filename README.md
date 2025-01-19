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

<details>
<summary><b>Ответ</b></summary>

```javascript
function makeObjByPath(path) {
    const segments = path.split('.');
    const res = {};
    let curr = res;
    for (const segment of segments) {
        curr[segment] = {};
        curr = curr[segment];
    }
    return res;
}
```
</details>

<hr />

`arr.push(0)` повлияет на массив так же, как если бы мы выполнили:

```javascript
const arr = [1,2,3,4,5];
/*1*/ arr[0] = 0;
/*2*/ arr[arr.length] = 0;
/*3*/ arr[arr.length – 1] = 0;
/*4*/ arr[-1] = 0;
```

<details>
<summary><b>Ответ</b></summary>

```javascript
/*2*/ arr[arr.length] = 0;
```
</details>

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

<details>
<summary><b>Ответ</b></summary>

```javascript
[ true, true, true, false ]
```
</details>

<hr />

Что вернёт следующий код?:

```javascript
Object.create(null).hasOwnProperty('toString');
```

<details>
<summary><b>Ответ</b></summary>

Будет выброшена ошибка, так как метод hasOwnProperty отсутствует у объекта, созданного через Object.create(null).

```javascript
TypeError: Object.create(...).hasOwnProperty is not a function
```

Чтобы избежать ошибки можно использовать `in` или `Object.prototype.hasOwnProperty`:

```javascript
'toString' in Object.create(null); // false
```

```javascript
Object.prototype.hasOwnProperty.call(Object.create(null), 'toString'); // false
```

</details>

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

<details>
<summary><b>Ответ</b></summary>

```javascript
function addLevelsDFS(obj, level = 1) {
    for (const key of Object.keys(obj)) {
        if (obj[key] instanceof Object && !Array.isArray(obj[key])) {
            obj[key].level = level;
            addLevelsDFS(obj[key], level + 1);
        }
    }
    return obj;
};
```

```javascript
function addLevelsBFS(obj) {
    let q = Object.values(obj);
    let levelI = 1;
    while (q.length) {
        const level = [...q];
        q = [];
        for (let i = 0; i < level.length; i++) {
            if (level[i] instanceof Object && !Array.isArray(level[i])) {
                level[i].level = levelI;
                for (const val of Object.values(level[i])) {
                    q.push(val);
                }
            }
        }
        levelI += 1;
    }
    return obj;
};
```
</details>

<hr />

Как можно заставить браузеры не кэшировать POST запрос?:

```javascript
// Рассказать (вопрос с подвохом)
```

<details>
<summary><b>Ответ</b></summary>

Браузеры не кэшируют POST запрос поскольку он (согласно конвенции) является мутирующим.
</details>

<hr />

Какое значение будет в `object.property`?:

```javascript
const object = {};
object.constructor.prototype.property = 1;
object.constructor.prototype = null;
console.log(object.property); // ?
```

<details>
<summary><b>Ответ</b></summary>

Будет выведено число 1.

```javascript
const object = {};
object.constructor.prototype.property = 1; // все объекты наследующие от Object.prototype получают доступ к property
object.constructor.prototype = null; // не работает, constructor.prototype нельзя переназначить так, чтобы он удалил существующую цепочку прототипов
console.log(object.property);
```
</details>

<hr />

Что выведется в `console.log`?:

```javascript
let i = 10, arr = [];
while (i--) {
    arr.push(() => i + i);  
}
console.log([arr[0](), arr[1]()]); // ?
```

<details>
<summary><b>Ответ</b></summary>

```javascript
[ -2, -2 ]
```

Для того, чтобы вывести `[ 18, 16 ]` можно отредактировать код следующим образом:

```javascript
let i = 10, arr = [];
while (i--) {
    const j = i;
    arr.push(() => j + j);
}
console.log([arr[0](), arr[1]()]);
```
</details>

<hr />

Написать функцию, которая будет возвращать `true` если строка (слово) является палиндромом, иначе `false`:

```javascript
function isPalindrome(str) {
    //...
}
isPalindrome('казак'); // true
isPalindrome('строка'); // false
isPalindrome('шалаш'); // true
```

<details>
<summary><b>Ответ</b></summary>

```javascript
function isPalindrome(str) {
    let l = 0;
    let r = str.length - 1;
    while (l < r) {
        if (str[l] !== str[r]) {
            return false;
        }
        l += 1;
        r -= 1;
    }
    return true;
}
```
</details>

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

<details>
<summary><b>Ответ</b></summary>

```javascript
function fizzBuzz(num) {
    for (let i = 0; i <= num; i++) {
        if (i % 3 === 0 && i % 5 === 0) {
            console.log('fizzbuzz');
        } else if (i % 3 === 0) {
            console.log('fizz');
        } else if (i % 5 === 0) {
            console.log('buzz');
        } else {
            console.log(i);
        }
    }
}
```
</details>

<hr />

Напишите функцию, проверяющую, являются ли две строки анаграммами друг друга (регистр букв не имеет значения).
Важны только символы, пробелы или знаки препинания не учитываются:

```javascript
function isAnagram(str1, str2) {
    //...
}
isAnagram('finder', 'Friend'); // true
isAnagram('hello', 'bye'); // false
```

<details>
<summary><b>Ответ</b></summary>

```javascript
function isAlphaNumeric(char) {
    return char.toLowerCase() !== char.toUpperCase() || !Number.isNaN(parseInt(char));
}

function isAnagram(str1, str2) {
    const frequencies = {};
    for (let i = 0; i < str1.length; i++) {
        const char = str1[i].toLowerCase();
        if (!isAlphaNumeric(char)) {
            continue;
        }
        if (!frequencies[char]) {
            frequencies[char] = 0;
        }
        frequencies[char] += 1;
    }
    let rest = str2.length;
    for (let i = 0; i < str2.length; i++) {
        const char = str2[i].toLowerCase();
        if (!isAlphaNumeric(char)) {
            rest -= 1;
            continue;
        }
        if (!frequencies[char]) {
            return false;
        }
        frequencies[char] -= 1;
        rest -= 1;
    }
    return rest === 0;
};
```
</details>

<hr />

Написать функцию сложения вида `sum(1)(2)(3)...`:

```javascript
function sum (num) {
    //...
}
sum(1)(2)(3)(4); // 10
```
