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
   ## Поднимаем контейнеры MySql(БД) и phpmyadmin(ВЕБ) с созданной сетью Overlay: <br>
   docker service create --name Mysql_Db --replicas 1 -e MYSQL_ROOT_PASSWORD=JK72pt3q --network Db-Web mysql <br>
   docker service create --name phpMyadmin-Web --replicas 1 -e PMA_HOST=Mysql_Db -p 8080:80 --network Db-Web phpmyadmin <br>
   ![service_Web-Db](https://github.com/Antonyo891/LUContSem5HW/blob/master/ServiceDb-Web.png)
   ![phpMyadmin](https://github.com/Antonyo891/LUContSem5HW/blob/master/TestT1.1.png)


# 3) далее необходимо создать 3 сервиса в каждом окружении (dev, prod, lab)
# 4) по итогу на каждой ноде должно быть по 2 работающих контейнера
# 5) выводы зафиксировать

Задание 2*:
1) нужно создать 2 ДК-файла, в которых будут описываться сервисы
2) повторить задание 1 для двух окружений: lab, dev
3) обязательно проверить и зафиксировать результаты, чтобы можно было выслать преподавателю для проверки

Задание со звездочкой - повышенной сложности, это нужно учесть при выполнении (но сделать его необходимо).
