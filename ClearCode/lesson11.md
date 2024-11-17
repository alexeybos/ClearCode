1. "Сжал" область видимости переменной, перенеся ее объявление+получение внутрь if, в котором она используется (только там):  
было: 
```java
ScenarioAsyncProcState firstProcState = matchedStates.get(0);
if (matchedStates.size() == 1) {
        //use firstProcState
}
```
стало:
```java
if (matchedStates.size() == 1) {
        ScenarioAsyncProcState firstProcState = matchedStates.get(0);
        //use firstProcState
}
```
2. "Сжал" область видимости переменной Set<String> operationTypeCodes. Также перенос внутрь if. За его пределами не используется.  
было:
```java
Set<String> operationTypeCodes = new HashSet<>();
//код
if (body != null) {
        //use operationTypeCodes
}
//код
```
стало:
```java
//код
if (body != null) {
        Set<String> operationTypeCodes = new HashSet<>();
        //use operationTypeCodes
}
//код
```
3. Переделал цикл forEach (внутри инкрементировался счетчик) на стандартный for. Переменная, ранее использующаяся в качестве счетчика стала внутренней переменной цикла. Обращений к ней за пределами цикла не было.  
было (переменная счетчика к тому же объявлялась за много строк до цикла, в котором использовалась): 
```java
int newIdIndex = 0;
//код
for (RunData item : batchPack) {
        ps.setLong(1, newIds.get(newIdIndex));
        newIdIndex++;
        //далее код
}
```
стало:
```java
//код
for (int i = 0; i < batchPack.size(); i++) {
        ps.setLong(1, newIds.get(i));
        RunData item = batchPack.get(i);
        //далее код
}
```
4. Аналогичное изменение (forEach->for) для следующего участка кода (счетчик становится внутренней переменной цикла):  
```java
int ridsIdx = 0;
for(FindObjectRequestMessage msg: requestList) {
        //код
    rids[ridsIdx] = msg.rid;
    ridsIdx++;
}
```
стало: 
```java
int ridsIdx = 0;
for(int i = 0; i < requestList.size(); i++) {
    FindObjectRequestMessage msg: requestList.get(i);    
    //код
    rids[i] = msg.rid;
}
```
5. "Сжал" область видимости поля класса в локальную переменную метода: использовалась в единственном методе класса (и по логике больше нигде не должна использоваться). Убрал из конструктора, добавил параметр в метод, где она используется.
6. Сменил модификатор у поля класса с public на private (к тому же у поля уже был и геттер и сеттер)  
//public String dataJSON; -> private String dataJSON;
7. Сменил модификатор у поля класса с public на private (к тому же у поля уже был и геттер и сеттер)  
//public String mapping; -> private String mapping;
8. "Сжал" область видимости переменной, перенеся ее внутрь if, в котором она используется (только в нем):
```java
List<OperationView> operationsView = operations.stream().map //и т.д.
//много кода кстати
if (!operations.isEmpty()) {
        //код
        //use operationsView
        //код
}
```
стало:
```java
//много кода
if (!operations.isEmpty()) {
        //код
        List<OperationView> operationsView = operations.stream().map //и т.д.
        //use operationsView
        //код
}
```
9. "Сузил" окно уязвимости переменной (distributeChargesRuleSetAdd) в groovy-сценарии, перенеся ее объявление и инициализацию непосредственно к месту ее использования:  
было:
```groovy
        LinkedHashMap distributeChargesRuleSetAdd = getDistributeChargesRuleSetAdd(subsAndCustomerLimitsUserObj)
        List<LinkedHashMap> resLimitsAdds = null
        LinkedHashMap commonLimit = ParamsUtil.get(subsAndCustomerLimitsUserObj, "commonLimit")
        if(commonLimit != null){
            resLimitsAdds = getDistributeChargesLimitAddList(commonLimit)
        }
        //еще много кода
        //использование distributeChargesRuleSetAdd
        return  [subscriberDistributeChargesRuleAdd: subscriberDistributeChargesRuleAdd,
                 distributeChargesRuleSetAdd: distributeChargesRuleSetAdd]
```
стало:
```groovy
        List<LinkedHashMap> resLimitsAdds = null
        LinkedHashMap commonLimit = ParamsUtil.get(subsAndCustomerLimitsUserObj, "commonLimit")
        if(commonLimit != null){
            resLimitsAdds = getDistributeChargesLimitAddList(commonLimit)
        }
        //еще много кода
        LinkedHashMap distributeChargesRuleSetAdd = getDistributeChargesRuleSetAdd(subsAndCustomerLimitsUserObj)
        //использование distributeChargesRuleSetAdd
        return  [subscriberDistributeChargesRuleAdd: subscriberDistributeChargesRuleAdd,
                 distributeChargesRuleSetAdd: distributeChargesRuleSetAdd]
```
10. Для предыдущего примера дополнительно вынес работу с переменной resLimitsAdds в отдельный метод  
стало:
```groovy
        //вся логика работы с бывшей resLimitsAdds теперь в getCustomerLimitsForAdd. Получаем только необходимый результат:
        List<Map<String, Long>> limitsForAdd = getCustomerLimitsForAdd(subsAndCustomerLimitsUserObj)                  
        //еще много кода, в том числе использование limitsForAdd
        LinkedHashMap distributeChargesRuleSetAdd = getDistributeChargesRuleSetAdd(subsAndCustomerLimitsUserObj)
        //использование distributeChargesRuleSetAdd
        return  [subscriberDistributeChargesRuleAdd: subscriberDistributeChargesRuleAdd,
                 distributeChargesRuleSetAdd: distributeChargesRuleSetAdd]
```
11. "Сузил" окно уязвимости переменной (propertyNames). Перенес объявление и инициализацию непосредственно к месту использования:  
было:
```groovy
List<String> propertyNames = new ArrayList()
//код
//заполнение:
for (Map item: items) {
    if("условие") {
        propertyNames.add(item.get("name"))
    }
}
//еще код
//использование
```
стало:
```groovy
//код
//еще код
List<String> propertyNames = new ArrayList()
//заполнение:
for (Map item: items) {
    if("условие") {
        propertyNames.add(item.get("name"))
    }
}
//использование

```
12. При формировании отчета в excel идет первоначальное заполнение строки заголовков, а потом заполнение остального тела отчета. Заголовки формируются из двух составляющих, обязательных и генерируемых из параметров задачи (requiredHeadersNames, parametersHeaders).
Процедуру формирования строки заголовков вынес в отдельный метод со всеми используемыми переменными. Т.е. "сузил" для них окно уязвимости.
Переменные parametersHeaders, requiredHeadersNames "ушли" в этот отдельный метод.
13. Перенес объявление и все этапы заполнения переменной resultList в конец метода. Все подготовительные действия перенес в начало метода. 
Ранее заполнение данной переменной "разбавляли" недоделанные промежуточные действия, хотя они не зависят от действий по заполнению resultList и спокойно выносятся в начало метода.
```java
List<BulkRequestInputItem> resultList = new ArrayList<BulkRequestInputItem>();
//много кода
//первичное заполнение resultList
//еще код
// окончательное заполнение resultList
```
стало:
```java
//много кода
List<BulkRequestInputItem> resultList = new ArrayList<BulkRequestInputItem>();
//первичное заполнение resultList
// окончательное заполнение resultList
```
14. В методе обработки callback вынес логику получения обобщенной информации по обрабатываемому продукту (productInfo) и по заказу (orderInfo) в отдельные методы (getProductInfo, getOrderInfo) соответственно убрав из основного метода работу с "временными" переменными productId, orderId и т.п.
15. "Сжал" область видимости переменной, перенеся ее объявление+получение внутрь if, в котором она используется (только там):  
было:
```java
PstxidLogManager pstxidLogManager = null;
if (msg instanceof RunDataHolder) {
        pstxidLogManager = new PstxidLogManager();
        //код
        pstxidLogManager.putPstxidToMdc(runData.getPstxId());
        //код
}
```
стало:
```java
if (msg instanceof RunDataHolder) {
        //код
        pstxidLogManager = new PstxidLogManager();
        pstxidLogManager.putPstxidToMdc(runData.getPstxId());
        //код
}
```