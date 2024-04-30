# Домашнее задание "Типы и структура СУБД"  

---

### Задание 1

Архитектор ПО решил проконсультироваться у вас, какой тип БД лучше выбрать для хранения определённых данных.  
Он вам предоставил следующие типы сущностей, которые нужно будет хранить в БД:    
электронные чеки в json-виде, склады и автомобильные дороги для логистической компании, генеалогические деревья,  
кэш идентификаторов клиентов с ограниченным временем жизни для движка аутенфикации, отношения клиент-покупка для интернет-магазина.    
Выберите подходящие типы СУБД для каждой сущности и объясните свой выбор.  

### Ответ:  

**1) Электронные чеки в json-виде:**  
Для хранения электронных чеков в формате JSON подойдет документо-ориентированная СУБД, такая как MongoDB или Couchbase.    
Эти системы оптимизированы для хранения, извлечения и управления документо-ориентированными данными и поддерживают JSON нативно.     
Это позволяет легко сохранять, обновлять и запрашивать документы, содержащие данные чеков, с использованием их встроенных операций на JSON.
  
**2) Склады и автомобильные дороги для логистической компании:**   
Здесь подойдет графовая СУБД, например, Neo4j или Amazon Neptune.  
Графовые базы данных хорошо подходят для представления и работы со сложными сетевыми структурами, такими как логистические сети, поскольку они позволяют эффективно моделировать взаимосвязи между различными сущностями (складами, пунктами назначения, дорогами) и обеспечивают высокопроизводительный доступ к связанным данным.    

**3) Генеалогические деревья:** 
Генеалогические деревья также хорошо подходят для хранения в графовой СУБД, поскольку они представляют собой сети взаимосвязей между индивидуумами.     
Графовые базы данных позволяют легко отображать сложные иерархии и отношения, что идеально подходит для генеалогических деревьев.    

**4) Кэш идентификаторов клиентов с ограниченным временем жизни для движка аутентификации:**  
Для кэширования данных с ограниченным временем жизни подходит ключ-значение СУБД, такая как Redis или Memcached.   
Эти системы обеспечивают высокую скорость доступа к данным и возможность установки времени жизни для каждого элемента, что идеально подходит для временного хранения данных аутентификации.

**5) Отношения клиент-покупка для интернет-магазина:**  
Для хранения отношений клиент-покупка хорошо подходит реляционная СУБД, такая как PostgreSQL или MySQL.
Реляционные базы данных предлагают строгую структуру таблиц и отношений, которая может быть использована для моделирования и отслеживания транзакций и взаимосвязей между клиентами и их покупками. 
Кроме того, они поддерживают транзакции, что критически важно для обработки покупок в интернет-магазинах.

---

### Задание 2

**Вы создали распределённое высоконагруженное приложение и хотите классифицировать его согласно CAP-теореме. Какой классификации по CAP-теореме соответствует ваша система, если (каждый пункт — это отдельная реализация вашей системы и для каждого пункта нужно привести классификацию):**  
- данные записываются на все узлы с задержкой до часа (асинхронная запись);  
- при сетевых сбоях система может разделиться на 2 раздельных кластера;  
- система может не прислать корректный ответ или сбросить соединение.  
Согласно PACELC-теореме как бы вы классифицировали эти реализации?  

### Ответ:  

**Насколько я понял, CAP-теорема гласит, что распределённая система не может одновременно обеспечить следующие три свойства:**  

- Согласованность (Consistency) - каждый запрос получает самую последнюю версию данных или ошибку;
- Доступность (Availability) - каждый запрос получает корректный ответ, но не обязательно самую последнюю версию данных;
- Устойчивость к разделению (Partition tolerance) - система продолжает работать, несмотря на произвольное количество сообщений, которые могут теряться или задерживаться между узлами в сети.

**Классификация по CAP-теореме:**  

**1) Данные записываются на все узлы с задержкой до часа (асинхронная запись):**  
- Система не обеспечивает Согласованность (C), так как данные записываются с задержкой.  
- Система обеспечивает Доступность (A), так как каждый узел может отвечать на запросы, даже если свежие данные еще не записаны.  
- Система обеспечивает Устойчивость к разделению (P), так как она продолжает работать, несмотря на задержки в сети.  
- CAP классификация: AP  
  
**2) При сетевых сбоях система может разделиться на 2 раздельных кластера:**  

- Система не обеспечивает Согласованность (C), так как после сбоя и разделения кластеров данные могут разойтись.  
- Система обеспечивает Доступность (A), так как оба кластера могут продолжать работать автономно.  
- Система обеспечивает Устойчивость к разделению (P), так как она продолжает работать даже при разделении на кластеры.  
- CAP классификация: AP  
  
**3) Система может не прислать корректный ответ или сбросить соединение:**  
   
- Система не обеспечивает Согласованность (C), так как не всегда может отправить корректный ответ.  
- Система не обеспечивает Доступность (A), так как может сбросить соединение.  
- Система обеспечивает Устойчивость к разделению (P), так как продолжает работать, несмотря на сбои.  
- CAP классификация: P  


**PACELC-теорема гласит, что во время разделения система должна выбрать между Согласованностью и Доступностью, а когда система работает нормально (Else), она должна выбирать между задержкой (Latency) и Согласованностью (Consistency).**  

**Классификация по PACELC-теореме:**

**1) Данные записываются на все узлы с задержкой до часа (асинхронная запись):**  
- PACELC классификация: PA/EL, так как предпочтение отдаётся Доступности во время разделения и увеличивается задержка для обеспечения легкой Согласованности при нормальной работе.  
**2) При сетевых сбоях система может разделиться на 2 раздельных кластера:**  
- PACELC классификация: PA/EL, поскольку система предпочитает Доступность во время разделения и может увеличивать задержку для обеспечения легкой Согласованности при нормальной работе.  
**3) Система может не прислать корректный ответ или сбросить соединение:**  
- PACELC классификация: PA/EC, так как система не обеспечивает ни Согласованность, ни Доступность во время разделения (приоритет не ясен), и выбирает Согласованность в ущерб задержке при нормальной работе.

---

<span style="color: #ff0000;">Доработка</span>

<span style="color:blue;">Красный текст</span>

<span style="color: red;">Согласно CAP-теореме:
1. Данные записываются на все узлы с задержкой до часа (асинхронная запись): AP (Доступность + Устойчивость к разделению)
2. При сетевых сбоях, система может разделиться на 2 раздельных кластера: AP (Доступность + Устойчивость к разделению)
3. Система может не прислать корректный ответ или сбросить соединение: P (Устойчивость к разделению)

Согласно PACELC-теореме:

1. Данные записываются на все узлы с задержкой до часа (асинхронная запись): PA/EL (Предпочтение Доступности при разделении + Предпочтение задержки при отсутствии разделения)
2. При сетевых сбоях, система может разделиться на 2 раздельных кластера: PA/EL (Предпочтение Доступности при разделении + Предпочтение задержки при отсутствии разделения)
3. Система может не прислать корректный ответ или сбросить соединение: PC/EC (Предпочтение Согласованности при разделении + Предпочтение Согласованности при отсутствии разделения)</span>

---

### Задание 3

Могут ли в одной системе сочетаться принципы BASE и ACID? Почему?

### Ответ: 

Сочетание принципов BASE и ACID в одной системе в чистом виде сложно, поскольку они представляют собой разные подходы к обработке данных. ACID стремится к строгой согласованности и надежности, в то время как BASE смиряется с временной неконсистентностью ради доступности и толерантности к разделению. 

Тем не менее как я понимаю, в практических реализациях могут использоваться гибридные модели, которые сочетают некоторые аспекты обеих философий. Например, система может обеспечивать ACID-свойства на уровне отдельных компонентов или сервисов, в то время как в более широком масштабе (например, в микросервисной архитектуре) может применяться BASE-подход, где некоторые операции могут быть консистентны в конечном счете (eventually consistent).  

Таким образом, хотя ACID и BASE предполагают разные методы работы с данными, в реальных системах могут применяться гибридные подходы, которые извлекают выгоду из обоих принципов в зависимости от конкретных требований и контекста.  

---

### Задание 4

Вам дали задачу написать системное решение, основой которого бы послужили:  
фиксация некоторых значений с временем жизни, реакция на истечение таймаута.    
Вы слышали о key-value-хранилище, которое имеет механизм Pub/Sub. Что это за система? Какие минусы выбора этой системы?  

### Ответ: 

Система, которая сочетает key-value хранилище с механизмом Pub/Sub и возможностью установки времени жизни для ключей, скорее всего, относится к NoSQL базам данных вроде Redis. Redis предлагает встроенную поддержку истечения срока действия ключей, а также механизм публикации и подписки, что позволяет реагировать на различные события, включая истечение времени жизни ключа.  

Минусы выбора Redis или подобной системы могут включать:

- Ограниченное место в памяти: Так как Redis хранит всё в оперативной памяти, размер данных ограничен объемом доступной RAM. Это может быть проблемой для очень больших наборов данных или при работе на машинах с ограниченными ресурсами памяти.  
- Устойчивость и долговечность данных: Хотя Redis предлагает различные политики сохранения данных на диск, он не может гарантировать абсолютную долговечность данных при сбое питания или системе, как это делают некоторые традиционные реляционные базы данных.  
- Сложности с транзакциями: Redis поддерживает транзакции с ограниченным функционалом, но они не такие мощные, как в традиционных СУБД, и могут потребовать дополнительной работы для обеспечения консистентности данных.  
- Управление репликацией и масштабированием: Хотя Redis поддерживает репликацию данных и предлагает некоторые опции для горизонтального масштабирования, управление этими процессами может быть сложным и требовать дополнительных усилий и ресурсов.  
- Сложности с моделированием данных: Redis, как key-value хранилище, не предназначен для сложных запросов и отношений между данными, как в реляционных базах данных. Это может сделать моделирование сложных данных более трудоемким.  
Выбор Redis или аналогичной системы должен основываться на требованиях конкретного приложения, принимая во внимание описанные выше ограничения и потенциальные преимущества, такие как высокая скорость доступа к данным и гибкая система подписки на события.   















