**3.1** Добавил комментарии (все комментарии ниже - новые добавленные)  
1. 
```groovy
List<Map> nextChargeDate = getSubscriberNextChargesDate(subscriberId, [ratePlanProductId])
//не нашли дату следующего списания по тарифному плану абонента - ищем дату по пакетам начислений абонента
if (!nextChargeDate || nextChargeDate.isEmpty()) { 
    nextChargeDate = getChargeDatesByPackIds(subscriberId)
}
```
2. 
```groovy
//если указан ид участия в программе лояльности получаем суммы скидок.
if (loyaltyRewardId != null) {
    Long loyaltyProgramGroupId = ParamsUtil.getLong(inParams, "loyaltyProgramGroupId")
    currentOrder = orders.remove(0)
    resultRow.put("loyaltyProgramGroupId", loyaltyProgramGroupId)
    Map response = getRewardsParameters()
    //...
}
```
3. 
```groovy
Map productSpecification = items[0].get("productSpecification")
//если заполнена спецификация продукта ищем и заполняем соответствующие спецификации клиентских сервисов и ресурсов
if (productSpecification) {
    List customerServiceSpecifications = (List) productSpecification.get("customerServiceSpecifications")
    if (customerServiceSpecifications != null && customerServiceSpecifications.size() != 0) {
        Long customerServiceSpecId = customerServiceSpecifications[0].get("customerServiceSpecId")
        result.put("customerServiceSpecId", customerServiceSpecId)
        List resourceSpecifications = (List) ((Map) customerServiceSpecifications.get(0)).get("resourceSpecifications")
        if (resourceSpecifications != null && resourceSpecifications.size() != 0) {
            Long resourceSpecId = (Long) resourceSpecifications[0].get("resourceSpecId")
            result.put("resourceSpecId", resourceSpecId)
        }
    }
}
```
4. 
```groovy
//ищем у абонента скидки для редактирования, если во входных параметрах есть значения объемов скидок
if (!discountsVolumes.isEmpty()) {
    Map packDiscountsProcessResult = getPackDiscountsForUpdate(oapiWrapper, scenarioRequestId, subscriberId,
            discountsVolumes, subscriberPackId, locale, pstxid)
    discountsForUpdate = (List) packDiscountsProcessResult.get("resultDiscounts")
    discountsWithProblem = (Map) packDiscountsProcessResult.get("discountCountMap")
}
```
5. 
```java
ResultItemExcelReportBuilder reportBuilder = reportBuilderMap.get(operationTypeCode);
ResultItemExcelReportBuilder newInstanceReportBuilder = null;
// если для кода операции определен кастомный билдер, то инициализируем его параметры
if (reportBuilder != null) {
    newInstanceReportBuilder = reportBuilder.getClass().newInstance();
    //тут код с инициализацией необходимых параметров кастомного билдера 
}
```
6. 
```java
//если ошибка доступа к ресурсам, ставим сценарий на паузу для последующего переповтора запроса
if (!this.stopFlag && isResourceError) {
    FiniteDuration delay = ErrorUtil.isResourceException(msg.error) ?
            Duration.create(30, TimeUnit.SECONDS) :
            Duration.create(500, TimeUnit.MILLISECONDS);
    LOG.warn(msg.error.getMessage(), msg.error);
    
}
```
7. 
```java
for (Long runId : this.runInfoMap.keySet()) {
    //попытка перезапустить все запуски, помеченные как остановленные
    RunInfo runInfo = this.runInfoMap.get(runId);
    if (runInfo == null) continue;
    if (!runInfo.isStopFlag()) continue;
    try {
        processRun(runInfo);
    } catch (Throwable e) {
        LOG.error(e.getMessage(), e);
    }
}
```
**3.2** Убранные комментарии
1. Было:
```groovy
if (result.code >= 400) { //не смогли найти адрес
    //пытаемся создать неструктурированный адрес
    String urlCreate = "/service/addresses"
    Map bodyCreate = [addressString: addressString]
    bodyCreate.put("addressElements", [])
    String bodyCreateString = oapiService.toJSON(bodyCreate, locale)
    
    ...тут еще код получения неструктурированного адреса
    
    OapiResponse createResult = oapiService.execute()
    Map createResultMap = (Map) createResult.contentObject
    resAddrId = createResultMap.get("addressId")
} else {
    resAddrId = resultMap.get("addressId")
}
```
Стало:
```groovy
boolean structuredAddrNotFound = result.code >= 400
if (structuredAddrNotFound) { 
    resAddrId = createNonStructuredAddr()
} else {
    resAddrId = resultMap.get("addressId")
    LOGGER.debug(labelService.msg("EndUserInfoManage.getAddressClear.finishSuccess", scenarioRequestId, subscriberId, resAddrId))
}
```
2. Было:
```groovy
//если ничего не получили, то информируем об ошибке
if (response.code != HttpStatus.SC_OK || items == null || items.isEmpty()) {
    String errorMessage = responseMap.get("userMessage")
    result.put(ERROR_MESSAGE, errorMessage != null && !errorMessage.isEmpty() ? errorMessage : labelService.msg("ProductOfferingActivate.user.execute.internalError"))
} else {
    result.put("charges", ((Map) items.get(0)).get("bonusPrice"))
}
```
Стало:
```groovy
boolean success = !(response.code != HttpStatus.SC_OK || items == null || items.isEmpty())
if (success) {
    result.put("charges", ((Map) items.get(0)).get("bonusPrice"))
} else {
    String errorMessage = responseMap.get("userMessage")
    result.put(ERROR_MESSAGE, errorMessage != null && !errorMessage.isEmpty() ? errorMessage : labelService.msg("ProductOfferingActivate.user.execute.internalError"))
}
```
3. Было:
```java
//надо вешать ожидание callback или завершать обработку
if ((loadRequest != null) && (dateStr != null)
    && (loadRequest.getCorrelationId() != null)
    && (!loadRequest.getCorrelationId().trim().isEmpty())
    && (loadRequest.getReplyTo() != null)
    && (!loadRequest.getReplyTo().trim().isEmpty())) {
        RunFinalNotification runFinalNotification = new RunFinalNotification();
        //...тут длинный код заполнения полей runFinalNotification
        runInfo.setStateInitFlag(true);
} else {
        runInfo.setInternalState(RunInternalState.END);
        processRun(runInfo);
}
```
Стало:
```java
boolean isNeedWaitCallback = (loadRequest != null) && (dateStr != null)
        && (loadRequest.getCorrelationId() != null)
        && (!loadRequest.getCorrelationId().trim().isEmpty())
        && (loadRequest.getReplyTo() != null)
        && (!loadRequest.getReplyTo().trim().isEmpty());
if (isNeedWaitCallback) {
        RunFinalNotification runFinalNotification = new RunFinalNotification();
        //...тут длинный код заполнения полей runFinalNotification
        runInfo.setStateInitFlag(true);
} else {
        runInfo.setInternalState(RunInternalState.END);
        processRun(runInfo);
}
```
4. Было:
```java
//если операция не найдена или на нее у пользователя нет прав - операция принадлежит не текущему пользователю (без роли Supervisor)
if (!bulkRequest.isPresent()
                || (!accessControlUtil.isSupervisor(input.identity)
                    && !currentUser.equals(bulkRequest.get().getNaviUser()))) {
    logger.error("request with id {} with operation id {} not found", params.getBulkRequestId(),
            params.getBulkOperationId());
    throw new WebCamelException(messageResolver.getMessage(input.languageId,
            "OperationService.error.operationOfRequestNotFound", params.getBulkOperationId(),
            params.getBulkRequestId()),
            "ObjectNotFound", 404);
}
```
Стало:
```java
boolean operationNotFound = !bulkRequest.isPresent();
boolean isNotAllowedToView = !accessControlUtil.isSupervisor(input.identity) && !currentUser.equals(bulkRequest.get().getNaviUser());
if (operationNotFound || isNotAllowedToView) {
    logger.error("request with id {} with operation id {} not found", params.getBulkRequestId(),
            params.getBulkOperationId());
    throw new WebCamelException(messageResolver.getMessage(input.languageId,
            "OperationService.error.operationOfRequestNotFound", params.getBulkOperationId(),
            params.getBulkRequestId()),
            "ObjectNotFound", 404);
}
```
5. Было:
```java
//если параметры не пустые, парсим их
if (params != null && !params.isEmpty() && !params.replace(" ", "").equals("{}")) {
    JsonNode parsedParams;
    try {
        parsedParams = this.objectMapper.readTree(params);
        ObjectNode jsonNode = new ObjectNode(objectMapper.getNodeFactory());
        //... тут много кода обработки параметров
    } catch (IOException e) {
        throw new MappingException("текст ошибки");
    }
    //... еще код преобразования параметров
    this.mapper.updateValues(res, parsedParams, model, mappingName, locale);
}
```
Стало:
```java
boolean isInputParamsNotEmpty = params != null && !params.isEmpty() && !params.replace(" ", "").equals("{}");
if (isInputParamsNotEmpty) {
    this.mapper.updateValues(res, parseInputParams(params), model, mappingName, locale);
}
```