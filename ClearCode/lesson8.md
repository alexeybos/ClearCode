1. private static final Level debugLevelDebug - DEBUG_LEVEL_DEBUG  
   private static final Level debugLevelTrace - DEBUG_LEVEL_TRACE  
// уровни дебага - поменял имена на заглавные буквы по стандарту
2. private static final Pattern allowed - ALLOWED_PATTERN  
// паттерн, для оценки ответа (разрешается действие или нет) - поменял имя на более говорящее и сделал заглавные буквы по стандарту
3. private static final long PROCESSING = 1L /*статус В обработке*/ - STATUS_PROCESSING  
   private static final long ACCEPTED = 2L /*статус Учтено*/ - STATUS_ACCEPTED  
// константные значения статусов обрабатываемых сущностей. Указал в наименовании сущность, а не только значение
4. private static final String MAX_DATE - MAX_FISCAL_PERIOD_END_DATE  
// Максимальная дата окончания действия фискального периода. Здесь также уточнил сущность
5. private static final String CALCULATED = "CALCULATED" - PENALTY_STATUS_CALCULATED  
   private static final String PRESENTED = "PRESENTED" - PENALTY_STATUS_PRESENTED  
// Указал в наименовании сущность (статус пени)
6. private static final String CONFLICTTYPE = "ERROR" - CONFLICT_TYPE  
// Слова в константе должны быть разделены "_" - разделил
7. ввел константу int MAX_COMMENT_LENGTH  
// было "магическое число" if (comment.length() > 255) {  
8. ввел константу int PRODUCT_LIST_MAX_PAGE_SIZE = 1000  
// было "магическое число", передаваемое в запрос
9. ввел константу String CUSTOMER_OPER_CATEGORY  
// было: if ("CUSTOMER".equals(operationCategory)) {
10. ввел константу String DUPLICATE_NOT_ALLOWED  
// было: "N".equals(op.getIsDuplicateAllowed())  
// стало: DUPLICATE_NOT_ALLOWED.equals(op.getIsDuplicateAllowed())
11. ввел константы String TYPE_FIELD и String CONFLICT_STATUS_ERROR   
// было: if (conflict.get("type").equals("ERROR")) {  
// стало: if (conflict.get(TYPE_FIELD).equals(CONFLICT_STATUS_ERROR)) {
12. разделил одну константу, которая использовалась для разных задержек (DELAY) 
на две: ZOO_SYNC_DELAY_MS и DB_BLOCK_WAITING_DELAY_MS  
// ранее константа DELAY использовалась в качестве задержки при выполнении серии попыток
синхронизации БД и Zookeeper, а так же в качестве ожидания снятия блокировки при редактировании статуса синхронизации  