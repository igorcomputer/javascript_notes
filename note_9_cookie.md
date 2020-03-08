## Использование Cookie в Javascript
<hr> 

Куки – это небольшие строки данных, которые хранятся непосредственно в браузере.

Куки обычно устанавливаются веб-сервером при помощи заголовка Set-Cookie. 
Затем браузер будет автоматически добавлять их в (почти) каждый запрос на тот же домен при помощи заголовка Cookie.

Если скрипт устанавливает куки, то нет разницы откуда был загружен скрипт – куки принадлежит домену текущей веб-страницы.

__document.cookie__ - в этом свойстве javascript хранятся текущие cookie в зависимости от домена.


__Примечание:__ 

__httpOnly__ - настройка веб-сервера, она запрещает любой доступ к куки из JavaScript. 

Мы не можем видеть такое куки или манипулировать им с помощью __document.cookie__.

```javascript
/////////////////////////////////////////////// 
// Чтение и вывод Cookie в консоль 
///////////////////////////////////////////////

console.log(document.cookie) // cookie1=value1; cookie2=value2;...

///////////////////////////////////////////////
// Запись Cookie 
///////////////////////////////////////////////

document.cookie = "user=John"; // обновляем только куки с именем 'user'

///////////////////////////////////////////////
// Запись Cookie с использование форматирования
// для кодировки спец символов
///////////////////////////////////////////////

// специальные символы (пробелы), требуется кодирование
let name = "my name";
let value = "John Smith"

// кодирует в my%20name=John%20Smith
document.cookie = encodeURIComponent(name) + '=' + encodeURIComponent(value);

alert(document.cookie); // ...; my%20name=John%20Smith
```

__Ограничения__:

- После encodeURIComponent пара name=value не должна занимать более 4Кб. Таким образом, мы не можем хранить в куки большие данные.

- Общее количество куки на один домен ограничивается примерно 20+. Точное ограничение зависит от конкретного браузера.

### Список настроек Cookie

__path=/mypath__ - Ограничивает доступность куков на определенных страницах сайта.

__path=/__ - будет доступно на всех страницах сайта

__domain=site.com__ - Домен, на котором доступны наши куки. 

- По умолчанию куки доступно лишь тому домену, который его установил, т.е. куки, которые были установлены сайтом __site.com__, не будут доступны на сайте __other.com__.

- Нет способа сделать куки доступным на другом домене 2-го уровня, так что other.com никогда не получит куки, установленное сайтом site.com.

Опция __domain__ позволяет разрешить доступ к куки для поддоменов.

```javascript
// находясь на странице site.com
// сделаем куки доступным для всех поддоменов *.site.com:
document.cookie = "user=John; domain=site.com"

// позже

// на forum.site.com
alert(document.cookie); // есть куки user=John
alert(document.cookie); // ...; my%20name=John%20Smith
```

__expires, max-age__ - дата истечения строка действия куки, когда браузер удалит его автоматически

Если этот значение не установлено, куки называются сессионными и действуют пока пользователь на закрыл страницу сайта
(т.е. время жизни таких куков равно сессии пользователя)

```javascript
///////////////////////////////////////////////////////////
// Пример установки expires
///////////////////////////////////////////////////////////

expires=Tue, 19 Jan 2038 03:14:07 GMT

///////////////////////////////////////////////////////////
// +1 день от текущей даты
///////////////////////////////////////////////////////////

let date = new Date(Date.now() + 86400e3);
date = date.toUTCString();
document.cookie = "user=John; expires=" + date;

```

__Если установить в expires прошедшую дату, то куки будет удалено!__

__max-age=3600__ - Альтернатива __expires__ 

- Определяет срок действия куки в секундах с текущего момента

- Для удаления нужно задать 0 или отрицательное значение

```javascript
// куки будет удалено через 1 час
document.cookie = "user=John; max-age=3600";

// удалим куки (срок действия истекает прямо сейчас)
document.cookie = "user=John; max-age=0";
```

__secure__ - флаг, разрешающий передачу куки только для https соединения

```javascript
// предполагается, что сейчас мы на https://
// установим опцию secure для куки (куки доступно только через HTTPS)
document.cookie = "user=John; secure";
```

__Настройка samesite__ - Параметр для защиты от межсайтовых атак Cross-Site Request Forgery (XSRF) 

Данную настройку следует изучить дополнительно. 

### Полезные функции для работы с куки


```javascript
////////////////////////////////////////////////////////////
// Получение куки с указанным именем (name)
// или undefined, если ничего не найдено
////////////////////////////////////////////////////////////

function getCookie(name) {

  // new RegExp генерируется динамически, чтобы находить ; name=<value>. 
  
  let matches = document.cookie.match(new RegExp(
    "(?:^|; )" + name.replace(/([\.$?*|{}\(\)\[\]\\\/\+^])/g, '\\$1') + "=([^;]*)"
  ));
  return matches ? decodeURIComponent(matches[1]) : undefined;
}


////////////////////////////////////////////////////////////
// Установка значения куки 
////////////////////////////////////////////////////////////

function setCookie(name, value, options = {}) {

  options = {
    path: '/',
    // при необходимости добавьте другие значения по умолчанию
    ...options
  };

  if (options.expires instanceof Date) {
    options.expires = options.expires.toUTCString();
  }

  let updatedCookie = encodeURIComponent(name) + "=" + encodeURIComponent(value);

  for (let optionKey in options) {
    updatedCookie += "; " + optionKey;
    let optionValue = options[optionKey];
    if (optionValue !== true) {
      updatedCookie += "=" + optionValue;
    }
  }

  document.cookie = updatedCookie;
}

// Пример использования:
setCookie('user', 'John', {secure: true, 'max-age': 3600});

////////////////////////////////////////////////////////////
// Удаление куки 
////////////////////////////////////////////////////////////

function deleteCookie(name) {
  setCookie(name, "", {
    'max-age': -1
  })
}
```

### jQuery плагин для работы с куки

Есть много плагинов для упрощения работы с куки (см. список ссылок ниже) 

Пример кода с использованием плагина __jquery-cookie__ (см. список ссылок ниже)

```javascript
////////////////////////////////////////////////////////////
// Установка куки 
////////////////////////////////////////////////////////////

$.cookie('name', 'value');

$.cookie('name', 'value', { expires: 7 });

$.cookie('name', 'value', { expires: 7, path: '/' }); 

////////////////////////////////////////////////////////////
// Чтение куки 
////////////////////////////////////////////////////////////

$.cookie('name'); // => "value"

$.cookie(); // => { "name": "value" } // Чтение всех доступных значений 

////////////////////////////////////////////////////////////
// Удаление куки 
////////////////////////////////////////////////////////////

$.removeCookie('name'); // => true

// Если в куке был прописан path, способ удаления, указанный строчкой выше не сработает 
$.removeCookie('name', { path: '/' }); // => true  

```

__Примечание:__ Операции обновления или удаления куки должны использовать те же путь и домен

### Сторонние куки

Куки называются __сторонними__, если они размещены __с домена, отличающегося от страницы, которую посещает пользователь.__

__Например:__

- Страница site.com загружает баннер с другого сайта: <img src="https://ads.com/banner.png">. Вместе с баннером удалённый сервер ads.com может установить заголовок Set-Cookie с куки. Такие куки создаются с домена ads.com и будут видны только на сайте ads.com

- Когда пользователь переходит с site.com на другой сайт other.com, на котором тоже есть баннер с сайта ads.com, то ads.com получит куки, так как они принадлежат ads.com, таким образом ads.com распознает пользователя и может отслеживать его перемещения между сайтами. 

__Некоторые современные браузеры используют специальные политики для таких куки:__

- Safari вообще не разрешает сторонние куки.

- У Firefox есть «чёрный список» сторонних доменов, чьи сторонние куки он блокирует.


__Ссылки:__

- https://learn.javascript.ru/cookie
- https://github.com/js-cookie/js-cookie    - javascript плагин 
- https://github.com/carhartl/jquery-cookie - jQuery Cookie 