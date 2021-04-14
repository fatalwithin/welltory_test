# Что нужно проверить/сделать, прежде чем выполнять задачи (check-list):
- Есть ли доступ к бастион-хосту, с которого нужно выполнять команды из пунктов ниже
- Есть ли административный доступ в кластер Kubernetes, в котором будет производиться работа, с данного бастион-хоста
- Установлен ли на бастион-хосте Ansible. Если нет - необходимо установить по [данному руководству](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-ansible)

## Как развернуть манифест:

### Развертывание Redis:

`some command`

Проверяем, что поды Redis запущены:

`kubectl get pods -n redis`

Вывод команды должен быть примерно такой:

```
redis-master-0   2/2     Running   0          3m40s
redis-slave-0    2/2     Running   0          2m4s
redis-slave-1    2/2     Running   0          2m16s 
```

`2/2` в поле работающих контейнеров в поде означает, что запустились сайдкары Prometheus exporter - компонента, который и будет отдавать метрики Redis

### Развертывание Prometheus:



## Как изменить версию у Prometheus:

Для начала нужно выполнить обновление helm-чарта, с помощью которого развертывался Prometheus:
`команды-для-обновления-чарта`

Prometheus у нас развертывается в виде контейнера, образ контейнера находится по следующему адресу: `http://quay.io/prometheus/prometheus`
Ищем тег нужного образа и данное значение тега прописываем вот [сюда](https://github.com/fatalwithin/welltory_test/blob/524769ec2955e4651d221748463a6569106d2c6a/playbooks/roles/install-prometheus/vars/main.yml#L2) взамен исходного.

Далее - запускаем процедуру развертывания Prometheus:
`some commands`
