# Технология разработки ПО
# Лабораторная работа №2: создание кластера Kubernetes и деплой приложения
# Ходырева Ирина Альбертовна, группа 3mac2001
# Цель лабораторной работы:
Знакомство с кластерной архитектурой на примере Kubernetes и деплой приложения в кластер.

# Манифест deployment.yaml  
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  replicas: 5
  selector:
    matchLabels:
      app: my-app
  strategy:
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
    type: RollingUpdate
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - image: myapi:latest
          # https://medium.com/bb-tutorials-and-thoughts/how-to-use-own-local-doker-images-with-minikube-2c1ed0b0968
          # указыаает на то, что образы нужно брать только из локального registry. В продакшене никогда не использовать
          imagePullPolicy: Never 
          name: myapi
          ports:
            - containerPort: 8080
      hostAliases:
      - ip: "192.168.49.1" # The IP of your VM
        hostnames:
        - postgres.local
 ```
 
 # Манифест service.yaml
 ```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: NodePort
  ports:
    - nodePort: 31317
      port: 8080
      protocol: TCP
      targetPort: 8080
  selector:
    app: my-app
 
 ```

# Скриншот шага 3.3
![Снимок 3](https://github.com/Edan-ib/Kubernetes-lab2/blob/main/Screenshot%20at%20Nov%2013%2019-10-16.png)
# Скриншот шага 3.5
![Снимок 4](https://github.com/Edan-ib/Kubernetes-lab2/blob/main/Screenshot%20at%20Nov%2013%2019-32-30.png)
![Снимок 5](https://github.com/Edan-ib/Kubernetes-lab2/blob/main/Screenshot%20at%20Nov%2013%2019-33-28.png)

# Видео с обзором созданного кластера
[Ссылка на видео](https://yadi.sk/i/nhvfHMaTv3XQFg)
