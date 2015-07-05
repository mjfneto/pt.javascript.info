# Переменные: let и const

В ES-2015 предусмотрены новые способы объявления переменных: через `let` и `const` вместо `var`.

Например:
```js
let a = 5;
```

## let

У объявлений `let` три основных отличия от `var`:

<ol>
<li>**Область видимости переменной `let` -- блок `{...}`.**

Как мы помним, переменная, объявленная через `var`, видна везде в функции. В старом стандарте минимальная "область видимости" -- функция.

Переменная, объявленная через `let`, видна только в рамках блока `{...}`, в котором объявлена.

Это, в частности, влияет на объявления внутри `if`, `while` или `for`.

Например, переменная через `var`:

```js
//+ run 
'use strict';

var apples = 5; 

if (true) {
  var apples = 10;

  alert(apples); // 10 (внутри блока)
}

alert(apples); // 10 (снаружи блока то же самое)
```

То же самое с `let`:

```js
//+ run 
'use strict';

let apples = 5; // (*)

if (true) {
  let apples = 10;

  alert(apples); // 10 (внутри блока)
}

*!*
alert(apples); // 5 (снаружи блока значение не изменилось)
*/!*
```

Здесь, фактически, две независимые переменные `apples`, одна -- глобальная, вторая -- в блоке `if`.

Заметим, что если объявление `apples` в строке `(*)` закомментировать, то в последнем `alert` будет ошибка: переменная неопределена. Это потому что переменная `let` всегда видна именно в том блоке, где объявлена и не более.

</li>
<li>**Переменная `let` видна только после объявления.**

Как мы помним, переменные `var` существуют и до объявления. Они равны `undefined`:

```js
//+ run
'use strict';

alert(a); // undefined

var a = 5;
```

С переменными `let` всё проще. До объявления их вообще нет.

Такой доступ приведёт к ошибке:
```js
//+ run
'use strict';

*!*
alert(a); // ошибка, нет такой переменной
*/!*

let a = 5;
```

Заметим также, что переменные `let` нельзя повторно объявлять. То есть, такой код выведет ошибку:

```js
//+ run
'use strict';

let x;
let x; // ошибка: переменная x уже объявлена
```

Это -- хоть и выглядит ограничением по сравнению с `var`, но на самом деле проблем не создаёт, так как область видимости ограничена блоком.

Например, два таких цикла совсем не конфликтуют:
```js
//+ run
'use strict';

for(let i = 0; i<10; i++) { /* … */ }
for(let i = 0; i<10; i++) { /* … */ }

alert( i ); // ошибка, переменная не определена
```

При объявлении внутри цикла переменная `i` будет видна только в блоке цикла. Она не видна снаружи, поэтому будет ошибка в последнем `alert`.


</li>
<li>**При использовании в цикле, для каждой итерации создаётся своя переменная.**

Переменная `var` -- одна на все итерации цикла (и видна после цикла):

```js
//+ run
for(var i=0; i<10; i++) { /* … */ }

alert(i); // 10
```

С переменной `let` -- всё по-другому. Добавляется ещё одна область видимости: блок цикла.

Каждому блоку цикла соответствует своя, независимая, переменная `let`. Если внутри цикла объявляются функции, то в замыкании каждой будет та переменная, которая была при итерации.

Это позволяет легко решить классическую проблему с замыканиями, описанную в задаче [](/task/make-army).

```js
//+ run
'use strict';

function makeArmy() {

  let shooters = [];

  for (*!*let*/!* i = 0; i < 10; i++) {
    shooters.push(function() {
      alert( i ); // выводит свой номер
    });
  }

  return shooters;
}

var army = makeArmy();

army[0](); // 0
army[5](); // 5
```

Если бы объявление было `var i`, то была бы одна переменная `i` на всю функцию, и вызовы в последних строках выводили бы `10` (подробнее -- см. задачу [](/task/make-army)). 

А выше объявление `let i` создаёт для каждого повторения блока в цикле свою переменную, которую функция и получает из замыкания в последних строках.
</li>
</ol>

## const

Объявление `const` задаёт константу, то есть переменную, которую нельзя менять:

```js
//+ run
'use strict';

const apple = 5;
apple = 10; // ошибка
```

В остальном объявление `const` полностью аналогично `let`.

## Итого


Переменные `let`:

<ul>
<li>Видны только после объявления и только в текущем блоке.</li>
<li>Нельзя переобъявлять (в том же блоке).</li>
<li>В цикле каждое значение `let` принадлежит конкретной итерации цикла (и видно в замыканиях).</li>
</ul>

Переменная `const` -- это константа, в остальном -- как `let`.
