# Описание задач

`Deploy latest version of Prometheus chart inside monitoring namespace (and create it)`
- развертывание последней актуальной версии Prometheus с помощью Helm chart, все требуемые параметры берутся из vars/main.yml

`Enabling RBAC for Prometheus`
- создание и настройка кластерной роли, сервисного аккаунта и ролевой привязки для Prometheus для возможности сбора метрик из других неймспейсов кластера

# Описание переменных в vars:

Prometheus_retention_days: количество дней до ротации метрик, в формате #d
Prometheus_version: версия образа с контейнером Prometheus
