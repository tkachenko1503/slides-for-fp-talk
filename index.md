---

layout: default

style: |

    .slide.center h2 {
        margin: 0px 0 30px 0;
    }

    .slide.center .copy {
        margin-top: 40px;
        font-size: 12px;
    }
---

# <img src="themes/yandex/images/logo_rus.svg">

## **{{ site.presentation.title }}** {#cover}

<div class="s">
    <img src="themes/yandex/images/logo_rus.svg">
</div>

{% if site.presentation.nda %}
<div class="nda"></div>
{% endif %}

<div class="info">
	<p class="author">{{ site.author.name }}, <br/> {{ site.author.position }}</p>
</div>

## &nbsp;
{:.section}

### Функциональное программирование


## Функциональное программирование

* ...1950 - 1960г
* ...LISP, Haskell
* ...javascript ?


## &nbsp;
{:.with-big-quote}
> Что такое ФП?


## &nbsp;
{:.with-big-quote}
> Это набор идей, а не чёткие инструкции и действия


## Основная идея
{:.section}

### Используем функции


## Функции

### Характеристики функций:

* Функции должны быть объектами высшего порядка


## Функции

~~~ javascript

var calculateTotalPrice = data => {
  // ...
};

fs.readFile('./offers.csv', 'utf8', calculateTotalPrice);

~~~


## Функции

### Характеристики функций:

* Функции должны быть объектами высшего порядка
* Функция не должна изменять внешнее состояние


## Функции

~~~ javascript

var someGlobalFlag = true;

var swapState = data => {
    if (data.needSwapState) {
        someGlobalFlag = !someGlobalFlag;
    }
};

~~~


## Функции

~~~ javascript

var someGlobalFlag = true;

var swapState = (data, flag) => {
    if (data.needSwapState) {
        return flag ? true : false;
    }
};

var newGlobalFlag = swapState(data, someGlobalFlag);

~~~


## Функции

### Характеристики функций:

* Функции должны быть объектами высшего порядка
* Функция не должна изменять внешнее состояние
* Функция не должна изменять переданные ей аргументы


## Функции

~~~ javascript

var toUpperCaseAll = (list) => {
    for (let i = 0; i < list.length; i++) {
        list[i] = list[i].toUpperCase();
    }

    return list;
};

toUpperCaseAll(['mike', 'leo', 'raf', 'don']);
// ['MIKE', 'LEO', 'RAF', 'DON']

~~~


## Функции

~~~ javascript

var toUpperCaseAll = (list) => {
    var result = [];

    for (let i = 0; i < list.length; i++) {
        result[i] = list[i].toUpperCase();
    }

    return result;
};

toUpperCaseAll(['mike', 'leo', 'raf', 'don']);
// ['MIKE', 'LEO', 'RAF', 'DON']

~~~


## Функции

### Характеристики функций:

* Функции должны быть объектами высшего порядка
* Функция не должна изменять внешнее состояние
* Функция не должна изменять переданные ей аргументы
* Результат функции зависит только от переданных ей аргументов


## Функции

~~~ javascript

var add = (a, b) => a + b;

add(2, 5) // 5
add(2, 5) // 5
add(2, 5) // 5

~~~


## &nbsp;
{:.with-big-quote}
> Чистые функции


## Основная идея №2
{:.section}
### Используем только функции


## &nbsp;

### Используем функции для:

* Расчётов
* ...Заменяем переменные
* ...Заменяем циклы и управляющие структуры
* ...Описываем изменение состояния программы


## Расчёты

~~~ javascript

a + b



var add = (a, b) => a + b;

add(2, 5);

~~~


## Переменные

~~~ javascript

var five = 5;



var five = () => 5;

~~~


## Циклы

~~~ javascript

var transformList = (callback, list) => {
    var result = [];

    for (let value of list) {
        let newValue = callback(value);

        result.push(newValue);
    }

    return result;
}

~~~


## Циклы

~~~ javascript

function transformList(callback, [x, ...list]) {
    if (!x) return [];

    return [callback(x),  . . . transformList(callback, list)];
}

~~~

## &nbsp;
{:.section}
### Изменение состояния программы


## Изменение состояния программы

* Инструменты
  * ...Замыкания
  * ...Каррирование
  * ...Композиция


## Замыкания

~~~ javascript

var add = function (x) {
    return function (y) {
        return x + y;
    };
};

var increment = add(1);
var addTen = add(10);

~~~


## Каррирование

~~~ javascript

var add3 = (a, b, c) =>  a + b + c;
var add3Curryed = curry(add3);

add3Curryed(1, 2, 3); // 6
add3Curryed(1)(2, 3); // 6
add3Curryed(1, 2)(3); // 6
add3Curryed(1)(2)(3); // 6

~~~


## Каррирование

~~~ javascript

curry = func => {
    var arity = func.length;

    return function f1(. . . args) {
        if (args.length >= arity) {
            return func.apply(null, args);
        } else{
            return function f2(. . . nargs) {
              return f1.apply(null, args.concat(nargs));
            };
        }
    };
};

~~~


## Композиция

~~~ javascript

var capitalize = str => str.charAt(0).toUpperCase() + str.slice(1);
var trim = str => str.trim();


var formatStr = compose(capitalize, trim);

~~~


## Композиция

~~~ javascript

var split = curry((separator, str) => str.split(separator));
var join = curry((separator, list) => list.join(separator));
var map = curry((func, list) => list.map(func));
var toLowerCase = str => str.toLowerCase();


var toSlug = compose(
  encodeURIComponent,
  joinString('-'),
  map(stringToLowerCase),
  splitString(' ')
);

~~~

## Композиция

~~~ javascript

var compose = (. . . funcs) =>
    (value) => funcs.reverse().reduce((v,fn) => fn(v), value);

~~~


## &nbsp;
{:.center}
![](pictures/Lego.jpg)


## Какие плюсы мы получаем?

* ...Предсказуемое поведение
* ...Чистые функции легче рафакторить
* ...Чистые функции проще отлаживать и тестировать
* ...Чистые функции легко поддаются оптимизации
* ...Можем откладывать и параллелить вычисления


## &nbsp;
{:.section}
### Как это использовать в&nbsp;реальной жизни?


## Мы живём в мире сайд-эффектов

* Запись логов
* Запись в localStorage
* Обращение к DOM элементам
* Отправка запросов на сервер
* и тд.


## Инструменты

* ramda.js
* ramda-fantasy.js

[fantasy-land](https://github.com/fantasyland/fantasy-land)


## Идеальный мир

~~~ javascript

var getRows = pluck('rows');
var getNames = pluck('name');

var renderUsersTable = Engine.render('users-table');

var getUsersFromDb = compose(getRows, User.findAll);

var cleanUpData = compose(capitalise, getNames);

var makePage = compose(renderUsersTable, map(cleanUpData), getUsersFromDb);


makePage( {limit: 20} );

~~~


## &nbsp;
{:.section}
### Контейнеры


## Контейнеры

~~~ javascript

var Container = function(x) {
    this.__value = x;
};

Container.of = x => new Container(x);

Container.prototype.map = func => {
    return Container.of(func(this.__value));
};

~~~


## Контейнеры

~~~ javascript

Container.of(2).map(function(two){ return two + 2 });
// Container(4)

~~~


## &nbsp;
{:.section}
### NullPointerException


## NullPointerException

~~~ javascript

var getUsersFromDb = compose(getRows, User.findAll);

~~~


## NullPointerException

~~~ javascript

var getUsersFromDb = compose(getRows, Maybe.of, User.findAll);

~~~


## NullPointerException

~~~ javascript

var getUsersFromDb = compose(map(getRows), Maybe.of, User.findAll);

getUsersFromDb() // Maybe([{name: 'leo', name: 'don']);
getUsersFromDb() // Maybe(null);

~~~


## Maybe

~~~ javascript

var Maybe = function(x) {
    this.__value = x;
}

Maybe.of = function(x) {
    return new Maybe(x);
}

Maybe.prototype.isNothing = function() {
    return (this.__value === null || this.__value === undefined);
}

Maybe.prototype.map = function(f) {
    return this.isNothing() ? Maybe.of(null) : Maybe.of(f(this.__value));
}

~~~


## &nbsp;
{:.section}
### Обработка ошибок


## Обработка ошибок

~~~ javascript

var renderUsersTable = Engine.render('users-table');

var makePage = compose(
  map(renderUsersTable),
  map(cleanUpData),
  getUsersFromDb
);

~~~


## Обработка ошибок

~~~ javascript

var errorMessage = 'sorry there is no users';
var errorContainer = Either.Left(errorMessage);

var getUsers = cond([
    [isArrayLike, Either.Right],
    [T, always(errorContainer)]
]);

var getUsersFromDb = compose(map(getRows), getUsers, User.findAll);

~~~


## Обработка ошибок

~~~ javascript

var renderUsersTable = Engine.render('users-table');
var renderErrorMessage = Engine.render('error-message');

var render = Either.either(renderErrorMessage, renderUsersTable);

~~~


## Обработка ошибок

~~~ javascript

var makePage = compose(render, getUsersFromDb);

~~~


## &nbsp;
{:.section}
### Асинхронность


## Асинхронность

~~~ javascript

var getUsersFromDb = compose(getRows, User.findAll);

~~~


## Асинхронность

~~~ javascript

var findAll = params =>
    Future((reject, resolve) =>
        User.findAll(params)
            .then(resolve)
            .catch(reject));

var getUsersFromDb = compose(map(getRows), findAll);

var makePage = compose(map(cleanUpData), getUsersFromDb);

~~~


## Асинхронность

~~~ javascript

var page = makePage({limit: 20});

page.fork(renderErrorMessage, renderUsersTable);

~~~


## &nbsp;
{:.section}
### Всё вместе


## Всё вместе

~~~ javascript

var getRows = pluck('rows');
var getNames = pluck('name');
var cleanUpData = compose(capitalise, getNames);

var renderUsersTable = Engine.render('users-table');
var renderErrorMessage = Engine.render('error-message');

var render = Either.either(renderErrorMessage, renderUsersTable);

~~~


## Всё вместе

~~~ javascript

var render = . . .

var errorMessage = 'sorry there is no users';
var errorContainer = Either.Left(errorMessage);

var getUsers = cond([
  [isArrayLike, Either.Right],
  [T, always(errorContainer)]
]);

var findAll = params =>
    Future((reject, resolve) =>
        User.findAll(params).then(resolve).catch(reject));

var getUsersFromDb = compose(flatMap(getRows), map(getUsers), findAll);

~~~


## Всё вместе

~~~ javascript

var render = . . .
var getUsersFromDb = . . .

var makePage = compose(map(render), flatMap(cleanUpData), getUsersFromDb);

var page = makePage({limit: 20});

page.fork(logError, insertToDOM);

~~~


## &nbsp;
{:.center}
![](pictures/Happy.jpg)

Homer Simpson.  ©1999 20TH CENTURY FOX FILM CORP.
{:.copy}


## Плюсы

* ...Легко рефакторить
* ...Легко тестировать
* ...Легко оптимизировать
* ...За функциональным программированием будущее


## Минусы

* ...Есть порог вхождения
* ...Может быть слишком многословным
* ...Может быть менее производительным


## Полезные ссылки

* [https://github.com/MostlyAdequate/mostly-adequate-guide-ru](https://github.com/MostlyAdequate/mostly-adequate-guide-ru)
* [http://ramdajs.com/docs/](http://ramdajs.com/docs/)
* [https://github.com/ramda/ramda-fantasy](https://github.com/ramda/ramda-fantasy)
* [https://github.com/fantasyland/fantasy-land](https://github.com/fantasyland/fantasy-land)


## **Контакты** {#contacts}

<div class="info">
<p class="author">{{ site.author.name }}</p>
<p class="position">{{ site.author.position }}</p>

    <div class="contacts">
        <p class="contacts-left mail">ts1503@yandex-team.ru</p>

        <p class="contacts-left contacts-top twitter">@tka4enko1503</p>
    </div>
</div>
