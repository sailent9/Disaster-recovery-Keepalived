Задание 1

Дана схема для Cisco Packet Tracer, рассматриваемая в лекции.
На данной схеме уже настроено отслеживание интерфейсов маршрутизаторов Gi0/1 (для нулевой группы)
Необходимо аналогично настроить отслеживание состояния интерфейсов Gi0/0 (для первой группы).
Для проверки корректности настройки, разорвите один из кабелей между одним из маршрутизаторов и Switch0 и запустите ping между PC0 и Server0.
На проверку отправьте получившуюся схему в формате pkt и скриншот, где виден процесс настройки маршрутизатора.


Решение 1
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


Задание 2

Запустите две виртуальные машины Linux, установите и настройте сервис Keepalived как в лекции, используя пример конфигурационного файла.
Настройте любой веб-сервер (например, nginx или simple python server) на двух виртуальных машинах
Напишите Bash-скрипт, который будет проверять доступность порта данного веб-сервера и существование файла index.html в root-директории данного веб-сервера.
Настройте Keepalived так, чтобы он запускал данный скрипт каждые 3 секунды и переносил виртуальный IP на другой сервер, если bash-скрипт завершался с кодом, 
отличным от нуля
(то есть порт веб-сервера был недоступен или отсутствовал index.html). Используйте для этого секцию vrrp_script
На проверку отправьте получившейся bash-скрипт и конфигурационный файл keepalived, 
а также скриншот с демонстрацией переезда плавающего ip на другой сервер в случае недоступности порта или файла index.htm
