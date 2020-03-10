## Функции конструкторы, создание объектов через "new"

Функции-конструкторы являются обычными функциями. Но есть два соглашения:

- Имя функции-конструктора должно начинаться с большой буквы.
- Функция-конструктор должна вызываться при помощи оператора "new".

```javascript 
function User(name) {
  this.name = name;
  this.isAdmin = false;
}

let user = new User("Вася");

// Это тоже самое: 
let user = {
  name: "Вася",
  isAdmin: false
};

alert(user.name); // Вася
alert(user.isAdmin); // false
```

Когда функция вызывается как new User(...), происходит следующее:

- Создаётся новый пустой объект, и он присваивается this.
- Выполняется код функции. Обычно он модифицирует this, добавляет туда новые свойства.
- Возвращается значение this.

```javascript 
function User(name) {
  // this = {};  (неявно)

  // добавляет свойства к this
  this.name = name;
  this.isAdmin = false;

  // return this;  (неявно)
}
```

Вариант конструктора, который не может быть вызван дважды: 

```javascript
let user = new function() {
  this.name = "Вася";
  this.isAdmin = false;

  // ...другой код для создания пользователя
  // возможна любая сложная логика и выражения
  // локальные переменные и т. д.
};

// Объект создаётся и тут же вызывается. Таким образом, такой метод создания позволяет инкапсулировать код, 
// который создаёт отдельный объект, но без возможности его повторного использования.

```

Конструктор может вернуть любой нужный нам объект с помощью return

```javascript
function BigUser() {
  this.name = "Вася";
  return { name: "Godzilla" };  // <-- возвращает этот объект
}
alert( new BigUser().name );  // Godzilla, получили этот объект
```

### Создание методов в конструкторе

```javascript
function User(name) {
  this.name = name;

  this.sayHi = function() {
    alert( "Меня зовут: " + this.name );
  };
}

let vasya = new User("Вася");

// Это аналогично следующему коду: 
vasya = {
   name: "Вася",
   sayHi: function() { ... }
}

// Вызов метода 
vasya.sayHi(); // Меня зовут: Вася
```

### Классы в Javascript (Синтаксический сахар)

__Методы в классе не разделяются запятой!__

__Классы всегда используют use strict. Весь код внутри класса автоматически находится в строгом режиме.__


### Class Definition

```javascript
/////////////////////////////////////
// Общая форма классов
/////////////////////////////////////

class MyClass {
  prop = value; // свойство
  constructor(...) { // конструктор
    // ...
  }
  method(...) {} // метод
  get something(...) {} // геттер
  set something(...) {} // сеттер
  [Symbol.iterator]() {} // метод с вычисляемым именем (здесь - символом)
  // ...
}

/////////////////////////////////////
// Пример использования класса
/////////////////////////////////////

class User {
  constructor(name) {
    this.name = name;
  }
  sayHi() {
    alert(this.name);
  }
}

let user = new User("Иван");
user.sayHi();

/////////////////////////////////////
// перепишем класс User на чистых функциях
/////////////////////////////////////

// 1. Создаём функцию constructor
function User(name) {
  this.name = name;
}
// 2. Добавляем метод в прототип 
User.prototype.sayHi = function() {
  alert(this.name);
};

// Использование:
let user = new User("Иван");
user.sayHi();
```

### Class Expression

```javascript
let User = class {
  sayHi() {
    alert("Привет");
  }
};
```

### Фабрика классов (Паттерн) 

```javascript
function makeClass(phrase) {
  // объявляем класс и возвращаем его
  return class {
    sayHi() {
      alert(phrase);
    };
  };
}

// Создаём новый класс
let User = makeClass("Привет"); 
new User().sayHi(); // Привет

```

### Геттеры/сеттеры, другие сокращения

```javascript
class User {
  constructor(name) {
    // вызывает сеттер
    this.name = name;
  }
  get name() {
    return this._name;
  }
  set name(value) {
    if (value.length < 4) {
      alert("Имя слишком короткое.");
      return;
    }
    this._name = value;
  }
}
let user = new User("Иван");
alert(user.name); // Иван
user = new User(""); // Имя слишком короткое.

```

### Свойства классов

__Старым браузерам может понадобиться полифил!__

Свойства классов добавлены в язык недавно.

```javascript
class User {
  name = "Аноним";
  sayHi() {
    alert(`Привет, ${this.name}!`);
  }
}
new User().sayHi();
```

__Ссылки__

- https://learn.javascript.ru/constructor-new
- https://learn.javascript.ru/class
- https://learn.javascript.ru/prototype-inheritance


