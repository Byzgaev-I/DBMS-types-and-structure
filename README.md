# Домашнее задание "Типы и структура СУБД"  

---

### Задание 1

Архитектор ПО решил проконсультироваться у вас, какой тип БД лучше выбрать для хранения определённых данных.  
Он вам предоставил следующие типы сущностей, которые нужно будет хранить в БД:    
электронные чеки в json-виде, склады и автомобильные дороги для логистической компании, генеалогические деревья,  
кэш идентификаторов клиентов с ограниченным временем жизни для движка аутенфикации, отношения клиент-покупка для интернет-магазина.    
Выберите подходящие типы СУБД для каждой сущности и объясните свой выбор.  

### Ответ:  

1) Электронные чеки в json-виде:  
Для хранения электронных чеков в формате JSON подойдет документо-ориентированная СУБД, такая как MongoDB или Couchbase.    
Эти системы оптимизированы для хранения, извлечения и управления документо-ориентированными данными и поддерживают JSON нативно.     
Это позволяет легко сохранять, обновлять и запрашивать документы, содержащие данные чеков, с использованием их встроенных операций на JSON.
  
2) Склады и автомобильные дороги для логистической компании:   
Здесь подойдет графовая СУБД, например, Neo4j или Amazon Neptune.  
Графовые базы данных хорошо подходят для представления и работы со сложными сетевыми структурами, такими как логистические сети, поскольку они позволяют эффективно моделировать взаимосвязи между различными сущностями (складами, пунктами назначения, дорогами) и обеспечивают высокопроизводительный доступ к связанным данным.    

3) Генеалогические деревья:  
Генеалогические деревья также хорошо подходят для хранения в графовой СУБД, поскольку они представляют собой сети взаимосвязей между индивидуумами.     
Графовые базы данных позволяют легко отображать сложные иерархии и отношения, что идеально подходит для генеалогических деревьев.    

4) Кэш идентификаторов клиентов с ограниченным временем жизни для движка аутентификации:  
Для кэширования данных с ограниченным временем жизни подходит ключ-значение СУБД, такая как Redis или Memcached.   
Эти системы обеспечивают высокую скорость доступа к данным и возможность установки времени жизни для каждого элемента, что идеально подходит для временного хранения данных аутентификации.

5) Отношения клиент-покупка для интернет-магазина:  
Для хранения отношений клиент-покупка хорошо подходит реляционная СУБД, такая как PostgreSQL или MySQL.
Реляционные базы данных предлагают строгую структуру таблиц и отношений, которая может быть использована для моделирования и отслеживания транзакций и взаимосвязей между клиентами и их покупками. 
Кроме того, они поддерживают транзакции, что критически важно для обработки покупок в интернет-магазинах.


### Задание 2

Вы создали распределённое высоконагруженное приложение и хотите классифицировать его согласно CAP-теореме. Какой классификации по CAP-теореме соответствует ваша система, если (каждый пункт — это отдельная реализация вашей системы и для каждого пункта нужно привести классификацию):  
- данные записываются на все узлы с задержкой до часа (асинхронная запись);  
- при сетевых сбоях система может разделиться на 2 раздельных кластера;  
- система может не прислать корректный ответ или сбросить соединение.
Согласно PACELC-теореме как бы вы классифицировали эти реализации?  
