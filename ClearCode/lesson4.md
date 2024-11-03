1. delay - retryDelayMs  
   // задержка перед переповтором сценария (в миллисекундах)
2. duration - cacheExpireTimeSec  
  // время "протухания" значение кеша в секундах  
Вообще много переменных для хранения каких-то промежутков времени без указания размерности. Постоянно приходится уточнять, в чем же там храниться время. Почему-то до этого курса не приходило в голову просто добавить суффикс размерности. Хотя ведь неоднократно в библиотеках наблюдал правильные имена подобных переменных...
3. maxBatchSize - requestsCountMax  
// максимальный размер пачки запросов
4. avgExecTime - dbExecTimeMsAvg  
// среднее время выполнения запросов к БД  
5. totalTime - execTimeMsTotal  
// общее время выполнения в мс  
Подобных тоже много переменных, т.к. max, min, avg, sum - в основном идут префиксом к переменным, а не суффиксом
6. Было:
```java
HeaderElement he = it.nextElement();
String param = he.getName();
String value = he.getValue();
```
// разбор параметров в заголовках
```java
HeaderElement headerElement = it.nextElement();
String paramName = headerElement.getName();
String paramValue = headerElement.getValue();
```
7. String messageStr - message  
// текст сообщения
8. пара переменных с очень похожими именами в одном методе: 
resObj и respObj - foundSubscribers и searchResponse  
// результат работы метода (найденные абоненты), ответ запроса поиска 
9. start - updateStartTime, end - updateEndTime  
// время начала/окончания апдейта сущности
10. runTime - requestExecTimeMs  
// время выполнения запроса в мс
11. String pathStr - pathToZkNode  
// путь к узлу zookeeper
12. Long val - customerIdFromContext  
// значение идентификатора клиента из контекста операции