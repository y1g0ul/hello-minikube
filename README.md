# Hello Minikube

Flask-приложение, упакованное в Docker и развёрнутое в локальном Kubernetes-кластере Minikube.

Приложение работает на порту `32777` и возвращает ASCII Hello World.

Docker image:

```text
y1g0ul/hello-minikube:v1.0.0
```

## Архитектура

<p align="center">
  <img src="docs/architecture.png" alt="Architecture">
</p>

Deployment поддерживает две реплики приложения.

Service типа NodePort выбирает Pod по label `app: hello-minikube` и предоставляет к ним единую точку доступа.

## Запуск

Запустить Minikube:

```bash
minikube start --driver=docker
```

Развернуть приложение:

```bash
kubectl apply -f k8s/
```

Проверить состояние:

```bash
kubectl get deployments
kubectl get pods
kubectl get services
```

Получить адрес приложения:

```bash
minikube service hello-minikube-service --url
```

Проверить ответ:

```bash
curl <service-url>
```

## Docker

Сборка образа:

```bash
docker build -t y1g0ul/hello-minikube:v1.0.0 .
```

Локальный запуск:

```bash
docker run --rm -p 32777:32777 y1g0ul/hello-minikube:v1.0.0
```

## Результат

<details>
<summary>Скриншоты</summary>

### Pods

![Pods](docs/screenshots/get-pods.png)

### Deployment и Service

![Deployment and Service](docs/screenshots/deployments-and-services.png)

### Ответ приложения

![Service response](docs/screenshots/service-response.png)

</details>
