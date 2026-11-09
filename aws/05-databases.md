# 5. Управляемые базы данных

## Выбор типа

- Relational: таблицы, SQL, joins и транзакции; подходит для согласованных связей.
- NoSQL: гибкая модель и горизонтальное масштабирование; DynamoDB проектируется от access patterns.
- In-memory: Redis/Memcached для cache и быстрых временных данных.

## Amazon RDS

RDS управляет provisioning, patching, backups, monitoring и failover для поддерживаемых relational engines. 
Клиент отвечает за schema, индексы, запросы, права и параметры приложения.

Multi-AZ — standby в другой AZ для высокой доступности и автоматического failover; standby не предназначен для read scaling. 
Read replica — асинхронная копия для разгрузки чтения и отдельных сценариев миграции. 
Это разные механизмы и их можно комбинировать.

RDS поддерживает automated backups, manual snapshots, point-in-time restore, encryption и CloudWatch metrics. 
Следует планировать maintenance window, storage autoscaling, connection limits, slow query analysis и тест восстановления. 
Aurora — совместимый с MySQL/PostgreSQL managed engine с распределённым storage и отдельными read replicas.

## DynamoDB

DynamoDB — managed key-value/document database. Таблица использует partition key и, опционально, sort key. 
Хороший дизайн начинается с запросов: ключи должны равномерно распределять нагрузку и поддерживать нужные access patterns. 
Индексы (GSI/LSI), conditional writes и transactions помогают моделировать операции.

Eventually consistent read дешевле/быстрее, но может видеть не самую свежую запись. 
Strongly consistent read возвращает актуальное значение в поддерживаемых сценариях. 
Capacity бывает provisioned или on-demand; для защиты от перегрузок используются throttling и adaptive capacity. 
Backups, point-in-time recovery, encryption и IAM нужны как часть эксплуатации.

## ElastiCache

Redis предоставляет богатые структуры данных, persistence, replication и дополнительные возможности; 
Memcached проще и подходит для распределённого ephemeral cache. 
Cache не должен быть единственным источником критичных данных. 
Нужны TTL, стратегия invalidation, sizing и защита сети.
