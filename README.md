# Disaster-recovery-Keepalived
«Disaster recovery и Keepalived» Васяева Ирина
# Задание 1
1. Настройка маршрутизатора0 <br/>
![image](https://github.com/user-attachments/assets/bc622a9d-cd18-40c2-acc7-69bca0da0119) <br/>
2. Настройка маршрутизатора1 <br/>
![image](https://github.com/user-attachments/assets/772e0da5-2ab4-4bf9-a74b-a81a0eb5d701) <br/>
3. Настройка отслеживания (tracking) для интерфейса Gi0/0: <br/>
   - для маршрутизатора0 <br/>
![image](https://github.com/user-attachments/assets/909e5f48-e4db-4bf3-bd23-04db70fd46ec) <br/>
   - для маршрутизатора1 <br/>
![image](https://github.com/user-attachments/assets/f6130780-90c2-4f24-b1e2-a0459a7f15f1) <br/>
4. Проверка соединения PC0 и Server0 <br/>
   ping 192.168.1.50 <br/>
5. Мониторинг состояния отслеживания <br/>
   show track
# Задание 2
1. Установка и настройка веб-серверов (в обе вирнутальные машины ВМ1 и ВМ2)  <br/>
```bash
sudo apt update
sudo apt install nginx -y
```
Настройка index.html  <br/>
```bash
echo "<h1>Hello from $(hostname)</h1>" | sudo tee /var/www/html/index.html
```
2. Установка Keepalived  <br/>
```bash
sudo apt install keepalived -y
```
3. Настройка конфигурационного файла Keepalived  <br/>
Для ВМ1:  <br/>
```bash
! Configuration File

global_defs {
   router_id VM1
}

vrrp_instance VI_1 {
    state MASTER
    interface eth0    
    virtual_router_id 51
    priority 100     
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1234
    }
    virtual_ipaddress {
        192.168.1.100 
    }
    track_script {
        check_web
    }
}

vrrp_script check_web {
    script "/usr/local/bin/check_nginx.sh"
    interval 3
    weight -2
}
```
Для ВМ2:  <br/>
```bash
! Configuration File

global_defs {
   router_id VM2
}

vrrp_instance VI_1 {
    state BACKUP
    interface eth0
    virtual_router_id 51
    priority 90       # Приоритет для BACKUP
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1234
    }
    virtual_ipaddress {
        192.168.1.100
    }
    track_script {
        check_web
    }
}

vrrp_script check_web {
    script "/usr/local/bin/check_nginx.sh"
    interval 3
    weight -2
}
```
4. Написание Bash-скрипта для проверки веб-сервера  <br/>
   Создание Bash-скрипт `/usr/local/bin/check_nginx.sh`:  <br/>
```bash
#!/bin/bash
PORT=80
HOST=localhost
FILE="/var/www/html/index.html"
if ! nc -z $HOST $PORT; then
    exit 1
fi
if [ ! -f $FILE ]; then
    exit 1
fi
exit 0
```
Сделать скрипт исполняемым:
```bash
sudo chmod +x /usr/local/bin/check_nginx.sh
```
5. Запуск Keepalived
```bash
sudo systemctl start keepalived
sudo systemctl enable keepalived
```
6. Проверка работы
```bash
sudo systemctl stop nginx
```
