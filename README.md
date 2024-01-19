Домашнее задание по занятости " «Disaster recovery и Keepalived»" -Тихун Вадим  - SYS-25
-----------------------------


Задание 1
=
Дана схема для Cisco Packet Tracer, рассматриваемая в лекции.
На данной схеме уже настроено отслеживание интерфейсов маршрутизаторов Gi0/1 (для нулевой группы)
Необходимо аналогично настроить отслеживание состояния интерфейсов Gi0/0 (для первой группы).
Для проверки корректности настройки, разорвите один из кабелей между одним из маршрутизаторов и Switch0 и запустите ping между PC0 и Server0.
На проверку отправьте получившуюся схему в формате pkt и скриншот, где виден процесс настройки маршрутизатора.


Решение 1
=
![1 1](https://github.com/sailent9/Disaster-recovery-Keepalived/assets/130309754/f552ddcc-38b6-4b81-97a3-7695fc986437)
![1 2](https://github.com/sailent9/Disaster-recovery-Keepalived/assets/130309754/88c33fd7-79cc-4aad-a1b3-90866d6bf9aa)
![1 3](https://github.com/sailent9/Disaster-recovery-Keepalived/assets/130309754/8b432dce-6a02-458f-a678-419cea157e38)
![1 4](https://github.com/sailent9/Disaster-recovery-Keepalived/assets/130309754/fae60962-6b6b-438e-8263-60dfddad5eed)

команды 
```
Router0>en
Router0#conf t
Router0(config)#interface gigabitEthernet 0/1
Router0(config-if)#standby 1 track gigabitEthernet 0/0
Router0(config-if)#standby preempt
Router0(config-if)#exit
Router0(config)#exit
Router0# 
```
ссылка на файл Cisko
https://github.com/sailent9/Disaster-recovery-Keepalived/blob/main/TIkhun.pkt




Задание 2
=

Запустите две виртуальные машины Linux, установите и настройте сервис Keepalived как в лекции, используя пример конфигурационного файла.
Настройте любой веб-сервер (например, nginx или simple python server) на двух виртуальных машинах
Напишите Bash-скрипт, который будет проверять доступность порта данного веб-сервера и существование файла index.html в root-директории данного веб-сервера.
Настройте Keepalived так, чтобы он запускал данный скрипт каждые 3 секунды и переносил виртуальный IP на другой сервер, если bash-скрипт завершался с кодом, 
отличным от нуля
(то есть порт веб-сервера был недоступен или отсутствовал index.html). Используйте для этого секцию vrrp_script
На проверку отправьте получившейся bash-скрипт и конфигурационный файл keepalived, 
а также скриншот с демонстрацией переезда плавающего ip на другой сервер в случае недоступности порта или файла index.htm



Решение 2
=

я установил сервер NGINX. 
![2-1](https://github.com/sailent9/Disaster-recovery-Keepalived/assets/130309754/c8a6ab81-439f-4b60-aa23-d897261d304d)
![2-2](https://github.com/sailent9/Disaster-recovery-Keepalived/assets/130309754/f686365d-2cd8-46d5-8ed3-b76bfb0fa5c7)

----------------------------------------------------

Создал скрипт проверки доступности порта и существования файла index.html.

----------------------------------------------------
```
#!/bin/bash
if [[ $(ss -lntup | grep LISTEN | grep :80) ]] && [[ -f /var/www/html/index.html ]]; then
        exit 0
else
        sudo systemctl stop keepalived
fi
```
---------------------------------------------------------

Добавил конфигурационный файл в сервис keepalived

-----------------------------------------------------------
![2-3](https://github.com/sailent9/Disaster-recovery-Keepalived/assets/130309754/a5065805-dae0-4fa9-914d-c6dd8b606b36)
![2-4](https://github.com/sailent9/Disaster-recovery-Keepalived/assets/130309754/2f728498-7b23-4d11-bf86-93ff199de154)


-----------------------------------


Проверяем в браузере и видим, что первый сервер 192.168.0.103откликается по плавающему IP-адресу 192.168.0.200.
![2-5](https://github.com/sailent9/Disaster-recovery-Keepalived/assets/130309754/d8a01aec-3413-4c03-867c-a0c87b834899)

--------------------------------------
![2-6](https://github.com/sailent9/Disaster-recovery-Keepalived/assets/130309754/fe51085b-7d82-4a10-ae3c-f2782110d442)
![2-7](https://github.com/sailent9/Disaster-recovery-Keepalived/assets/130309754/7f23d520-594a-4056-b823-781788bf6983)
Теперь попробуем отключить сервис keepalivedна первом сервере и проверим переключение между серверами. Вводим команду

```
sudo systemctl stop keepalived
```

и видно, что по IP-адресу 192.168.0.200 на первом окне отображается начальная страница второго (РЕЗЕРВНОГО) сервера.

![2-8](https://github.com/sailent9/Disaster-recovery-Keepalived/assets/130309754/c3059f37-7e35-4622-9cff-465924535050)
Так же происходит, что второй сервер получил плавающий IP-адрес и стал MASTER-сервером.
![2-9](https://github.com/sailent9/Disaster-recovery-Keepalived/assets/130309754/365b345a-2c84-4399-8122-1547375164c7)
![2-10](https://github.com/sailent9/Disaster-recovery-Keepalived/assets/130309754/09f11ac0-8e5a-4ddb-adca-9bfde9f07222)





