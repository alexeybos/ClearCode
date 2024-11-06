**1**
1. public class CustomerInfo - Customer  
// Класс информации по клиенту
2. public class RunInfo - BulkOperationRun  
// Сущность "запуск групповой операции"
3. public class SubscriberInfo - Subscriber  
// Класс информации по абоненту
4. public class SQLStatisticProcessor - SQLStatistic  
// Класс для сбора статистики по SQL-запросам
5. public class ContextLogFieldsManager - OperationContextLogger  
// класс управления логированием контекста групповой операции 

Классов с глаголом в названии у себя не нашел 

**2**
1. makeAttributeName - createAttributeName  
// создание имени атрибута
2. make - create  
// create чаще используется для создания чего-либо, чем make. Имеет смысл в нескольких местах заменить.
3. takeId - getReportId  
// получение идентификатора отчета
4. insertRunnerInstance - createRunnerInstanceRecord  
// создание записи о новом инстансе раннера (запускатора сценариев групповых операций). Бэкенд раннера может быть развернут на разных хостах. Для различных нужд мы сохраняем информацию о хосте, на котором крутится раннер. При первом запуске на новом хосте создается запись об этом в специальной таблице. И кажется create тут более уместен, чем insert (хотя речь в том числе и о вставке в БД)   
5. LdapManager - LdapAuthController
// класс для аутентификации через LDAP
6. PstxidLogManager - PstxIdLogger  
// в предыдущем задании похожий класс (бывший ContextLogFieldsManager) "лишил" постфикса (стал OperationContextLogger). Здесь логично привести в аналогичный вид.
7. было processInputParameters, updateInputParameters, transformInParams, prepareInParameters - стало transformInputParameters  
// в скриптах разных групповых операций метод пред обработки входных параметров называется по-разному. Хотя смысл его везде одинаковый. Заменил везде на единое имя.  