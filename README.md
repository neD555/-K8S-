### Домашнее задание к занятию «Запуск приложений в K8S»
### Задание 1. Создать Deployment и обеспечить доступ к репликам приложения из другого Pod.
1.Создать Deployment приложения, состоящего из двух контейнеров — nginx и multitool. Решить возникшую ошибку.

2.После запуска увеличить количество реплик работающего приложения до 2.

3.Продемонстрировать количество подов до и после масштабирования.

4.Создать Service, который обеспечит доступ до реплик приложений из п.1.

5.Создать отдельный Pod с приложением multitool и убедиться с помощью curl, что из пода есть доступ до приложений из п.1.

### Ответ.
<img width="657" height="387" alt="01-deployment" src="https://github.com/user-attachments/assets/d179412a-a5d3-408d-9203-13ca4a503784" />
<img width="658" height="221" alt="02-pods-before-scale" src="https://github.com/user-attachments/assets/24db4828-24ac-4140-b05c-669095004393" />
<img width="656" height="300" alt="03-service" src="https://github.com/user-attachments/assets/514147ca-d85d-497e-9e7b-39527df695d7" />
<img width="522" height="127" alt="04-multitool-test-pod" src="https://github.com/user-attachments/assets/02116b68-2ec2-4a66-b683-320e42c27dd2" />
<img width="658" height="463" alt="05-curl-nginx" src="https://github.com/user-attachments/assets/c4ccdb4a-95d7-418a-8979-fb773db53429" />
<img width="658" height="156" alt="06-curl-multitool" src="https://github.com/user-attachments/assets/d2794b67-f9b4-4732-b7a5-277196ca11e4" />
<img width="524" height="174" alt="07-nslookup" src="https://github.com/user-attachments/assets/6e35966f-6813-4227-bb97-cd7293558cff" />

### Список команд:
Deployment создан:

kubectl get deployments

kubectl get pods -o wide
### До масштабирования.
kubectl get pods
### Масштабирование.
kubectl scale deployment nginx-multitool --replicas=2
### После масштабирования.
kubectl get pods
### Service.
kubectl get svc
### Проверка pod multitool-test.
kubectl get pods
### curl nginx.
kubectl exec -it multitool-test -- curl http://nginx-multitool-svc:9001
### curl multitool.
kubectl exec -it multitool-test -- curl http://nginx-multitool-svc:9002
### DNS.
kubectl exec -it multitool-test -- nslookup nginx-multitool-svc

### Задание 2. Создать Deployment и обеспечить старт основного контейнера при выполнении условий.
1.Создать Deployment приложения nginx и обеспечить старт контейнера только после того, как будет запущен сервис этого приложения.

2.Убедиться, что nginx не стартует. В качестве Init-контейнера взять busybox.

3.Создать и запустить Service. Убедиться, что Init запустился.

4.Продемонстрировать состояние пода до и после запуска сервиса.

### Ответ.

<img width="525" height="268" alt="дз2" src="https://github.com/user-attachments/assets/cbd6ba57-4d59-45d1-b518-4e20d3acf669" />
<img width="655" height="620" alt="дз2(1)" src="https://github.com/user-attachments/assets/49101826-5dad-4def-8ae5-d1876cf031db" />
<img width="653" height="1026" alt="дз2(2)" src="https://github.com/user-attachments/assets/713d98f2-0c19-40a4-b29f-d27918ec4901" />
<img width="656" height="1013" alt="дз2(3)" src="https://github.com/user-attachments/assets/456c0374-9965-44d7-be42-ae1a9ab40aaf" />

### Список команд:

### После создания Deployment, до создания Service.
kubectl get deployments

kubectl get pods
### Более подробный вывод pod до создания Service.
kubectl get pods -o wide

kubectl describe pod nginx-init-75b8896cdb-2bwvs
### Логи init container, где видно ожидание сервиса.
kubectl logs nginx-init-75b8896cdb-2bwvs -c wait-for-service
### После создания Service.
kubectl get svc
### Состояние pod после запуска Service.
kubectl get pods

kubectl get pods -o wide

kubectl describe pod nginx-init-75b8896cdb-2bwvs









