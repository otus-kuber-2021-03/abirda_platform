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
4. С помощью указания свойств maxUnavailable и maxSurge создал два манифеста Deployment приложения paymentservice, которые реализауют сценарии развертывания 'blue-green' и 'Reverse Rolling Update' *
5. В frontend-deployment.yaml добавил проверку того, что приложение запущенно, с помощью readinessProbe
6. Создал манифест Daemonset приложения node-exporter, который при применении запускается как на worker, так и на master нодах **

### Почему обновление ReplicaSet не повлекло обновление запущенных pod?
* ReplicaSet не проверяет соответствие запущенных подов примененному шаблону, поэтому при применении шаблона не произошел перезапуск подов
* Deployment же, в свою очередь, сверяет шаблон с описанием запущенных подов, поэтому и пересоздаст эти поды
