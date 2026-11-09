# AWS Developer — overview-конспект

Конспект собран по материалам курса `EPAM CloudX Associate — AWS Developer program`: 
общим заметкам, теоретическим README и практическим заданиям. 
Это учебный обзор, а не замена актуальной документации AWS.

## Разделы

1. [Основы AWS и IAM](01-aws-basics-and-iam.md)
2. [S3 и хранение объектов](02-s3.md)
3. [EC2, EBS и масштабирование](03-ec2-and-compute.md)
4. [VPC и сетевая безопасность](04-vpc-and-networking.md)
5. [Базы данных](05-databases.md)
6. [SNS, SQS и интеграция](06-messaging.md)
7. [Serverless: Lambda, API Gateway и SAM](07-serverless.md)
8. [CI/CD и AWS Developer Tools](08-ci-cd.md)

## Сквозная архитектура курса

Пользователь обращается к API Gateway или Load Balancer. 
Приложение работает на Lambda либо EC2 в VPC, 
статические файлы хранятся в S3, 
данные — в RDS или DynamoDB. 
SNS/SQS связывают компоненты асинхронно, 
CloudWatch собирает метрики и логи, 
а CodePipeline автоматизирует сборку и доставку.

## Общие правила

- Минимизировать IAM-права и не хранить ключи в коде.
- Разделять public и private subnet, доступ к БД ограничивать security group.
- Включать логирование, мониторинг, резервное копирование и tagging.
- Учитывать регион, тип ресурса, объём и длительность использования при оценке стоимости.
- Инфраструктуру описывать кодом, где это возможно, и проверять изменения до deployment.
