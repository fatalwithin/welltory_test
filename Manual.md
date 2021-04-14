# Что нужно проверить/сделать, прежде чем выполнять задачи (check-list):
- Есть ли доступ к бастион-хосту, с которого нужно выполнять команды из пунктов ниже
- Есть ли административный доступ в кластер Kubernetes, в котором будет производиться работа, с данного бастион-хоста
- Установлен ли на бастион-хосте Ansible. Если нет - необходимо установить по [данному руководству](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-ansible)

## Как развернуть манифест:

### Устанавливаем модуль Ansible community-kubernetes:

`ansible-galaxy collection install community.kubernetes`

### Развертывание Redis:

`some command`

Проверяем, что поды Redis запущены:

`kubectl get pods -n redis`

Вывод команды должен быть примерно такой:

```
welltory-redis-master-0   2/2     Running   0          3m40s
welltory-redis-slave-0    2/2     Running   0          2m4s
welltory-redis-slave-1    2/2     Running   0          2m16s 
```

`2/2` в поле работающих контейнеров в поде означает, что запустились сайдкары Prometheus exporter - компонента, который и будет отдавать метрики Redis

### Развертывание Prometheus:

`some command`

Проверяем, что поды Prometheus запущены:

`kubectl get pods -n monitoring`

Вывод команды должен быть примерно такой:

```
NAME                                               READY   STATUS    RESTARTS   AGE
welltory-prometheus-alertmanager-86986878b5-5w2vw        2/2     Running   0          76s
welltory-prometheus-kube-state-metrics-dc694d6d7-xk85n   1/1     Running   0          76s
welltory-prometheus-node-exporter-grgqw                  1/1     Running   0          76s
welltory-prometheus-node-exporter-njksq                  1/1     Running   0          76s
welltory-prometheus-node-exporter-pcmgv                  1/1     Running   0          76s
welltory-prometheus-pushgateway-6694855f-n6xwt           1/1     Running   0          76s
welltory-prometheus-server-77ff45bc6-shrmd               2/2     Running   0          76s
```

Добавлять снятие метрик для развернутых сайдкар-контейнеров redis-exporter в конфигурацию Prometheus не нужно, так как он по умолчанию содержит kubernetes_sd_configs, обеспечивающий Service Discovery. За счёт этого динамически подключается сбор метрик для вновь появившихся подов. 

## Как изменить версию у Prometheus:

Для начала нужно выполнить обновление helm-чарта, с помощью которого развертывался Prometheus:
`команды-для-обновления-чарта`

Prometheus у нас развертывается в виде контейнера, образ контейнера находится по следующему адресу: `http://quay.io/prometheus/prometheus`
Ищем тег нужного образа и данное значение тега прописываем вот [сюда](https://github.com/fatalwithin/welltory_test/blob/524769ec2955e4651d221748463a6569106d2c6a/playbooks/roles/install-prometheus/vars/main.yml#L2) взамен исходного.

Далее - запускаем процедуру развертывания Prometheus:
`some commands`
