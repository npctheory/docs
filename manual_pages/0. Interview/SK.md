---
title: SK Вопросы
---
##### Evantual consistency что это?
##### DI vs IoC. Что это и как связаны с Dependency Container
##### Как работает WCF  
##### Что такое gRPC ?
##### Какие характеристики микросервисной архитектуры?  
##### IQueryable<T> наружу библиотеки, можно ли?
##### Чем ValueTask отличается от Task<T>?
##### Как прикрутить к готовому решению новое приложение?
##### IHttpClientFactory vs HttpClient
##### Паттерн API GateWay
##### Что такое event-sourcing?  
##### CQRS vs Event sourcing
##### Что такое Blazor?
##### Как ускорить Entity Framework?
##### Какие уровни OSINT и на каком уровне http?


##### CQRS vs SQL View

##### DDD vs Anemic
[Ссылка](https://www.youtube.com/watch?v=pFlvQ97nIGs&list=PLIB8be7sunXNsWzOg4GyAnw8BnezVE3Mq&index=49)

##### Что такое vertical slice architecture?
[Ссылка](https://www.youtube.com/watch?v=Ko7nvkrrA4A&list=PLIB8be7sunXNsWzOg4GyAnw8BnezVE3Mq&index=48)

##### Рефакторинг vs Новый функционал, чем отличаются?  
Исправление кода без изменения контракта и бизнес-логики  

##### XML-комментарии в коде это что? Summary?


##### Что такое SpecFlow?
Фреймворк для BDD  (Похоже на TDD)  
Генерирует тесты  


##### Чем DDD отличается от Clean Architecture?
Две разных книги  
DDD: Аггрегаты, Энтити, Сервисы, какие должны быть между ними зависимост
Clean Architecture: Как располагать код  

##### String.Empty vs ""
Одинаково  

##### OpenID vs SAML
[Ссылка](https://youtu.be/Ex8n5kh_l3g?t=109)  


##### CQRS как расшифровыывается?
Command query responsibility segregation  

##### Для чего MassTransit?
Надстройка на RabbitMQ  
Для сериализации и десериализации сообщений RabbitMQ  

##### Что такое lowering в C#?  
Писать минимально сахара вроде var или foreach    


##### Что такое CorreliationID/TraceID в микросервисе?  
id-запроса, по которому можно найти его в логах  


##### Какие конфиги есть в asp.net core?
appsettings.json и весь каскад Configuration
launchsettings.json 

##### Сколько способов загрузить данные из БД?
SELECT  
INCLUDE
Explicit - load collection, load reference  
lazy loading - Зло, не использовать. В EF 6 использовалась по умолчанию, но очень грузила сериализацияей и в EF 7 ее отключили.    




##### Когда использовать gRPC?  
В микросервисах, чтобы они между собой общались

##### Паттерны Orchestration и Choreography когда использовать?  
Choreography внутри микросервиса, Orchestration поверх.  


##### Зачем вручную выбрасывать ArgumentNullReference?
[Ссылка](https://youtube.com/watch?v=1n0oErce5ls&si=EnSIkaIECMiOmarE&t=398)  
Исторически в бэкенде активно кидали эксепшны когда была хоть какая-то ошибка.  
Сейчас пользователю уже не принято показывать текст эсепшна.  
ArgumentNullReference, MicroserviceNullReference для собственных эксепшнов, NullReference для системных.  


##### Если существует метод, который например возвращает имя файла filename и размер filesize в байтах, и потребовалось добавить возможность возвращать байты вместо названия, то какой способ реализации использовать?
Создать новый метод, который будет возвращать байты. Это Single Responsibility Principle. Это другая ответственность.  


##### Что такой Automapper и зачем он нужен?
Очень помогает для передачи данных из Model во ViewModel
Считается что при работе с DTO не всегда подходит



##### Поддерживает ли Impersonation в ASP.NET Core? ЧТо это?
Когда запросы отправляются на sql-сервер от имени какого-то пользователя
Не поддерживается


##### Чем отличается аутентификация от авторизации?
Сначала Аутенификация, потом Авторизация  
Аутентификация: Sign In  
Авторизация: Роли  

##### Что такое SOLID?  
##### SCP принцип  
Single repsponsibility principle  
Модуль должен иметь только одну причину для изменения  

##### Может ли интерфейс иметь реализацию? Реальные свойства, методы?  
Через extension-методы  
В C# 8

##### Чем отличается interface до и после C# 8.  
До: Только методы, свойства, индексаторы, события  
После:
1. default-члены,
2. private, protected
3. virtual absctract, static  

##### Какие уровни тестирования существуют?
Unit, Integration, System, на готовой системе(обычно то же что и функциональное)  

##### Guid vs Long для идентификатора, какой лучше?
Guid  

##### Как общаются между собой микросервисы? Все способы.
MQ (RabbitMQ...)
REST или gRPC-запросы. gRPC специально для этого сделан.
Смешанная



##### Является ли запрос приложения к БД бизнес-процессом?  
нет, потому что


##### Что такое архитектура?  
организация элементов программы и способов взаимодействия между ними

##### Что такое SignalR?
WebSocket, Long Polling, Server sent event, Forever Frame  

##### Версионность WEB API, какие есть способы?  
1. Роутинг - в тело запроса вставляется версия  
2. Еще бывает header  
3. 
##### Что такое валидация, какие виды?  
Клиентская, в форме  
Серверная ASPNET  + Валидация базы данных - foreign кеи  
Проверка бизнес правил  


