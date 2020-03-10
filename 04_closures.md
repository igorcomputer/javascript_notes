## Замыкания в Javascript (Closure) 
<hr>

Замыкание — это функция, объявленная внутри другой функции и имеющая доступ к переменным внешней (вмещающей) функции. 

Замыкание имеет доступ сразу к трем областям видимости:

- к своей собственной области видимости (переменные, объявленные внутри замыкания);
- к области видимости внешней функции (переменные, объявленные внутри внешней функции);
- к глобальной области видимости.

```javascript 
function outerFn(myArg) {
   var myVar;
   function innerFn() {
      //имеет доступ к myVar и myArg
   }
}
```

При этом, такие переменные продолжают существовать и остаются доступными внутренней функцией даже после того, как внешняя функция, в которой они определены, была исполнена. 

__Простыми словами замыкание — это функция, описанная внутри другой функции.__


### Примеры использования замыканий
<hr>

__Счетчики__

```javascript  

//////////////////////////
// Вариант 1 
//////////////////////////

function makeCounter() {
  let count = 1;
  return function() {
    return count++; // есть доступ к внешней переменной "count"
  };
}

let counter = makeCounter();

// каждый вызов увеличивает счётчик и возвращает результат
alert( counter() ); // 1
alert( counter() ); // 2
alert( counter() ); // 3

// создать другой счётчик, он будет независим от первого
var counter2 = makeCounter();
alert( counter2() ); // 1

//////////////////////////
// Вариант 2 
//////////////////////////

var fn = (function() {
  var numberOfCalls = 0;
  return function() {
    return ++ numberOfCalls;
  }
})();

//////////////////////////
// Вариант 3 (счетчик с помощью свойства функции) 
//////////////////////////

function makeCounter() {
  function counter() {
    return counter.currentCount++;
  };
  counter.currentCount = 1;
  return counter;
}

var counter = makeCounter();
alert( counter() ); // 1
alert( counter() ); // 2

```

__Запоминание параметров__

```javascript

///////////////////////////////////////
// Пример с запоминанием 
///////////////////////////////////////

var createHelloFunction = function(name) {
  return function() {
    alert('Hello, ' + name);
  }
}
var sayHelloHabrahabr = createHelloFunction('Habrahabr'); // Запомнили 
sayHelloHabrahabr(); //alerts «Hello, Habrahabr» // Используем ранее запомненное 

///////////////////////////////////////
// Суммировние и подобные варианты (умножение, факториал) 
///////////////////////////////////////

function sum(a) {
  return function(b) {
    return a + b; // берёт "a" из внешнего лексического окружения
  };
}

alert( sum(1)(2) ); // 3
alert( sum(5)(-1) ); // 4

///////////////////////////////////////////
// Умножение числа на любое заданное число
///////////////////////////////////////////
function multiplier(factor) {
  return function(number) {
    return number * factor;
  };
}

var twice = multiplier(2);
console.log(twice(5));
```

__Инкапсуляция__

```javascript
////////////////////////////////////////
// Инкапсуляция приватной переменной 
////////////////////////////////////////
function user() {
    var name = ‘Unknown’;
    return {
        getName: function() {
            return name;
        },
        setName: function(newName) {
            name = newName;
        }
    }
}

var testUser = user();

testUser.getName(); // Unknown
testUser.setName(‘John Smith’); // Изменяем значение приватной переменной
testUser.getName(); // John Smith 

////////////////////////////////////////
// Инкапсуляция с помощью паттерна Модуль 
////////////////////////////////////////
var MyModule = (function() {
   var name = 'Habrahabr';
   function sayPreved() {
      alert('PREVED ' + name.toUpperCase());
   }
   return {
      sayPrevedToHabrahabr: function() {
         sayPreved(name);
      }
   }
})();
MyModule.sayPrevedToHabrahabr(); //alerts «PREVED Habrahabr»
```

__Использование в циклах__

```javascript
////////////////////////////
// Задача - создать несколько функций, которые вызовутся через определенные отрезки времени
////////////////////////////

for (var i = 1; i <= 5; i++) {
    setTimeout(function() {
        console.log(i);
    }, i * 1000);
}
// Нужно показывать числа 1, 2, 3, 4, 5, но показывает 6, 6, 6, 6, 6. 

////////////////////////////////////////////
// Классическое решение с помощью замыкания
////////////////////////////////////////////

for(var i = 1; i <= 5; i++) {
  (function(i) {
    setTimeout(function() {
      console.log(i);
    }, i * 1000);
  })(i);
}

// Варианты решений подобных задач можно посмотреть по ссылке: 
https://ru.stackoverflow.com/questions/433887/Почему-асинхронная-функция-внутри-цикла-выполняет-последнюю-итерацию-много-раз/433888#433888
```

Ссылки

- https://getinstance.info/articles/javascript/closures-in-javascript/
- https://learn.javascript.ru/closure
- https://habr.com/ru/post/38642/
- https://monsterlessons.com/project/lessons/zamykaniya-v-javascript
- https://habr.com/ru/company/ruvds/blog/340194/

   