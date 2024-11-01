**7.1**
1. notInFuture - isInFuture // признак постановки заказа "на будущее" (использовался как раз как !notInFuture)
2. isAllowDublicateListItems - canDuplicate // признак разрешения дублирования значений
3. Boolean checkBalance - isNeedCheckBalance // признак необходимости учета баланса клиента
4. checkCallback - isNeedCheckCallback // признак необходимости проверки callback
5. Boolean haveIdentityDoc - isHaveIdentityDoc // признак наличия идентификационного документа

**7.2**  
* Есть в коде вот такое:
```
boolean dateParsedOK = false;
//... преобразование даты и доп действия
if (!dateParsedOK) {
    //..
}
```
Тут просится замена на стандартное имя "success" (количество строк кода небольшое, смысл переменной не потеряется)
* Еще нашел в нескольких местах
```boolean hasError = false``` Меняю на "error"
* Ну и нашел несколько мест где у переменных есть в наименовании "flag". Одна прямо так и называется, хотя по смыслу это не flag, а очень даже "success". Произведенные замены:  
isMasterFlag - isMaster  
successFlag - success  
flag - success  

**7.3**
1. Нашел, что зачем-то в нескольких местах используется int index, а не простое i.
2. Заметил что часто использую цикл while в проходах по какой-либо структуре. Что-то типа вот такого:
```
Iterator<RunData> it = msg.runDataList.iterator();
while (it.hasNext()) {
    RunData rd = it.next();
    ...
```
Тут мне кажется можно заменить на цикл for, в котором оперировать переменной цикла "RunData ranData". 
В учебных примерах первого курса по алгоритмам и структурам данных в перемещениях по связным циклам, там где менял цикл while на for, использовал в качестве переменной цикла обозначение Node iNode. 

Ну а вложенных циклов с длинным телом, в котором была бы необходимость удлинять имя переменной цикла, к счастью, не нашел.

**7.4**   
1. При работе с Exchange в ходе обработки данных идет работа с объектом input, а вот для установки out значения Exchange используется объект с именем result. 
Вот тут просится связка input - output  
2. Нашел в коде такие переменные для ограничения размера страницы: initSize и maxSize.
Тут по смыслу работы кода как раз лучше заменить initSize на minSize, т.к. в коде это не столько инициализационное значение, сколько минимальное ограничение длины списка.
3. Нашел в коде несколько мест, где уже корректно используются антонимы first - last: 
firstOperationId - lastOperationId, firstPage - lastPage

**7.5**
* Нашел несколько одинаковых (по логике) мест, где можно избавиться от временных переменных. Код вот такого типа:
```java
Object wuo = Await.result(sf, Duration.create(60000, TimeUnit.MILLISECONDS));
WorkingRequestResponseMessage workingUnit = (WorkingRequestResponseMessage) wuo;
```
Абсолютно лишние переменные (в данном примере wuo) - далее по коду не используются.
* В учебных примерах для курса АСД нашел несколько классически неряшливо оформленных временных переменных:
```java
//сдвиг значений после удаления значения из упорядоченного списка
//Было:
for (int i = orderedKeys.count(); i >= slot; i--) {
        T temp = values[i];
        values[i] = descentValue;
        descentValue = temp;
}

// можно заменить на:
for (int i = orderedKeys.count(); i >= slot; i--) {
    T nextDescentValue = values[i];
    values[i] = currentDescentValue;
    descentValue = nextDescentValue;
}
```
```java
//Перехеширование при изменении размера хэш-таблицы
//Было:
String[] tempValue = new String[count];
for (int i = 0; i < count; i++) {
    tempValue[i] = slots.getItem(i);
    slots.put(null, i);
}
for (int i = 0; i < count; i++) slots.put(tempValue[i], seekSlot(tempValue[i]));

// Стало:
String[] valuesForRehash = new String[count];
for (int i = 0; i < count; i++) {
    valuesForRehash[i] = slots.getItem(i);
    slots.put(null, i);
}
for (int i = 0; i < count; i++) slots.put(valuesForRehash[i], seekSlot(valuesForRehash[i]));
```