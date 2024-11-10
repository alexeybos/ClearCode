**3.1**
1. Класс, описывающий выполняемую команду(сценарий) стороннего сервиса (CRB)
```java
public class CrbCommand implements Serializable {
    public static final String PROCESSING_TYPE_PARALLEL = "parallel";
    public static final String PROCESSING_TYPE_SEQUENTIAL = "sequential";

    private String externalId;
    private String processingType;
    private CrbOperation operation;

    //было public CrbCommand(CrbOperation operation) {
    private CrbCommand(CrbOperation operation) {
        this.externalId = UUID.randomUUID();
        this.operation = operation;
        this.processingType = PROCESSING_TYPE_PARALLEL;
    }
    //было public CrbCommand(CrbOperation operation, String processingType) {
    private CrbCommand(CrbOperation operation, String processingType) {
        this.externalId = UUID.randomUUID();
        this.operation = operation;
        this.processingType = processingType;
    }
    
    public static CrbCommand crbCommandByOperation(CrbOperation operation) {
        return new CrbCommand(operation);
    }
    
    public static CrbCommand crbCommandWithProcessingType(CrbOperation operation, String processingType) {
        return new CrbCommand(operation, processingType);
    }
    //...
}    
```
2. Класс, описывающий операцию стороннего сервиса (CRB) 
```java
public class CrbOperation implements Serializable {
    private String name;
    private List<CrbOperationParam> params;
    
    // было public CrbOperation(String name) {
    private CrbOperation(String name) {
        this.name = name;
    }
    
    // было public CrbOperation(String name, List<CrbOperationParam> params) {
    private CrbOperation(String name, List<CrbOperationParam> params) {
        this.name = name;
        this.params = params;
    }
    
    public static CrbOperation crbOperation(String name) {
        return new CrbOperation(name);
    }

    public static CrbOperation crbOperationWithParams(String name, List<CrbOperationParam> params) {
        return new CrbOperation(name, params);
    }
    //...
}
```
3. Класс, описывающий работу с заголовком отчета
```java
public class ReportHeader {
    private String name;
    private int index;
    
    // было public ReportHeader(String name) {
    private ReportHeader(String name) {
        this.name = name;
    }
    
    // было public ReportHeader(String name, int index) {
    private ReportHeader(String name, int index) {
        this.name = name;
        this.index = index;
    }
    
    public static ReportHeader reportHeaderByName(String name) {
        return new ReportHeader(name);
    }
    
    //заголовок зафиксированный на определенном индексе
    public static ReportHeader reportHeaderFixedByIndex(String name, int index) {
        return new ReportHeader(name, index);
    }
    //...
}
```

**3.2**
1. public interface ApacheOapiService - public class ApacheOapiServiceImpl implements ApacheOapiService {
// служба доступа к open API
2. public interface OperationResultsService - public class OperationResultsServiceImpl implements OperationResultsService {
// обработка запросов на получение результатов выполнения операции.
3. public interface AsyncDBLoader - public class AsyncDBLoaderImpl implements AsyncDBLoader<BulkLoadData> {
// подгрузка дополнительных данных из БД
4. public interface ScenarioStateService - public class ScenarioStateServiceImpl implements ScenarioStateService {
// вспомогательный класс для работы с состояниями сценариев
5. public interface DictionariesService - public class CachingDictionariesServiceImpl implements DictionariesService {
// класс для работы закешированными "статическими" справочниками