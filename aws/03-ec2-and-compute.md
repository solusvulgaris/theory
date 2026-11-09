# 3. EC2, EBS и масштабирование

## EC2

EC2 — виртуальные серверы. 
При создании выбираются AMI, instance type, сеть/subnet, IAM instance profile, storage и security group. 
AMI содержит образ ОС и настройки для запуска экземпляров.

Instance type определяет vCPU, memory, network и storage characteristics. 
Для экономии подбирают free-tier типы, Spot для прерываемых задач, Savings Plans/Reserved Instances для стабильной нагрузки.

## Security group

Security group — stateful virtual firewall. 
Inbound rules разрешают входящие соединения, outbound — исходящие. 
Обычно открывают HTTP/HTTPS для пользователей, SSH только из административного диапазона или через bastion/SSM. 
Не открывать SSH всему интернету без необходимости.

## Инициализация

User data/cloud-init выполняется при старте и может 
установить пакеты, скачать артефакт из S3, запустить HTTP-сервер и настроить автозапуск. 
Для повторяемости лучше использовать подготовленный AMI или configuration management. 
Instance role даёт приложению доступ к AWS без хранения ключей.

## EBS

EBS — persistent block storage для EC2. 
Volume обычно привязан к AZ, его можно detach/attach к другому экземпляру в той же AZ. 
Нужно форматировать и монтировать volume, продумать filesystem, snapshots, encryption и backup. 
Snapshot — инкрементальная резервная копия, хранящаяся в S3-managed storage.

## Балансировка и Auto Scaling

Launch template описывает, как запускать экземпляры. 
Auto Scaling Group поддерживает desired/min/max capacity и заменяет unhealthy instances. 
Scaling policy может ориентироваться на CPU или request count. 
Load Balancer распределяет трафик: ALB — HTTP/HTTPS и routing по host/path, NLB — L4 и высокая производительность.

Надёжная схема: минимум две AZ, несколько экземпляров, health checks, immutable/rolling deployment 
и отсутствие локального состояния на instance. 
Статические данные выносить в S3, сессии — во внешний storage.
