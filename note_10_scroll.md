## Размеры окна, прокрутка (scroll)

Для работы с прокруткой нужно знать размеры окна, которые зависят от специфики браузеров.

Чтобы надёжно получить полную высоту документа, нам следует взять максимальное из этих свойств:

```javascript
let scrollHeight = Math.max(
  document.body.scrollHeight, document.documentElement.scrollHeight,
  document.body.offsetHeight, document.documentElement.offsetHeight,
  document.body.clientHeight, document.documentElement.clientHeight
);

alert('Полная высота документа с прокручиваемой частью: ' + scrollHeight);
```

### Получение текущей прокрутки

Обычные элементы хранят текущее состояние прокрутки в __elem.scrollLeft/scrollTop__.

Текущую прокрутку для всего документа можно прочитать из свойств __window.pageXOffset/pageYOffset__

__Данные свойства доступны только для чтения, изменить их нельзя__


### Прокрутка: scrollTo, scrollBy, scrollIntoView

__Для прокрутки страницы из JavaScript её DOM должен быть полностью построен!__

- Обычные элементы можно прокручивать, изменяя свойства __scrollTop/scrollLeft__

- Страницу целиком можно прокручивать используя свойства __document.documentElement.scrollTop/Left__ (кроме основанных на старом WebKit (Safari) - __document.body.scrollTop/Left__.

__Для прокрутки есть также и другие способы:__

- Метод __window.scrollBy(x,y)__ прокручивает страницу относительно её текущего положения. 

Например, scrollBy(0,10) прокручивает страницу на 10px вниз.

- Метод __window.scrollTo(pageX,pageY)__ прокручивает страницу на абсолютные координаты.  

То есть, чтобы левый-верхний угол видимой части страницы имел данные координаты относительно левого верхнего угла документа. Это всё равно, что поставить scrollLeft/scrollTop. Для прокрутки в самое начало мы можем использовать scrollTo(0,0).

### Метод scrollIntoView

Применяется для прокрутки текущего элемента (например id="elem") к верхней части экрана или к нижней части экрана

- __elem.scrollIntoView(true)__ - в верхнюю часть экрана

- __elem.scrollIntoView(true)__ - в нижнюю часть экрана

### Запрет прокрутки

- __document.body.style.overflow = "hidden"__ - Чтобы запретить прокрутку страницы.

- __document.body.style.overflow = ""__ - Разрешить прокрутку 



### Событие scroll
<hr> 

При прокрутке возникает событие __scroll__, на которое можно повесить различные обработчики

```javascript
window.addEventListener('scroll', function() {
  document.getElementById('elem').innerHTML = pageYOffset + 'px';
});
```

Можно установить значение прокрутки для текущей страницы равной определенному значению: 

```javascript
// Прокрутка в начало страницы 
window.scrollTo(pageXOffset, 0);
```

__Ссылки:__

- https://learn.javascript.ru/size-and-scroll-window
- https://learn.javascript.ru/onscroll
- https://api.jquery.com/scrolltop/
- https://api.jquery.com/scroll/



