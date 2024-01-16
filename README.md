Урок 5. Docker Compose и Docker Swarm
Задание 1:
# 1) создать сервис, состоящий из 2 различных контейнеров: 1 - веб, 2 - БД. <br>
  ## создадим docker-compose.yaml файл с двумя сервисами db - mysql и web - phpmyadmin: <br>
   ![yaml](https://github.com/Antonyo891/LUContSem5HW/blob/master/yaml11.png)
   ## необходимо перейти в директорию с файлом .yaml и выполнить команду docker compose up -d
   ![docker-compose1](https://github.com/Antonyo891/LUContSem5HW/blob/master/DC1.2.png)
   ![docker-compose1](https://github.com/Antonyo891/LUContSem5HW/blob/master/DC1.4.png)
   ![docker-compose1](https://github.com/Antonyo891/LUContSem5HW/blob/master/DC1.3.png)
# 2) далее необходимо создать 3 сервиса в каждом окружении (dev, prod, lab) <br>
## На рег.ру созданы три виртуальных сервера для создания общего кластера.
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
   ## На сервере-менеджере создаем 3 Overlay сети для работы контейнеров БД и ВЕБ: <br>
   root@cv3512249:~# docker network create --driver overlay Db-Web --attachable <br>
   jtsspam2g7z1myx3tgvhsau63 <br>
   root@cv3512249:/home# docker network create --driver overlay Db-Web1 --attachable <br>
   ihdrx9uvigxx6z0gag1e81303 <br>
   root@cv3512249:/home# docker network create --driver overlay Db-Web2 --attachable <br>
   7b6yzxnee1ywgla433zofooso <br>
   ![net_Db-Web](https://github.com/Antonyo891/LUContSem5HW/blob/master/Net1.png)
   ## Поднимаем контейнеры MySql(БД) и phpmyadmin(ВЕБ) с тремя разными сетями Overlay (Db-Web, Db-Web1, Db-Web2) в нодах (чтобы убедиться что сервисы не связаны будем менять пароль для ROOT в MySql и Phpmyadmin будет слушать разные порты): <br>
   docker service create --name Mysql_Db --replicas 1 -e MYSQL_ROOT_PASSWORD=***JK72pt3q*** --network Db-Web mysql <br>
   docker service create --name phpMyadmin-Web --replicas 1 -e PMA_HOST=Mysql_Db -p 8080:80 --network Db-Web phpmyadmin <br>
   docker service create --name Mysql_Db1 --replicas 1 -e MYSQL_ROOT_PASSWORD=JK72pt3 --network Db-Web1 mysql <br>
   docker service create --name phpMyadmin-Web1 --replicas 3 -e PMA_HOST=Mysql_Db1 -p 8081:80 --network Db-Web1 phpmyadmin <br>
   docker service create --name Mysql_Db2 --replicas 1 -e MYSQL_ROOT_PASSWORD=JK72pt --network Db-Web2 mysql <br>
   docker service create --name phpMyadmin-Web2 --replicas 1 -e PMA_HOST=Mysql_Db2 -p 8082:80 --network Db-Web2 phpmyadmin <br>
   
   ![service_Web-Db](https://github.com/Antonyo891/LUContSem5HW/blob/master/ServiceDb-Web.png)
   На порте 8080 должен быть пароль для root JK72pt3q (JK72pt3 и JK72pt не пускают) 
   ![phpMyadmin](https://github.com/Antonyo891/LUContSem5HW/blob/master/TestT1.1.png)
   ![phpMyadminVerificated](https://github.com/Antonyo891/LUContSem5HW/blob/master/TestT1.1.2.png)
   ![phpMyadminVerificated1](https://github.com/Antonyo891/LUContSem5HW/blob/master/MSQL1.png)
   ![phpMyadminVerificated1](https://github.com/Antonyo891/LUContSem5HW/blob/master/MSQL2.png)
# 3) по итогу на каждой ноде должно быть по 2 работающих контейнера <br>
   ## В итоге на 2 нодах по 3 контейнера, на 1 ноде -2 контейнера <br>
   ![manager-3-cont](https://github.com/Antonyo891/LUContSem5HW/blob/master/manager3conte.png)
   ![worker-3cont](https://github.com/Antonyo891/LUContSem5HW/blob/master/woker2cont.png)
   ![worker-2cont](https://github.com/Antonyo891/LUContSem5HW/blob/master/woker3cont.png)
# 4) выводы зафиксировать (все есть)<br>

# Задание 2*: <br> 
# 1) нужно создать 2 ДК-файла, в которых будут описываться сервисы <br>
## Инициализируем кластер:<br>
root@cv3512249:~# docker swarm init
Swarm initialized: current node (vqkkyzq6s8qv7hsmm9yihqsoa) is now a manager.
To add a worker to this swarm, run the following command:
  docker swarm join --token SWMTKN-1-00x4nk7b5xjwbw61vreajmc7mur172z4398sjdyyrzt6c8i28s-3fim5e4mugcdtrrguwtjw1l2p 192.168.0.160:2377
To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
## и присоединяем ноды через docker swarm join ...
![nodes2](https://github.com/Antonyo891/LUContSem5HW/blob/master/NodesT2.png)
## создаем два файла yaml.
![Dev.yaml](https://github.com/Antonyo891/LUContSem5HW/blob/master/devYaml%201.png)
![Lab.yaml](https://github.com/Antonyo891/LUContSem5HW/blob/master/labYaml%201.png)
# 2) повторить задание 1 для двух окружений: lab, dev
## создаем сервисы: <br> 
docker stack depoy -c /home/lab.yaml lab
docker stack deploy -c /home/dev.yaml dev
![dev_stack](https://github.com/Antonyo891/LUContSem5HW/blob/master/devstack.png)
## в общем если stack по очереди разворачивать, тот что  первый развернут работает, при развораивании второго ошибок не пишеи, но mysql постоянно падает и перезапускается в итогк сервер зависает....ничего сделать не смог... 
# 3) обязательно проверить и зафиксировать результаты, чтобы можно было выслать преподавателю для проверки

Задание со звездочкой - повышенной сложности, это нужно учесть при выполнении (но сделать его необходимо).
