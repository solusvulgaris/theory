# 7. Serverless: Lambda, API Gateway и SAM

## Lambda

Lambda запускает функцию по событию без управления серверами. Важные параметры: runtime, handler, memory, timeout, environment variables, execution role, reserved/provisioned concurrency и layers. Стоимость зависит от числа invocations и compute duration.

События могут приходить от API Gateway, S3, EventBridge, SQS, SNS, расписания и других сервисов. Invocation бывает synchronous, когда вызывающий ждёт ответ, и asynchronous, когда Lambda сама управляет retry. Для SQS Lambda читает batch из event source mapping.

Функции должны быть stateless и идемпотентными. Инициализацию SDK clients и connection pools можно выполнять вне handler: execution environment иногда переиспользуется, но гарантии постоянного состояния нет. Не логировать секреты, использовать least-privilege role, таймауты и DLQ/destinations там, где нужны.

Lambda масштабируется concurrency. Reserved concurrency ограничивает и резервирует capacity; provisioned concurrency уменьшает cold starts ценой постоянной оплаты. Для БД с большим количеством параллельных функций полезен RDS Proxy.

## API Gateway

API Gateway публикует REST/HTTP APIs и WebSocket APIs. Он выполняет routing, authentication/authorization, throttling, quota, stages, versioning, metrics и access logs. Интеграция API Gateway + Lambda формирует serverless HTTP backend. Защита может использовать IAM, Cognito, Lambda authorizer и API keys/usage plans.

Нужно валидировать input, ограничивать payload/rate, корректно маппить ошибки и не полагаться только на скрытие endpoint. CloudWatch даёт latency, errors и request metrics.

## Function URLs и SAM

Lambda Function URL — более простой HTTP endpoint для функции; API Gateway подходит, когда нужны маршруты, авторизация, throttling и lifecycle management. AWS SAM — расширение CloudFormation: описывает Lambda, API, permissions и другие ресурсы в шаблоне, поддерживает local build/test и единый deployment stack.
