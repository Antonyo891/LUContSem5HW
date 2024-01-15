Урок 5. Docker Compose и Docker Swarm
Задание 1:
# 1) создать сервис, состоящий из 2 различных контейнеров: 1 - веб, 2 - БД.
   ## На рег.ру созданы два облачных сервера для создания общего кластера.
    ![manager](https://github.com/Antonyo891/LUContSem5HW/blob/master/DSM1.png)
   ![worker](https://github.com/Antonyo891/LUContSem5HW/blob/master/DSW1.png)
   ## Работать с серверами буду через ssh из консоли ВМ Ubuntu (консоль удобнее).<br>
   ## Инициализируем кластер:<br>
   oot@cv3512249:~# docker swarm init <br>
   Swarm initialized: current node (perjao30grrnee4h0un8o4hbk) is now a manager.<br>
   To add a worker to this swarm, run the following command:<br>
   docker swarm join --token SWMTKN-1-5cfd2a8yzd715cwnyyrkbz1h1rw3hjafadxe84b06l5cqz9a9c-8yaliboeorh2g54y0tpwen9wg 192.168.0.160:2377<br>
   To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.<br>
   ![swarm_join](https://github.com/Antonyo891/LUContSem5HW/blob/master/DS2.png)
   ## На сервере-менеджере создаем Overlay сеть для работы контейнеров БД и ВЕБ: <br>
   root@cv3512249:~# docker network create --driver overlay Db-Web --attachable <br>
   jtsspam2g7z1myx3tgvhsau63 <br>
   ## Поднимаем контейнеры MySql(БД) и phpmyadmin(ВЕБ) с созданной сетью Overlay в нодах: <br>
   docker service create --name Mysql_Db --replicas 1 -e MYSQL_ROOT_PASSWORD=JK72pt3q --network Db-Web mysql <br>
   docker service create --name phpMyadmin-Web --replicas 1 -e PMA_HOST=Mysql_Db -p 8080:80 --network Db-Web phpmyadmin <br>
   ![service_Web-Db](https://github.com/Antonyo891/LUContSem5HW/blob/master/ServiceDb-Web.png)
   ![phpMyadmin](https://github.com/Antonyo891/LUContSem5HW/blob/master/TestT1.1.png)
   ![phpMyadminVerificated](https://github.com/Antonyo891/LUContSem5HW/blob/master/TestT1.1.2.png)
# 2) далее необходимо создать 3 сервиса в каждом окружении (dev, prod, lab) <br>
## Создадим 3 сервиса состоящих из nginx и openjdk <br>
   ## создаем 3 Overlay сети (net_service_1,net_service_2,net_service_3) <br>
oot@cv3512249:/home# docker network create --driver overlay net_service_1 --attachable
ly2zylbqirl1uluouswcjmroz
root@cv3512249:/home# docker network create --driver overlay net_service_2 --attachable
3jrcr1flpdic9cr3ssid5h2hz
root@cv3512249:/home# docker network create --driver overlay net_service_3 --attachable
hxfttelo0ril4bdliqyy5ox6d
## Поднимаем контейнеры nginx и openjdk с созданными сетями Overlay в нодах
oot@cv3512249:/home# docker service create --name service1_nginx --replicas 1 -p 6080:80 --network net_service_1 nginx
9z8cus60pofw0rs3ys2hzw8f8
overall progress: 1 out of 1 tasks 
1/1: running   [==================================================>] 
verify: Service converged 
root@cv3512249:/home# docker service create --name service1_nginx --replicas 1 -p 6081:80 --network net_service_2 nginx
Error response from daemon: rpc error: code = AlreadyExists desc = name conflicts with an existing object: service service1_nginx already exists
root@cv3512249:/home# docker service create --name service2_nginx --replicas 1 -p 6081:80 --network net_service_2 nginx
dlz6gc085pg42pg4uhasqo487
overall progress: 1 out of 1 tasks 
1/1: running   [==================================================>] 
verify: Service converged 
root@cv3512249:/home# docker service create --name service3_nginx --replicas 1 -p 6082:80 --network net_service_3 nginx
j4v8z3c4wxgt7tpxi6y8y8dfl
overall progress: 1 out of 1 tasks 
1/1: running   [==================================================>] 
verify: Service converged 


   
# 3) по итогу на каждой ноде должно быть по 2 работающих контейнера <br>
# 4) выводы зафиксировать <br>

# Задание 2*: <br> 
# 1) нужно создать 2 ДК-файла, в которых будут описываться сервисы <br>
# 2) повторить задание 1 для двух окружений: lab, dev
# 3) обязательно проверить и зафиксировать результаты, чтобы можно было выслать преподавателю для проверки

Задание со звездочкой - повышенной сложности, это нужно учесть при выполнении (но сделать его необходимо).
