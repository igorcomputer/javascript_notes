## Javascript Promises

__Синтаксис создания промисов__

```javascript
var promise = new Promise(function(resolve, reject) {
  // Эта функция будет вызвана автоматически

  // В ней можно делать любые асинхронные операции,
  // А когда они завершатся — нужно вызвать одно из:
  // resolve(результат) при успешном выполнении
  // reject(ошибка) при ошибке
});

// Универсальный метод для навешивания обработчиков: 
promise.then(onFulfilled, onRejected)

// onFulfilled – функция, которая будет вызвана с результатом при resolve.
// onRejected – функция, которая будет вызвана с ошибкой при reject.
```

__Использование c Timeout__

```javascript
// Создание промиса 
function delay(milliseconds) {
  return function(result) {
    return new Promise(function(resolve, reject) {
      setTimeout(function() {
        resolve(result);
      }, milliseconds);
    });
  };
}

// Использование 
delay(1000)('hello').then(function(result) {
  console.log(result);
});

// Такую функцию можно включить после другого Promise 
somePromise.then(delay(1000)).then(function() {
  console.log('1 second after somePromise!');
}); 
```


__Использование с AJAX__

```javascript

// Объявление 
function ajax(options) {
  return new Promise(function(resolve, reject) {
    $.ajax(options).done(resolve).fail(reject);
  });
}

// Использование 
ajax({ url: '/' }).then(function(result) {
  console.log(result);
});

```

__Полезные примеры jQuery для асинхронных операций__

```javascript
//---------------------------------------------------
// Выполнение всех задач, а потом вызов метода 
//---------------------------------------------------
$.when($.get('/echo/html/'), $.post('/echo/json/')).done(function(args1, args2) { alert("загрузка завершена"); });

//---------------------------------------------------
// Простая обработка ошибок: один обработчик для всех операций
//---------------------------------------------------
$.ajax('http://echo.jsontest.com/id/1')
.then(function(){
    console.log('OK 1');
    return $.ajax('http://echo.jsontest.com/id/2');
}).then(function(){
    console.log('OK 2');
    return $.ajax('http://echo.jsontest_fail.com/id/3');
}).then(function(){
    console.log('OK 3');
    return $.ajax('http://echo.jsontest.com/id/4');
}).then(function(){
    console.log('OK 4');
}).fail(function(){
    console.log('error');
});

//---------------------------------------------------
// Остановка выполнения цепочки после обработки ошибки
//--------------------------------------------------- 
$.ajax('http://echo.jsontest.com/id/1')
.then(function(){
    console.log('OK 1');
    return $.ajax('http://echo.jsontest.com/id/2');
}).then(function(){
    console.log('OK 2');
    return $.ajax('http://echo.jsontest_fail.com/id/3');
}).fail(function(){
    console.log('error 1');
}).then(function(){
    console.log('OK 3');
    return $.ajax('http://echo.jsontest.com/id/4');
}).fail(function(){
    console.log('error 2');
}).then(function(){
    console.log('OK 4');
}).fail(function(){
    console.log('error 3');
});

//---------------------------------------------------
// Продолжение выполнения цепочки после обработки ошибки 
//---------------------------------------------------
$.ajax('http://echo.jsontest_fail.com/id/1')
.then(function(){
    console.log('OK 1');
    return $.ajax('http://echo.jsontest.com/id/2');
}, function(){
    console.log('error 1');
    return $.when();
}).then(function(){
    console.log('OK 2');
}, function(){
    console.log('error 2');
});

//---------------------------------------------------
// После обработки ошибки ни один последующий колбэк не вызывается
//---------------------------------------------------
$.ajax('http://echo.jsontest_fail.com/id/1')
.then(function(){
    console.log('OK 1');
    return $.ajax('http://echo.jsontest.com/id/2');
}, function(){
    console.log('error 1');
    return $.Deferred(); // возвращаем объект promise, который не является ни resolved ни rejected.
}).then(function(){
    console.log('OK 2');
}, function(){
    console.log('error 2');
}); 

```

__Promise.resolve() VS new Promise()__

```javascript

Promise.resolve(x); 

// Тоже самое, что и: 

new Promise(function(resolve){ resolve(x); }); 

// Пример использования (промис внутри другого промиса) 

const promiseObj = Promise.resolve('Batman');
const anotherPromiseObj = new Promise((resolve, reject) => {
    setTimeout(() => {
        promiseObj.then(value => console.log(value));
    }, 1000);
});anotherPromiseObj.then(values => { 
    console.log(values); 
});

// Return 'Batman' 

// Может использоваться для реализации кеширования с использованием промисов 

var someCachedValue; 

var getValue = function() {

    if (someCachedValue) {
        return Promise.resolve(someCachedValue);
    }

    return db.queryAsync().then(function(value) {
        someCachedValue = value;
        return value;
    });
};
```

__Ссылки:__ 

- https://learn.javascript.ru/promise
- https://learn.javascript.ru/promise-basics
- https://webdevblog.ru/rasprostranennye-oshibki-pri-ispolzovanie-promise-v-javascript/
- https://habr.com/ru/company/bankrot-pro/blog/230441/
- https://proglib.io/p/9-js-promise-advice/
- https://levelup.gitconnected.com/understanding-promise-resolve-c6eacf37da9e 
- http://bluebirdjs.com/docs/api/promise.resolve.html 




