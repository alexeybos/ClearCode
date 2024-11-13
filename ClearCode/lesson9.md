1. Было: Map originalConflicts = new HashMap();   
Стало: Map<String, String> originalConflicts = new HashMap<String, String>();  
// Устранил предупреждение компилятора "Raw use of parameterized class 'Map'"
2. Было: ```Map originalErrors; originalErrors.put(ScenarioConstant.OutParameters.ORIGINAL_ERROR, originConflictList);```  
Стало: ```Map<String, List<Map<String, String> originalErrors; originalErrors.put(ScenarioConstant.OutParameters.ORIGINAL_ERROR, originConflictList);```  
// Устранил предупреждение компилятора Unchecked call to 'put(K, V)' as a member of raw type 'java.util.Map'
3. ввел константу String SUBSCRIBER_OPER_CATEGORY вместо магической строки  
// было: if ("SUBSCRIBER".equals(operationCategory)) {
4. ввел константы String MEDIA_TYPE_ZIP = "application/zip"; и String MEDIA_TYPE_G_ZIP = "application/gzip";
вместо магических строк:
```java
if ("application/zip".equals(mediaType.toString())) {
    //...
} else if ("application/gzip".equals(mediaType.toString())){
    //...
}
```
5. Нашел "немагическую константу" - строковый шаблон private static final String IS_BLANK_FORMAT = "%s can't be blank.";  
// Т.к. использовалось далеко от объявления и однократно, заменил на прямое использование строки
6. Для выражения maxPageSize = msg.runDataList.size() / cfg.getRunDataActorCount();  
сделал предварительную проверку значения cfg.getRunDataActorCount() на 0 (такая вероятность есть, т.к. берется из конфигурации куда потенциально пользователь может ввести 0)
7. Заменил в условии if (!bulkRequest.isPresent() || (!bulkRequest.get().getNaviUser().equals(currentUser) && !allowSupervisor)) {  
оба условия на отдельные boolean переменные:
```java
boolean requestNotExists = !bulkRequest.isPresent();
boolean isRequestNotAllowedToView = !bulkRequest.get().getNaviUser().equals(currentUser) && !allowSupervisor;
if (requestNotExists || isRequestNotAllowedToView) {}
```
8. заменил в условии if (orders == null || orders.isEmpty()) && (result == null || result.isEmpty()))  
оба условия на отдельные boolean переменные:
```java
boolean noOrders = orders == null || orders.isEmpty();
boolean noResult = result == null || result.isEmpty();
if (noOrders && noResult)
```
9. Заменил "магические строки" на константы:  
   было: "vkontakte" и "facebook" //используются профили только этих соц сетей  
   стало: String SOCIAL_NETWORK_VK и String SOCIAL_NETWORK_FB
10. В выражении if сначала заменил составные условия на boolean переменные:  
было: 
```java
if ((StringUtils.endsWithIgnoreCase(fileName.toLowerCase(), ".xls")
        || StringUtils.endsWithIgnoreCase(fileName.toLowerCase(), ".xlsx"))
        && ("x-tika-ooxml".equals(mediaType.getSubtype())
        || "x-tika-msoffice".equals(mediaType.getSubtype())
        || "zip".equals(mediaType.getSubtype()))) {}
```
стало сначала:
```java
boolean isExcelFile = (StringUtils.endsWithIgnoreCase(fileName.toLowerCase(), ".xls")
        || StringUtils.endsWithIgnoreCase(fileName.toLowerCase(), ".xlsx"));
boolean isCompatibleMediaType = ("x-tika-ooxml".equals(mediaType.getSubtype())
        || "x-tika-msoffice".equals(mediaType.getSubtype())
        || "zip".equals(mediaType.getSubtype()));
if (isExcelFile && isCompatibleMediaType) {}
```
11. После этого в предыдущем куске кода заменил "магические строки" на константы (в дальнейшем используются в классе в нескольких местах):    
```String MEDIA_TYPE_TIKA_OOXML; String MEDIA_TYPE_TIKA_MSOFFICE; String MEDIA_TYPE_ZIP; ```
12. Было: Map parametersMap = objectMapper.readValue(parameters, HashMap.class);  
Стало: Map<String, Object> parametersMap = objectMapper.readValue(parameters, HashMap.class);  
// Устранил предупреждение компилятора "Raw use of parameterized class 'Map'"
Вообще, в процессе выполнения данного задания "подчистил" много таких мест.