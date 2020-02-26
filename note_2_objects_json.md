### Объекты в Javascript 
<hr>

Объекты используются для хранения коллекций различных значений и более сложных сущностей 
(в том числе функций и других сложных объектов)

__Создание объектов:__

```javascript 
let user = new Object(); // синтаксис "конструктор объекта" 
let user = {};           // синтаксис "литерал объекта" (предпочтительный способ) 
```

__Свойства объектов в литеральном синтаксисе__

```javascript 

// Объявление объекта 
let user = {
  name: "John",  // под ключом "name" хранится значение "John"
  age: 30        // под ключом "age" хранится значение 30
};

// Чтение свойств - Вариант 1 
alert( user.name ); // John
alert( user.age ); // 30

// Чтение свойств - Вариант 2 
alert( user[name] ); // John
alert( user["age"] ); // 30 

// Добавление нового свойства
user.isAdmin = true; 

// Удаление свойств 
delete user.age;

// Изменение существующего свойства 
user.age = 31;  // или так user["age"] = 31; 

```

__Проверка существования свойства, оператор «in»__

```javascript 

// C помощью оператора сравнения: 
let user = {}; 
alert( user.noSuchProperty === undefined ); // true означает "свойства нет"

// С использованием оператора "in": 
let user = { name: "John", age: 30 }; 
alert( "age" in user );    // true, user.age существует
alert( "blabla" in user ); // false, user.blabla не существует

```

__Цикл по свойствам объектов__

```javascript 
let user = {
  name: "John",
  age: 30,
  isAdmin: true
};

for (let key in user) {
  // ключи
  alert( key );  // name, age, isAdmin
  // значения ключей
  alert( user[key] ); // John, 30, true
}
```

### Преобразование объектов в примитивы 
<hr>

__Методы toString/valueOf__

Данные методы можно описать в объектах, они будут выполнены при преобразовании объектов. 

toString - преобразование к строке 
valueOf  - преобразование к числу 

```javascript
let user = {

  name: "John",
  money: 1000,

  // для хинта равного "string"
  toString() {
    return `{name: "${this.name}"}`;
  },

  // для хинта равного "number" или "default"
  valueOf() {
    return this.money;
  }

};

alert(user);       // toString -> {name: "John"}
alert(+user);      // valueOf -> 1000
alert(user + 500); // valueOf -> 1500 
```

### Формат JSON, метод toJSON  
<hr>

JSON - формат для передачи данных между клиентом и сервером, используемым в различных языках. 
По сути JSON формат - это объект в литеральной нотации в строковой форме, удобной для передачи или хранения.

__!Важно:__

- JSON не поддерживает комментарии. Добавление комментария в JSON делает его недействительным.
- JSON не следует путать со спецификацией JSON5: https://json5.org/  

JavaScript предоставляет методы для работы с JSON 

__Популярные методы Javascript:__

JSON.stringify - для преобразования объектов в JSON.
JSON.parse     - для преобразования JSON обратно в объект.

```javascript 

// Берем объект 

let student = {
  name: 'John',
  age: 30,
  isAdmin: false,
  courses: ['html', 'css', 'js'],
  wife: null
};

// Преобразуем в JSON формат

let json = JSON.stringify(student);   
console.log(typeof json);  // Проверим тип - String (строка) 

// Посмотрим на JSON объект (это строка) 
 
console.log(json); 

/* 
{
  "name": "John",
  "age": 30,
  "isAdmin": false,
  "courses": ["html", "css", "js"],
  "wife": null
}
*/ 
```

Объект в формате JSON имеет несколько важных отличий от объектного литерала:

- Строки используют двойные кавычки. Никаких одинарных кавычек или обратных кавычек в JSON. Так 'John' становится "John".
- Имена свойств объекта также заключаются в двойные кавычки. Это обязательно. Так age:30 становится "age":30.

__Пользовательский метод «toJSON»__

Может использоваться при преобразовании объекта в JSON формат

```javascript 

let room = {
  number: 23,
  toJSON() {
    return this.number;
  }
};

let meetup = {
  title: "Conference",
  room
};

// При преобразовании в объект JSON, выполнился описанный в объекте метод toJSON() 
alert( JSON.stringify(room) ); // 23
```


### JSON.parse - для преобразования объекта JSON в объект Javascript 
<hr> 

Базовый синтаксиc: 

```javascript  
let value = JSON.parse(strJSON, [reviver]); 
// reviver - функция, которая будет вызываться для каждой пары (ключ, значение) и может преобразовывать значение. 
```

Пример 1: 

```javascript
let user = '{ "name": "John", "age": 35, "isAdmin": false, "friends": [0,1,2,3] }';
user = JSON.parse(user);
alert( user.friends[1] ); // 1
```

Пример 2 (с использованием сложных вложенных объектов и функции reviver) 

```javascript

let schedule = `{
  "meetups": [
    {"title":"Conference","date":"2017-11-30T12:00:00.000Z"},
    {"title":"Birthday","date":"2017-04-18T12:00:00.000Z"}
  ]
}`;

schedule = JSON.parse(schedule, function(key, value) {
  if (key == 'date') return new Date(value);
  return value;
});

alert( schedule.meetups[1].date.getDate() ); // 18 - И это работает! 

```

### Eval: выполнение строки кода 
<hr>

Использовать не рекомендуется, но знать нужно! 

```javascript 
// Выполнение строки кода, находящейся в переменной 
let code = 'alert("Привет")';
eval(code); // Привет 
```

Ссылки: 

- https://learn.javascript.ru/object - Основная инфа по объектам в JS 
- https://learn.javascript.ru/object-conversion - Преобразование объектов (для сведения) 
- https://learn.javascript.ru/object-toprimitive - Преобразование объектов (новый материал) 
- https://learn.javascript.ru/types-conversion - Преобразование типов (для простых типов данных) 
- https://learn.javascript.ru/json - Преобразование в объект JSON (
- https://learn.javascript.ru/eval - Использование Eval 


