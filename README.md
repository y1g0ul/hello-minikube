```text
$$\   $$\           $$\ $$\                               $$\           $$\ $$\                 $$\                 
$$ |  $$ |          $$ |$$ |                              \__|          \__|$$ |                $$ |                
$$ |  $$ | $$$$$$\  $$ |$$ | $$$$$$\        $$$$$$\$$$$\  $$\ $$$$$$$\  $$\ $$ |  $$\ $$\   $$\ $$$$$$$\   $$$$$$\  
$$$$$$$$ |$$  __$$\ $$ |$$ |$$  __$$\       $$  _$$  _$$\ $$ |$$  __$$\ $$ |$$ | $$  |$$ |  $$ |$$  __$$\ $$  __$$\ 
$$  __$$ |$$$$$$$$ |$$ |$$ |$$ /  $$ |      $$ / $$ / $$ |$$ |$$ |  $$ |$$ |$$$$$$  / $$ |  $$ |$$ |  $$ |$$$$$$$$ |
$$ |  $$ |$$   ____|$$ |$$ |$$ |  $$ |      $$ | $$ | $$ |$$ |$$ |  $$ |$$ |$$  _$$<  $$ |  $$ |$$ |  $$ |$$   ____|
$$ |  $$ |\$$$$$$$\ $$ |$$ |\$$$$$$  |      $$ | $$ | $$ |$$ |$$ |  $$ |$$ |$$ | \$$\ \$$$$$$  |$$$$$$$  |\$$$$$$$\ 
\__|  \__| \_______|\__|\__| \______/       \__| \__| \__|\__|\__|  \__|\__|\__|  \__| \______/ \_______/  \_______|
```                                                                                                                  
                                                                                                                    
                                                                                                                    
                                                                                                   
Flask-приложение, упакованное в Docker-контейнер и развёрнутое в локальном Kubernetes-кластере Minikube.

Приложение работает на порту 32777.

## Структура проекта

```text
.
├── k8s/
│   ├── deployment.yaml
│   └── service.yaml
├── docs/
│   ├── architecture.drawio
│   └── screenshots/
├── app.py
├── Dockerfile
└── requirements.txt
```

## Приложение

Приложение написано на Python с использованием Flask.

При обращении к корневому пути `/` возвращает:

```text
Hello World
```

Локальный запуск:

```bash
pip install -r requirements.txt
python app.py
```

После запуска приложение доступно по адресу:

```text
http://localhost:32777
```

## Docker

Сборка образа:

```bash
docker build -t hello-minikube:latest .
```

Запуск контейнера:

```bash
docker run --rm -p 32777:32777 hello-minikube:latest
```

Docker-образ опубликован в Docker Hub:

```text
y1g0ul/hello-minikube:latest
```

Скачать образ можно командой:

```bash
docker pull y1g0ul/hello-minikube:latest
```

## Kubernetes

Для локального Kubernetes-кластера используется Minikube.

Запуск кластера:

```bash
minikube start --driver=docker
```

Проверка состояния:

```bash
minikube status
kubectl get nodes
```

Развёртывание приложения:

```bash
kubectl apply -f k8s/deployment.yaml
```

Deployment создаёт две реплики приложения.

Проверка:

```bash
kubectl get deployments
kubectl get pods
```

Создание Service:

```bash
kubectl apply -f k8s/service.yaml
```

Проверка:

```bash
kubectl get services
```

Service имеет тип NodePort и направляет запросы на порт 32777 контейнеров приложения.

## Доступ к приложению

Получить адрес Service:

```bash
minikube service hello-minikube-service --url
```

Или открыть приложение в браузере:

```bash
minikube service hello-minikube-service
```

## Архитектура

```text
Browser
   |
   v
NodePort Service
   |
   +----------------+
   |                |
   v                v
Pod 1              Pod 2
   |                |
   v                v
Container          Container
Flask :32777       Flask :32777
```

Deployment следит за тем, чтобы в кластере постоянно работали две реплики приложения.

Service выбирает Pod по label `app: hello-minikube` и предоставляет единую точку доступа к ним.
