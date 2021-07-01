# Задание 1. Что сделал
1. Настроил окружение
2. Создал Dockerfile для сборки образа приложения web. Залил образ на hub.docker.com
3. Создал манифест для запуска пода с приложeнием web
4. Собрал образ hipster shop frontend, залил в репозиторий
5. Создал рабочий манифест для запуска hipster shop frontend приложения

### Вопрос про поды из системного неймспейса
* etcd, kube-apiserver, kube-controller-manager, kube-scheduler - kubelet создает, используя манифесты из /etc/kubernetes/manifests/
* kube-proxy управляется DemonSet-ом
* core-dns управляется ReplicaSet-ом

# Задание 2. Что сделал
1. Установил kind
2. Создал и запушил на hub.docker.com приложения frontend и paymentservice версий v0.0.1 и v0.0.2
2. Создал манифесты ReplicaSet для этих приложений
3. Создал манифесты Deployment для этих приложений
4. С помощью указания свойств maxUnavailable и maxSurge создал два манифеста Deployment приложения paymentservice, которые реализуют сценарии развертывания 'blue-green' и 'Reverse Rolling Update' *
5. В frontend-deployment.yaml добавил проверку того, что приложение запущенно, с помощью readinessProbe
6. Создал манифест Daemonset приложения node-exporter, который при применении запускается как на worker, так и на master нодах **

### Почему обновление ReplicaSet не повлекло обновление запущенных pod?
* ReplicaSet не проверяет соответствие запущенных подов примененному шаблону, поэтому при применении шаблона не произошел перезапуск подов
* Deployment же, в свою очередь, сверяет шаблон с описанием запущенных подов, поэтому и пересоздаст эти поды

# Задание 3. Что сделал
1. Задача 1
    * Создал sa bob, назначил ему роль admin на весь кластер с помощью ClusterRoleBinding
    * Создал sa dave
2. Задача 2
    * Создал ns prometheus
    * Создал sa carol в ns prometheus
    * Создал кластерную роль дающую права совершать по отношению к pods операции get, list, watch
    * Назначил созданную роль на группу system:serviceaccounts:prometheus
3. Задача 3
    * Создал ns dev
    * В этом неймспейсе создал пользователей jane и ken
    * С помощью манифеста RoleBinding назначил jane кластерную роль admin в ns dev
    * С помощью манифеста RoleBinding назначил ken кластерную роль view в ns dev

# Задание 4. Что сделал
1. Поднял StatefulSet с помощью манифеста minio-statefulset.yaml
2. Поднял Service с помощью minio-headless-service.yaml
3. Сделал коммит
4. Создал Secret (minio-statefulset-secret.yaml), в который занес преобразованные в base64 значения переменных окружения MINIO_ACCESS_KEY и MINIO_SECRET_KEY из манифеста minio-statefulset.yaml *
5. Изменил minio-statefulset.yaml таким образом, чтобы значения переменных окружения MINIO_ACCESS_KEY и MINIO_SECRET_KEY брались из секрета *

# Задание 5. Что сделал
1. Создал Deployment web приложения с livenessProbe и readinessProbe
2. Создал сервис для web подов типа ClusterIP
3. Перевел kube-proxy в режим работы ipvs
4. Установил и настроил MetalLB
5. Создал сервис для web подов типа LoadBalancer
6. Создал сервис типа LoadBalancer, открывающий доступ к CoreDNS *
7. Установил и сконфигурировал ingress-nginx
8. Создал headless-сервис для web подов и объект ingress для балансировки на этот сервис по пути /web/
9. Создал объект ingress для балансировки по пути /dashboard/ на сервис kubernetes-dashboard. kubernetes-dashboard предварительно был развернут из манифеста https://raw.githubusercontent.com/kubernetes/dashboard/v2.2.0/aio/deploy/recommended.yaml *

# Задание 6. Что сделал
1. Зарегистрировался на GCP. Получил облако
2. Создал кластер kubernetes с помощью инстурмента gcloud
3. Установил helm 3 и сконфигурировал репозитории
4. Установил на кластер k8s nginx-ingress
5. Установил на кластер k8s cert-manager. Создал ClusterIssuer
6. Установил на кластер k8s chartmuseum с кастомизированным values.yaml. Убедился, что сервис работает зайдя на https://chartmuseum.34.88.81.253.nip.io
7. Установил на кластер k8s harbor с кастомизированным values.yaml (дополнительно отключил notary). Проверил, что harbor работает, залогинившись под дефолтными учетными данными на https://harbor.34.88.81.253.nip.io/
8. Создал helm chart hipster-shop
9. Выделил из hipster-shop сервис frontend. Разделил описатели на отдельные файлы шаблонов. Создал values.yaml, в котором перечислил настраиваемые свойства шаблонов. Добавил frontend в зависимости к hipster-shop
10. Выделил из hipster-shop два сервиса paymentservice и shippingservice в kubecfg. Создал services.jsonnet
11. Выделил из hipster-shop сервис adservice в kustomize. Создал kustomization.yaml для окружения hipster-shop и hipster-shop-prod

## chartmuseum | Задание со *
Загрузить chart можно с помощью выполнения http запросов к chartmuseum, например, с помощью curl
```
curl --data-binary "@mychart-0.1.0.tgz" {CHARTMUSEUM_BASE_URL}/api/charts
```
либо использовать helm-push plugin:
```
helm push mychart/ chartmuseum
```
Для использования chartmuseum в качестве репозитория чартов необходимо добавить его в список репозиториев:
```
helm repo add chartmuseum {CHARTMUSEUM_BASE_URL}
```
Для установки чартов из chartmuseum:
```
helm install chartmuseum/mychart --generate-name
```
