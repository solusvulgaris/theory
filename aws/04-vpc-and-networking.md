# 4. VPC и сетевая безопасность

VPC — изолированная виртуальная сеть с CIDR-блоком, например `10.0.0.0/16`. 
Subnet принадлежит одной AZ и бывает public или private по маршрутизации, а не по названию.

## Основные компоненты

- Internet Gateway даёт VPC маршрут в интернет.
- Public subnet имеет route `0.0.0.0/0` на Internet Gateway.
- Private subnet не принимает входящие соединения из интернета; 
исходящий доступ обычно идёт через NAT Gateway в public subnet.
- Route table определяет маршруты subnet.
- Network ACL — stateless фильтр на уровне subnet.
- Security group — stateful фильтр на уровне ENI/instance.
- Bastion host — контролируемая точка административного доступа; 
предпочтительнее AWS Systems Manager Session Manager, когда возможно.

## Типовая схема

В двух AZ создаются public subnets для ALB/NAT/bastion, private subnets для application EC2/Lambda и отдельные DB subnets для RDS.
RDS security group принимает соединения только от application security group. 
NAT позволяет private instances скачать обновления, но не делает их доступными извне.

## VPC best practices

- Планировать CIDR так, чтобы оставался запас и не было пересечения с on-premises сетью.
- Разносить critical components по AZ.
- Не использовать public IP для БД.
- Ограничивать правила SG конкретными SG, портами и источниками.
- Помнить, что NAT Gateway и cross-AZ traffic стоят денег.
- Для private AWS services использовать VPC endpoints, если это снижает путь через NAT.
