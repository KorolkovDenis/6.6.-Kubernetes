# Домашнее задание к занятию «6.6 Kubernetes. Часть 2»

### Оформление домашнего задания

1. Домашнее задание выполните в [Google Docs](https://docs.google.com/) и отправьте на проверку ссылку на ваш документ в личном кабинете.  
1. В названии файла укажите номер лекции и фамилию студента. Пример названия: 6.6. Kubernetes. Часть 2 — Александр Александров.
1. Перед отправкой проверьте, что доступ для просмотра открыт всем, у кого есть ссылка. Если нужно прикрепить дополнительные ссылки, добавьте их в свой Google Docs.

Любые вопросы по решению задач задавайте в чате учебной группы.

---

## Важно

Перед отправкой работы на проверку удаляйте неиспользуемые ресурсы. Это нужно, чтобы предупредить неконтролируемый расход средств, полученных после использования промокода.

Рекомендации [по ссылке](https://github.com/netology-code/sdvps-homeworks/tree/main/recommend).

---

### Задание 1

**Выполните действия:**

1. Создайте свой кластер с помощью kubeadm.
1. Установите любой понравившийся CNI плагин.
1. Добейтесь стабильной работы кластера.

В качестве ответа пришлите скриншот результата выполнения команды `kubectl get po -n kube-system`.

Ответ:

по плану у меня в кластере 1 master-нода и 2 worker- ноды:

прописываю их все в /etc/hosts на всех нодах для дальнейшего благополучного взаимодействия.

echo "192.168.43.178 k8s-master-1.dv.local k8s-master-1" | sudo tee -a /etc/hosts
echo "192.168.43.224 k8s-node-1.dv.local k8s-node-1" | sudo tee -a /etc/hosts
echo "192.168.43.222 k8s-node-2.dv.local k8s-node-2" | sudo tee -a /etc/hosts

![screen1](https://github.com/KorolkovDenis/6.6.-Kubernetes/blob/main/screenshots/screen1.jpg)

###До установки calico:

Посмотрим список всех узлов в кластере и статус каждого узла с помощью команды:

kubectl get nodes

![screen2](https://github.com/KorolkovDenis/6.6.-Kubernetes/blob/main/screenshots/screen2.jpg)

Узлы находятся статусе “NotReady”. Чтобы это исправить нужно установить CNI (Container Network Interface) или сетевые надстройки, такие как Calico, Flannel и Weave-net.

Посмотрим список всех узлов в кластере и статус каждого узла с помощью команды:

kubectl get pods -n kube-system

![screen3](https://github.com/KorolkovDenis/6.6.-Kubernetes/blob/main/screenshots/screen3.jpg)

###После установки calico:

Узлы находятся статусе “Ready” и кластер Kubernetes готов к работе.

kubectl get nodes
kubectl get pods -n kube-system

![screen4](https://github.com/KorolkovDenis/6.6.-Kubernetes/blob/main/screenshots/screen4.jpg)

---

### Задание 2

Есть файл с деплоем:

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis
spec:
  selector:
    matchLabels:
      app: redis
  replicas: 1
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
      - name: master
        image: bitnami/redis
        env:
         - name: REDIS_PASSWORD
           value: password123
        ports:
        - containerPort: 6379
```
**Выполните действия:**

1. Создайте Helm Charts.
1. Добавьте в него сервис.
1. Вынесите все нужные, на ваш взгляд, параметры в `values.yaml`.
1. Запустите чарт в своём кластере и добейтесь его стабильной работы.

В качестве ответа пришлите вывод команды `helm get manifest <имя_релиза>`.

Ответ:

```
kubectl cluster-info
kubectl get nodes
kubectl get po -n kube-system
kubectl get po
helm list
```

![screen5](https://github.com/KorolkovDenis/6.6.-Kubernetes/blob/main/screenshots/screen5.jpg)

```
helm get manifest my-helm-project-chart
```

![screen6](https://github.com/KorolkovDenis/6.6.-Kubernetes/blob/main/screenshots/screen6.jpg)

---
## Дополнительные задания* (со звёздочкой)

Их выполнение необязательное и не влияет на получение зачёта по домашнему заданию. Можете их решить, если хотите лучше разобраться в материале.

---

### Задание 3*

1. Изучите [документацию](https://kubernetes.io/docs/concepts/storage/volumes/#hostpath) по подключению volume типа hostPath.
1. Дополните деплоймент в чарте подключением этого volume.
1. Запишите что-нибудь в файл на сервере, подключившись к поду с помощью `kubectl exec`, и проверьте правильность подключения volume.

В качестве ответа пришлите получившийся yaml-файл.


## Моя полная работа в Google:

[Моя работа по Kubernetes](https://docs.google.com/document/d/1Fxe3hFqvGzJJq_71se5I4TKkli9w5F40/edit?usp=share_link&ouid=104113173630640462528&rtpof=true&sd=true)