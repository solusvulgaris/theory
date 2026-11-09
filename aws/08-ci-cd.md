# 8. CI/CD и AWS Developer Tools

## CI/CD

Continuous Integration означает частые изменения в общей ветке с автоматической сборкой и тестами. Continuous Delivery держит release готовым к ручному запуску; Continuous Deployment автоматически доставляет прошедшие проверки изменения.

Pipeline обычно содержит source, build/test, artifact, deploy и post-deployment verification. Артефакты должны быть версионированы и воспроизводимы. Secrets хранятся в Secrets Manager/Parameter Store, а не в repository или build logs.

## CodeCommit

Managed Git repository. В современных проектах источник может быть GitHub/Bitbucket или другой provider; важны webhook/trigger и branch protection независимо от выбора.

## CodeBuild

CodeBuild создаёт изолированное build environment, получает source, читает `buildspec.yml`, выполняет install/pre_build/build/post_build и сохраняет artifacts, часто в S3. Логи идут в CloudWatch Logs. Build project задаёт image, runtime, compute type, environment variables, cache, service role и artifact configuration.

Build должен запускать compile, unit tests, static analysis и упаковку. Не выводить secrets в лог; использовать cache только для ускорения, не как источник истины.

## CodeDeploy

CodeDeploy доставляет приложение на EC2/on-premises, Lambda или ECS. Для EC2 важны application, deployment group, agent и lifecycle hooks. Стратегии могут быть in-place или blue/green. Для Lambda/ECS traffic shifting бывает canary, linear или all-at-once. Нужны health checks и автоматический rollback.

## CodePipeline

CodePipeline связывает source, build, test и deploy actions. Перед production полезны manual approval, integration tests и staged environments. Pipeline должен быть observable: статусы, CloudWatch alarms, notifications и audit trail.

## App Runner

App Runner упрощает запуск web service из source repository или container image: предоставляет build/deploy, HTTPS, scaling и service endpoint. Это удобнее для простого stateless web-приложения, но даёт меньше контроля, чем EC2/ECS/EKS, и имеет свои ограничения и стоимость.

## IaC и эксплуатация

CloudFormation/SAM позволяют описывать ресурсы, зависимости, outputs и параметры декларативно. Изменения проходят code review, stack update и rollback. Для production нужны отдельные environments/accounts, tagging, least-privilege deployment role, backups, alarms и план удаления ресурсов.
