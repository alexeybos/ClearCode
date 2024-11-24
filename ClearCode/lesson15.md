**по п. 1 (Информативные комментарии)**
1. Добавлен информативный комментарий
```java
//Если для отчета объявлен кастомный билдер, дополняем данные по алгоритму его метода data()
if (newInstanceReportBuilder != null) {
    List<List<String>> rows = newInstanceReportBuilder.data(row, item.getOutParameters());
    rows.forEach(_row -> excelUtils.writeExcelRow(sheet, _row));
} else {
    excelUtils.writeExcelRow(sheet, row, i + 1);
}
```
2. Добавлен информативный комментарий
```groovy
//объявление загрузчика списка IP-адресов для использования в кэше ресурсов
def loader = { pageIndex ->
    String url = "/openapi/accessPoints/${accessPointId}/IPAddresses"
    Map headers = CommonUtils.getBaseHeaders()
    //... тут длинный код получения списка IP-адресов 
    def ipAddressList = list.stream().map({ item ->
        //... еще код
        return Pair.of(ipAddress, ipAddressId)
    }).collect(Collectors.toList())
    return ipAddressList
}
Pair result = cacheService.getFromCollection(cacheKey, loader)
```
**по п. 2 (Представление намерений)**
1. Добавлен комментарий
```java
//эмулируем долгое ожидание коллбэка - больше чем интервал ожидания
operation.addDefaultParameter("generateCallbackMessage", "mock");
operation.addDefaultParameter("delayPublishCallback", delay * 1000);
```
2. Добавлен комментарий
```java
//для 1-ого абонента укажем, что он завершит выполнения скрипта с ошибкой
InputItemRequest firstSubscriber = new InputItemRequest(subscribers.get(0).getId());
firstSubscriber.addIndividualParameters("appException", "true");
inputItems.add(firstSubscriber);
//для 2-го абонента сделаем успешное завершение
inputItems.add(new InputItemRequest(subscribers.get(1).getId()));
```
**по п. 3 (Прояснение)**
1. Изменен комментарий
```java
//инициализация библиотеки tika для работы с zip файлами
TikaConfig tikaConfig = null;
try {
    tikaConfig = new TikaConfig();
} catch (TikaException | IOException e) {
    logger.warn("tika config not created due to exception");
    logger.warn(e.getMessage(), e);
}
```
2. Добавлен комментарий
```java
//интерцептор mybatis-запроса для сбора статистики по времени выполнения
Instant start = Instant.now();
//выполнение самого mybatis-запроса
Object returnObject = invocation.proceed();
Instant end = Instant.now();
//...код
return returnObject;
```
**по п. 4 (Предупреждения о последствиях)**
1. Изменен комментарий
```java
sheet.trackAllColumnsForAutoSizing();
excelUtils.writeExcelHeader(sheet, excelHeaders);
for (int i = 0; i < excelHeaders.size(); i++) {
    sheet.autoSizeColumn(i);
}
//не рекомендуется оставлять AutoSizing, т.к. отчет будет формироваться дольше выставленного в системе таймаута (на него мы повличть не можем)
sheet.untrackAllColumnsForAutoSizing();
```
2. Добавлен комментарий
```java
//Данные тесты добавлять только в ночной прогон, т.к. они выполняются от 20 минут (ожидание тайм-аута + ожидание переповторов)
```
**по п. 5 (Усиление)**
1. Поправил комментарий:
```java
//коммит важен для алгоритма проверки дубликатов, т.к. он работает итерационно перечитывая данные одинаковым селектом.
conn.commit();
```
**по п. 6 (Комментарии TODO)**
1. Добавлен комментарий (это параметр размера пачки данных загрузки для формирования данных выполнения)
```java
@Value("${" + PS_CONFIG_RUNNER_COMMON + "/loadDataReadSize}")
private int loadDataReadSize; //TODO добавить параметр в применяемые без перезагрузки инстанса раннера 
```
2. Добавлен комментарий:
```groovy
//TODO Данный метод с минимальными отличиями (на первый взгляд даже не функциональными) используется еще в одном сценарии (PersonalDataUpdate)
//необходимо рассмотреть возможность выноса в утилитный класс
private String getAddressId(Long subscriberId, String addressString, Locale locale) {
```
3. Добавлен комментарий для предыдущего случая (в обоих groovy-сценариях, где используется метод getAddressId):  
//TODO добавить unit-тесты на работу метода getAddressId, т.к. планируется его рефакторинг

