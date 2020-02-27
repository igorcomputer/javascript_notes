## Функции в Javascript
<hr>

Рассмотрим некоторые виды функций в Javascript: 

### Функции Объявления (Function Declaration - функциональное объявление) 

После объявления функции необязательно ставить точку с запятой. 

__Базовый синтаксис__:

```javascript 
// Объявление функции
function showMessage() {
  alert( 'Всем привет!' );
}

// Использование функции
showMessage()
```

Особенность данного вида функций: 

__Объявление функции может размещаться после ее использования в коде.__

### Функции Выражения (Function Expression - функциональные выражения) 
<hr>

Также данный вид функций называют - __Named Function Expression (NFE)__ 

Особенность данного вида функций: 
__Объявление функции может размещаться только до ее использования в коде.__

__Базовый синтаксис__:

```javascript 
/// Объявление функции
let sayHi = function() {
  alert( "Привет" );
};

// Использование функции
sayHi()
```

### Функции обратного вызова (Callback Function) 
<hr>

__Функция обратного вызова называется функция, переданная в качестве параметра в аргументы другой функции.__

```javascript 
// Function Expression 
function ask(question, yes, no) {
  if (confirm(question)) yes() // Callback Function
  else no(); // Callback Function 
}

// Callback Declaration 
function showOk() {
  alert( "Вы согласны." );
}

// Callback Declaration 
function showCancel() {
  alert( "Вы отменили выполнение." );
}

// Использование: функции showOk, showCancel передаются в качестве аргументов ask
ask("Вы согласны?", showOk, showCancel);
```

__В качестве функций обратного вызова могут быть анонимные функции или функции стрелки__

```javascript 
// Function Expression 
function testF(x, m = function(){ alert(x) }) { 
  m(x); 
}

// Вызов 
testF(100); // Сработает анонимная функция, заданная по умолчанию 
```

### Стрелочные функции (Arrow Function - Функции стрелки) 
<hr> 

- Данные функции представляют собой сокращенную запись функций 
- Можно использовать их в качестве Function Expression 
- Функции-стрелки отличаются от остальных функций только кодом объявления

```javascript
// Общий вид объявления Arrow Function
let func = (arg1, arg2, ...argN) => expression 

// Общий вид объявления Function Expression
let func = function(arg1, arg2, ...argN) {
  return expression;
};

// Пример объявления Arrow Function
let sum = (a, b) => a + b;

// Пример объявления для Function Expression 
let sum = function(a, b) {
  return a + b;
};

// Пример вызова функций 
let result = sum(1, 2); // одинаковый вызов для  Arrow Function и Function Expression
```

__Вызов Функций-стрелок с сокращенным количеством аргументов:__ 

```javascript
// Объявление Arrow Function с одним аргументом
let double = n => n * 2;

// Объявление Arrow Function без аргументов
let sayHi = () => alert("Hello!"); 
```
__Многострочные стрелочные функции__

```javascript
// В многострочном виде Arrow Function используются фигурные скобки (Function Body) 
let sum = (a, b) => {
  let result = a + b;
  return result; // !при фигурных скобках для возврата значения нужно явно вызвать return
};
```

### Самовызывающиеся функции - Immediately Invoked Function Expression (IIFE) 

Самовызывающиеся функции —  это функции, которые способны вызывать сами себя. 

Такие функции как правило не надо вызывать они вызывают сами себя. 
Обычно это надо для инкапсуляции переменных и методов. 

```javascript
// Пример простой функции 
(function(){
    alert("Hello IIFE Function");
}());

// Пример функции с передачей параметров
(function(m){
  var res = 1;
  for(var i=1; i<=n; i++){
    res *=i;
  }  
  console.log("Факториал числа " + m + " равен " + res);
}(6));
```

__Функции могут возвращать другие функции:__

```javascript 
/// Объявление функции
let result = function sayHi(){
  return function(){
  	alert( "Привет" );
  }
};

let x = result(); // Вызвали функцию sayHi()  
x();  // Вызвали alert("Привет"); 

Тоже самое по другому: 
result()(); // Вызвали alert("Привет"); 
```

### Анонимные и именованные функции 

Примеры: 

```javascript 
const myFunc  = function() { };                // 1 - анонимая 
const myFuncA = function myFuncA2() { };       // 2 - именованная 
const myFuncB = () => { };                     // 3 - анонимая 

// Объявление функции в качествео объекта
const myFuncC = new Function()                 // 4 - анонимаяconst property = Symbol('symbolProperty')

const myObject = { 
  methodA: function() { },                     // 5 - анонимная
  methodB: function MyMethodB() {},            // 6 - именованная
  methodC: () => { },                          // 7 - анонимная
  methodD(){ },                                // 8 - анонимная
  [property](){ }                              // 9 - анонимная
}

function myFuncD() { };                        // 10 - именованная
(function() { })()                             // 11 - анонимая
(function myFuncF(){ })()                      // 12 - именованная
```

Ссылки: 

- https://learn.javascript.ru/function-basics
- https://learn.javascript.ru/function-expressions 

