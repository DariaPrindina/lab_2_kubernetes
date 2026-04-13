# 2 Лабораторная Kubernetes

## 1 часть

Использованные ресурсы Kubernetes:

- Namespace (lab2) — изоляция ресурсов лабораторной.
- ConfigMap (hello-html) — хранение index.html с текстом страницы.
- Deployment (hello-deployment) — запуск nginx Pod с монтированием index.html из ConfigMap.
- Service (hello-service, NodePort) - сетевой доступ к приложению извне кластера.

1. Запуск локального кластера Kubernetes

- Кластер Minikube запущен локально с драйвером Docker, нода перешла в состояние Ready.

![alt text](screenshots/image-1.png)

![alt text](screenshots/image-2.png)

### Пошаговый деплой манифестов для демонстрации этапов

2. Создание отдельного namespace для лабы

- Для изоляции ресурсов создан namespace lab2.

![alt text](screenshots/image-3.png)

3. Создание ConfigMap с HTML-страницей

- Создан ConfigMap hello-html в namespace lab2, в нем хранится index.html с текстом страницы.

![alt text](screenshots/image-4.png)

![alt text](screenshots/image-5.png)

4. Развертывание приложения через Deployment

- Развернут Deployment hello-deployment с контейнером nginx.
- Файл index.html из ConfigMap смонтирован в nginx как главная страница.
- Rollout завершился успешно.

![alt text](screenshots/image-6.png)

![alt text](screenshots/image-7.png)
![alt text](screenshots/image-8.png)

![alt text](screenshots/image-9.png)

5. Публикация приложения через Service (NodePort)

- Создан сервис hello-service типа NodePort, который направляет трафик на Pod приложения.

![alt text](screenshots/image-10.png)

![alt text](screenshots/image-11.png)

### Деплой манифестов одной командой

2. Деплой всех ресурсов одной командой

- Все манифесты можно развернуть одной командой:

```bash
kubectl apply -f manifests/
```

3. Проверка доступности сервиса

- Получен URL сервиса через Minikube.
- Страница успешно открывается в браузере, сервис работает.

![alt text](screenshots/image-12.png)

![alt text](image.png)

## 2 часть

1. Создание Helm chart

- Создан chart hello-chart командой helm create hello-chart
- В deployment.yaml добавлено монтирование index.html из ConfigMap.
- Настроены файлы values.yaml и шаблоны templates/configmap.yaml, templates/deployment.yaml, templates/service.yaml.

2. Проверка chart перед деплоем

![alt text](screenshots/image-13.png)
![alt text](screenshots/image-14.png)

3.  Первый деплой релиза в кластер

![alt text](screenshots/image-15.png)

4. Проверка созданных ресурсов

![alt text](screenshots/image-16.png)

![alt text](screenshots/image-17.png)

![alt text](screenshots/image-18.png)

![alt text](screenshots/image-19.png)
![alt text](screenshots/image-20.png)

![alt text](screenshots/image-21.png)

5. Обновление релиза

![alt text](screenshots/image-22.png)

6. Подтверждение апгрейда

![alt text](screenshots/image-23.png)

7. Проверка обновленной версии сервиса

![alt text](screenshots/image-24.png)

![alt text](screenshots/image-25.png)

## 3 причины, почему Helm удобнее

1. Helm дает параметризацию через values.yaml и --set, поэтому не нужно вручную править много YAML-манифестов при каждом изменении.

2. Helm управляет релизами: можно смотреть историю (helm history), обновлять (helm upgrade) и откатывать версии (helm rollback), что удобнее и безопаснее.

3. Helm упаковывает набор ресурсов в единый chart и позволяет разворачивать одинаковую конфигурацию в разных окружениях (dev/test/prod) одной командой.
