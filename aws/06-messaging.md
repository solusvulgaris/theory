# 6. SNS, SQS и асинхронная интеграция

## SQS

SQS — pull-based очередь. 
Producer отправляет сообщение, consumer получает его и после успешной обработки удаляет. 
Visibility timeout временно скрывает сообщение; 
если consumer не удалил его до истечения timeout, сообщение может появиться снова. 
Поэтому обработчики должны быть идемпотентными.

Standard queue обеспечивает высокий throughput и at-least-once delivery, но допускает дубликаты и изменение порядка. 
FIFO queue нужна для ordering и deduplication с соответствующими ограничениями throughput. 
Long polling уменьшает пустые запросы. 
Dead-letter queue хранит сообщения, которые не удалось обработать после заданного числа попыток.

## SNS

SNS — pub/sub: publisher отправляет сообщение в topic, 
а topic доставляет его подписчикам (email, SQS, Lambda, HTTP и другие интеграции). 
Subscription confirmation важна для email/HTTP. 
SNS fan-out позволяет одним событием уведомить несколько независимых consumers.

## Типовой workflow

Приложение публикует событие в SNS или кладёт задачу в SQS. 
Worker читает SQS batch, обрабатывает сообщения, применяет retry/backoff и удаляет только успешно обработанные. 
Для надёжной архитектуры добавляются DLQ, correlation/request ID, CloudWatch metrics и alarms.

## Практические правила

- Не отправлять большие payloads без необходимости; хранить файл в S3, а в сообщении передавать bucket/key и metadata.
- Проверять permissions для producer и consumer отдельно.
- Настраивать visibility timeout длиннее максимального времени обработки.
- Учитывать at-least-once delivery при записи в БД и отправке уведомлений.
