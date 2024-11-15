1. Перенес объявление переменной ближе к месту использования  
было:  
```groovy
String url = "openapi/bills/$billId/penalties/add/check"
//код
Response response = Oapi.post(url, params)
```
стало:
```groovy
//код
String url = "openapi/bills/$billId/penalties/add/check"
Response response = Oapi.post(url, params)
```
2. Переменную из предыдущего примера сделал final
```groovy
final String url = "openapi/bills/$billId/penalties/add/check"
```
3. Заменил отдельное создание переменной с последующей инициализацией на создание+инициализацию в месте использования: 
```java
String currentUser;
//код
currentUser = input.headers.get(Constants.EXECUTE_USER_CAMEL_HEADER).toString();
//использование
```
стало:
```java
//код
String currentUser = input.headers.get(Constants.EXECUTE_USER_CAMEL_HEADER).toString();
//использование
```
4. Добавил инициализацию полей класса в конструкторе.  
было:
```java
public class ConsoleAuthProvider {
    private final String useLdap;
    private String ldapUrl;
    private String ldapManagerUser;
    private String ldapUserSearchBase;
    
    public ConsoleAuthProvider(String useLdap) {
        this.useLdap = useLdap;
    }
```
стало:
```java
public class ConsoleAuthProvider {
    private final String useLdap;
    private String ldapUrl;
    private String ldapManagerUser;
    private String ldapUserSearchBase;
    
    public ConsoleAuthProvider(String useLdap) {
        this.useLdap = useLdap;
        this.ldapManagerUser = null;
        this.ldapUrl = null;
        this.ldapUserSearchBase = null;
    }
```
5. Переименовал и перенес инициализацию переменной непосредственно к месту использования (наполнения):    
было: 
```java
List<BulkRequestInputItem> resultList = new ArrayList<BulkRequestInputItem>();
//код
for () {
    // resultList.add()
}
```
стало: 
```java
//код
List<BulkRequestInputItem> transformedInputItems = new ArrayList<BulkRequestInputItem>();
for () {
    // transformedInputItems.add()
}
```
6. Проинициализировал все поля класса в конструкторе:
```java
public class UtilsContext {
    public final LabelService labelService;
    public final String scenarioRequestId;
    public final String pstxid;
    public final String runForUser;

    public UtilsContext(LabelService labelService, String scenarioRequestId, String pstxid) {
        this.labelService = labelService;
        this.scenarioRequestId = scenarioRequestId;
        this.pstxid = pstxid;
        this.runForUser = null;
    }
```
7. После использования, "обнулил" переменную:  
Map<String, String> queryParameters = new HashMap();  
// использование  
   queryParameters = null;  
//продолжение кода  
8. После использования "завершил" работу с целочисленной переменной:  
int totalUpdatedCount = 0;  
//использование  
   totalUpdatedCount = -1;  
//продолжение кода
9. После использования "избавился" от значения строковой переменной (дополнительно переименовал переменную dateStr -> callbackDateStr):
```java
//String dateStr = null;
String callbackDateStr = null;
try {
  callbackDateStr = this.externalService.formatDate(
            DateFactory.getCurrentDate(), Locale.getDefault());
} catch (AppException e) {
    LOG.error(e.getMessage(), e);
    callbackDateStr = null;
}
//использование
callbackDateStr = "***NOT DEFINED DATE***";
//дальнейший код
```
10. Корректно оформил цикл со счетчиком (в нескольких местах). Пример одного из:  
было: 
```java
long i = 0;
for (String account: accounts) {
    FindItem findItem = new FindItem(i++, new CustomerInfo(null, null, account), null);
    items.add(findItem);
}
```
стало:
```java
for (int i = 0; i < accounts.size; i++) {
    FindItem findItem = new FindItem(i, new CustomerInfo(null, null, accounts.get(i)), null);
    items.add(findItem);
}
```
11. Заменил цикл while на стандартный со счетчиком (в нескольких местах). Пример одного из:  
было: 
```java
int i = 0;
while (i < columnsSize) {
        String header = headers.get(i);
        strings.add(getValue(i));
        //много кода
        i++;
}
```
стало:
```java

for (int i = 0; i < columnsSize; i++) {
        String header = headers.get(i);
        strings.add(getValue(i));
        //много кода
}
```
12. Перенес объявление и инициализацию переменной непосредственно к циклу, в котором она используется (в нескольких местах). Пример одного из:  
было:
```java
final Sheet sheet = workbook.createSheet();
//много кода
for (int i = 0; i < dataList.size(); i++) {
    //код
    excelUtils.writeExcelRow(sheet, row, i + 1);
}
```
стало:
```java
//много кода
final Sheet sheet = workbook.createSheet();
for (int i = 0; i < dataList.size(); i++) {
    //код
    excelUtils.writeExcelRow(sheet, row, i + 1);
}
```
13. Для случая из прошлого урока (где была добавлена проверка переменной на 0) добавил сообщение в лог о недопустимом значении:     
log.error("В настройке RunDataActorCount указано недопустимое значение {}", runDataActorCount)
14. Добавил инвариант в код метода работы с запуском. Идентификатор (runId) должен присутствовать:  
```if (runId = null) throw new BusinesException("Internal error. Run descriptor not passed to processRun method")```  
// данный инвариант добавлен к существующим в методе проверкам.
15. Добавил проверку в метод обработки контекста операции.  
if (operationContext = null) log.error("Контекст операции {} утерян", operationId);  
// добавлено по аналогии с реакцией в других методах работы с контекстом 

