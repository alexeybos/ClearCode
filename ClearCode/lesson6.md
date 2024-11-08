1. String mbeanName(String name) - getMBeanFullName  
// получение полного имени MBean (для ведения статистики)
2. private Object regReq(Object message) - registerWaitingRequest  
// регистрация (во внутренней структуре) запроса, который будет ожидать callback
3. private void inspectStopingRuns() - checkStoppingRuns  
// проверка останавливаемых запусков
4. private boolean checkExists(String path) - isNodeExists  
// проверка наличия узла в zookeeper
5. private boolean checkNextPages() - hasNextPage  
// проверка наличия следующей страницы в выборке
6. private void processOvertimeWaiting() - rerunOvertimeWaitingTasks()  
// перезапуск задач, ожидающих коллбэк сверх положенного времени
7. private Date nexDate(long timeout) - getNextDateForTasksRedistribute  
// получить следующую дату для перераспределения задач (c одного инстанса раннера на другой)
8. public Object currentRunnerId() - getCurrentRunnerId()  
// получение идентификатора текущего раннера (инстанаса раннера)
9. private Map editRC(...) - getAndEditRecurringCharges()  
// получить и отредактировать регулярные начисления (абон плата и аналогичное)
10. private String packageInfo() - getPackageDescription()  
// получить описание пакета (услуг)
11. private Map packageLaunchAction(...) - prepareParamsAndActivateOrEditPackage()
// Подготавливаем данные и инициируем заказ на подключение/редактирование пакета - метод-кандидат на рефакторинг
12. private Map callTimezoneOapi(Long subscriberId) - getSubscriberTimeZone()
// получение тайм-зоны абонента