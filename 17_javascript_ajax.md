## AJAX jquery

__Примеры ajax запросов__

```javascript
$(document).ready(function() {

    //////////////////////////////////////////////////////
    /// Простая загрузка содержимого по URL 
    //////////////////////////////////////////////////////
	
    // 1) Метод load() - GET-запрос 
	
	// Загружаем в блок id=result данные, полученнные по адресу URL 
    $("#result").load("/ajax/handler.php");
    
    // Загрузка данных с фильтрацией: загружаем данные, 
    // потом берем из них блок id=from_block, и из этого блока 
    // загружаем данные в блок id=result 
    $("#result").load("/ajax/handler.php #from_block");

    // 2) Методы get(), post()
    
    $.get("/ajax/handler.php", function(resultHTML) {
        $("#result").html(resultHTML);
    });

    $.post("/ajax/handler.php", function(resultHTML) {
        $("#result").html(resultHTML);
    });

    //////////////////////////////////////////////////////
    /// Загрузка страницы с отправкой данных на сервер 
    //////////////////////////////////////////////////////

    // Простая передача данных на сервер и загрузка содержимого 
    // Считали атрибут кнопки, отправили запрос на сервер 
    
    var id = $(this).attr("data-id"); 
    var dataToServer = {"id": id}; // Объект ЗАПРОСА формате JSON 
    
    // На сервере можем перехватить данные с помощью $_REQUEST["id"],  
    // и вернуть результат в зависмости от id 
    
    // 3) Метод load()
    
    // GET-запрос, данные передаем в СТРОКЕ запроса 
    $("#result").load("/ajax/handler.php?id="+id);  
    
    // GET-запрос, данные передаем в ОБЪЕКТЕ запроса 
    $("#result").load("/ajax/handler.php", dataToServer); // GET 
    
    // 4) Методы get(), post()  
    
    // Данные в СТРОКЕ запроса 
    $.get("/ajax/handler.php?id="+id, function(resultHTML){  
    	$("#result").html(resultHTML);
    });
    $.post("/ajax/handler.php?id="+id, function(resultHTML){  
     	$("#result").html(resultHTML);
    });
    
    // Данные в ОБЪЕКТЕ запроса 
    $.get("/ajax/handler.php", dataToServer, function(resultHTML) {
        $("#result").html(resultHTML);
    });
    $.post("/ajax/handler.php", dataToServer, function(resultHTML) {
        $("#result").html(resultHTML);
    });

    // 5) Отправка данных с обработкой ошибок
    
    // Аналогично можно передавать данные методом "get",
    // как в СТРОКЕ, так и в ОЪЕКТЕ запроса 
    $.post("/ajax/handler.php", dataToServer).done(function(resultHTML) {
        $("#result").html(resultHTML);
    }).fail(function() {
        $("#result").html('<p style="color:red;">Возникла ошибка!</p>');
    }); 

    //////////////////////////////////////////////////////
    /// ПОСТРОЕНИЕ УНИВЕРСАЛЬНЫХ AJAX ЗАПРОСОВ  
    //////////////////////////////////////////////////////  

    // 6) Метод ajax() - get(), post(), load() - обертки этого метода
    
    $.ajax({
    	
        method: "POST", // или GET 
        url: "/ajax/handler.php", 
        data: dataToServer, // Данные, отправляемые на сервер, 
        dataType: "html",   // xml, json, script (тип получаемых от сервера данных) 
        beforeSend: beforeSendAJAX,  
        
        // success: function(data, textStatus){}, // Аналог .done() 
        // error: function(){},                   // Aналог .fail()
        // complete: function(){},                // Аналог .alwais()
        
    }).done( successResult ).fail( failResult ).always( alwaysResult ); 
    
    // Метод, выполняется перед началом AJAX запроса 
    // - может отобразить прелоадер загрузки данных 
    function beforeSendAJAX(){ 
    	console.log("before send ajax"); 
    }
    
    // Данные пришли с сервера (успешно) 
    function successResult(resultHTML) {
        $("#result").html(resultHTML); 
    }

    // Обработчик Ошибок 
    function failResult(){
        $("#result").html('<p style="color:red;">Возникла ошибка!</p>');
    } 

    // Обработчик выполняется ВСЕГДА (при успехе и при ошибках, не обязательный)  
    // - обычно используют, чтобы скрыть блок с прелоадером 
    function alwaysResult() {
        console.log("AJAX запрос к серверу завершен!");  
    }

});

```

__Ссылки:__

- https://api.jquery.com/load/
- https://api.jquery.com/jquery.get/
- https://api.jquery.com/jQuery.post/
- https://api.jquery.com/jquery.ajax/



