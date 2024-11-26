1. (по п.п.2 бормотание) Изменен комментарий для ```if (startUse == null) {```   
c //передан null - текущее время. Получаем серверную дату/время, т.к. оапи-функция получения наборов правил не умеет работать с startUse = null  
на //Функция получения набора правил не преобразует null в текущую дату (как остальные наши oapi-функции). Получаем текущую дату (серверную) самостоятельно 
2. (по п.п.3 недостоверный комментарий) Найден и удален устаревший комментарий:  
   // используется только в одном месте при логировании. Бесполезный атрибут.  
   String getOrderId();  
Данный метод (ну и атрибут) используется в классе обработки результатов выполнения заказа
3. (п.п. 4 Шум) Удален очевидный комментарий ("действия по приему колбэка"):  
   if (needCheckCallback) {  
     // действия по приему колбэка
4. (п.п.1 Неочевидный комментарий) Было (вообще загадочный комментарий для меня, т.к. метод вызывает оапи-функцию проверки возможности отключения услуги):
```groovy
/**
* Проверяем, что услуга не легкая
*/
private void checkServiceId(def serviceId, Locale locale) {
```
Стало:
```groovy
/**
 * Проверяем возможность отключения услуги
 */
private void checkServiceDeactivatePossibility(def serviceId, Locale locale) {
```
5. (п.п.8 Слишком много информации) Было:
```java
//для временного восстановления функционирования раннера, здесь пустые параметры из операции
// подменяются на {}.
//В целевой картине нужно уходить от необоснованного расхода БД-ресурсов (тут вопрос больше не к конректному кейсу с
// пустыми параметрами, а вообще к хранению смерженных параметров в БД. Они находятся в топе по
// занимаемому месту среди всех объектов БД BULK, при этом не содержат ничего эксклюзивного).
```
Размышления перенес в таску в Jira. Комментарий в коде:
```java
//Для последующей корректной обработки параметров заменяем значение null на "{}" 
```
6. (п.п.9 Нелокальная информация) Был комментарий:
```java
//Через основной датасорс достается отдельная сессия.
// Таким образом можно фиксировать, что настройка maxActive у бэка должна быть не менее 2.
// И batchSize можно увеличивать, хотя бы до 10-20К.
try (Connection conn = dataSource.getConnection();
    PreparedStatement ps = conn.prepareStatement(INSERT_REQUEST_ITEM)) {
```
Совет перенес в таску на проверку и документирование. Оставил:
```java
//Через основной dataSource достается отдельная сессия непосредственно для пакетной вставки запросов на выполнение.
try (Connection conn = dataSource.getConnection();
    PreparedStatement ps = conn.prepareStatement(INSERT_REQUEST_ITEM)) {
```
7. (п.п.10 Обязательные комментарии) Убрал комментарии из кода ниже:
```java
/* Тип источника */
private String sourceType;
/* Дата запуска */
private Date startDate;
/* Дата завершения */
private Date endDate;
/* Ид инстанса раннера */
private Long runnerInstanceId;
```
8. (п.п.4 Шум) Убраны очевидные комментарии:
```groovy
//Создаем результат выполнения скрипта
def scenarioStepResult = new SimpleScenarioStepResult()
//Получаем идентификатор запуска (нужен только для логирования)
String scenarioRequestId = scenarioRequest == null ? "???" : scenarioRequest.getId()
//Логируем идентификатор запуска и сколько в нем запросов
LOGGER.debug(labelService.msg("SubscriberRatingOptionsDeactivate.execute.start",
        scenarioRequestId, scenarioRequest == null ? null : scenarioRequest.requests.size()))
```
9. (п.п.3 Недостоверные комментарии) Изначальный комментарий:
```groovy
/**
* Заполняем тело запроса и проверяем обязательные параметры
*/
    private static String buildBody(RunData runData, Map<String, Object> extraParams) {
```
Проверка давно убрана в отдельный метод и запускается гораздо раньше данного метода. Так что решил вообще убрать этот комментарий, т.к. по названию метода ясна его цель.  
10. (п.п.9, а может 1) Удалил нижепреведенный комментарий, т.к. он не относится ни к выполняемому действию, ни даже, к решаемой в данном куске кода задаче. Просто общая мысль по возможному действию поддержки в дальнейшем...  
```java
//во всех непонятных ситуациях анализ будет по SUBSCRIBER
return this.runDataMapper.getDuplicated(bulkRequestId, customerModeFlag, fetchCount);
```
11. (п.п.10 Обязательные комментарии) Удален комментарий, приведенный ниже:
```groovy
    /**
     * Заказываем отчет
     */
    private Long orderReport(Long subscriberId,Map mainParams, Map allParams, Locale locale, String replyTo, String correlationId) {
```
12. (п.п.11 Закомментированный код) Удален закомментированный код
```java
/*    private boolean rowMustBeLowerCase(final String rowValue) {
        Matcher matcher = isTimePattern.matcher(rowValue);
        return !matcher.find();
    }
*/
```
13. (п.п.12) Заменил комментарий на нормально названную константу:  
было: 
```java
/* OAPI delay in milliseconds */
private static final long DELAY = 3000;
```
Стало: ```private static final long OAPI_REQUEST_DELAY_MS = 3000;```  
14. (п.п.11 Закомментированный код) Нашел даже такой интересный вариант (ну и удалил)
```java
if (//instance != null &&
    psqlIgnorableCodesPattern != null &&
    psqlIgnorableCodesPattern.matcher(sqlState).find()) {
    return true;
}
```
Вообще закомментированного кода достаточно много - удалил гораздо больше, описанного здесь  
15. (п.п.4) Убран очевидный комментарий:  
// таймаут ожидания ответа.  
int responseTimeoutMillis = getResponseTimeoutMillis();