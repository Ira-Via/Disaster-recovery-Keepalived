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
1. Установка Keepalived
2. Настройка конфигурационного файла Keepalived
3. Написание Bash-скрипта для проверки веб-сервера
   Создайте Bash-скрипт /usr/local/bin/check_nginx.sh:
4. Запуск Keepalived
5. Проверка работы
# Задание 3*
