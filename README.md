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
