## Популярные паттерны

__Паттерн модуль с помощью литерального объявления__

Паттерн модуль позволяет организовать код приложения и не засорять глобальное пространство переменными. 

```javascript
var myModule = {
 
  myProperty: "someValue",
 
  // объекты могут содержать в себе свойства и методы
  // задаём какой-либо конфиг нашему модулю
  myConfig: {
    useCaching: true,
    language: "en"
  },
 
  // создадим простенький метод
  saySomething: function () {
    console.log( "Where in the world is Paul Irish today?" );
  },
 
  // метод вызова текста в консоль, который зависит от нашего конфига
  reportMyConfig: function () {
    console.log( "Caching is: " + ( this.myConfig.useCaching ? "enabled" : "disabled") );
  },
 
  // метод переопределения свойств в конфиге
  updateMyConfig: function( newConfig ) {
 
    if ( typeof newConfig === "object" ) {
      this.myConfig = newConfig;
      console.log( this.myConfig.language );
    }
  }
};
 
// Выводится: Where in the world is Paul Irish today?
myModule.saySomething();
 
// Выводится: Caching is: enabled
myModule.reportMyConfig();
 
// Добавляем новое свойство в конфиг + обновляем текущее: fr
myModule.updateMyConfig({
  language: "fr",
  useCaching: false
});
 
// Выводится: Caching is: disabled
myModule.reportMyConfig();
```

__Паттер модуль на основе самовыполняющейся функции__

```javascript	
var myNamespace = (function () {
 
  var myPrivateVar, myPrivateMethod;
 
  // Приватная переменная - счётчик
  myPrivateVar = 0;
 
  // Приватная функцию - логирует аргумент
  myPrivateMethod = function( foo ) {
      console.log( foo );
  };
 
  return {
 
    // Публичная переменная
    myPublicVar: "foo",
 
    // Публичная функция, использующая приватные переменные
    myPublicFunction: function( bar ) {
 
      // Инкрементируем нашу приватную переменную
      myPrivateVar++;
 
      // Вызов приватного метода
      myPrivateMethod( bar );
 
    }
  };
 
})();
```

__Паттерн модуль освобождающий (Revealing Module Pattern)__ 

```javascript	
var myModule = (function() {
  'use strict';

  var _privateProperty = 'Hello World';
  var publicProperty = 'I am a public property';

  function _privateMethod() {
    console.log(_privateProperty);
  }

  function publicMethod() {
    _privateMethod();
  }

  return {
    publicMethod: publicMethod,
    publicProperty: publicProperty
  };
})();

myModule.publicMethod(); // outputs 'Hello World'
console.log(myModule.publicProperty); // outputs 'I am a public property'
console.log(myModule._privateProperty); // is undefined protected by the module closure
myModule._privateMethod(); // is TypeError protected by the module closure
```

__Плагин jQuery__ 

```javascript
;(function( $ ){
  $.fn.myPluginName = function() {
    // код плагина
  };
})(jQuery);
```

__Организация кода в виде функции__

Пример виджета

```javascript

// Объявление функции конструктора 
function Menu(options) {
  var elem = options.elem;

  elem.onmousedown = function() {
    return false;
  }

  elem.onclick = function(event) {
    if (event.target.closest('.title')) {
      toggle();
    }
  };

  function toggle() {
    elem.classList.toggle('open');
  }

  this.toggle = toggle;
}

// Использование 
var menu = new Menu(...);
menu.toggle();

```

__Ссылки:__ 

- https://learn.javascript.ru/closures-module
- https://frontand.ru/module-javascript-design-patterns-part-3/
- https://coryrylan.com/blog/javascript-module-pattern-basics
- https://learn.jquery.com/code-organization/concepts/
- https://habr.com/ru/sandbox/39442/
- https://docs.microsoft.com/en-us/previous-versions/msdn10/ff608209(v=msdn.10)
- https://learn.javascript.ru/widgets-structure
- https://scotch.io/bar-talk/4-javascript-design-patterns-you-should-know



