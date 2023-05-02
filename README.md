# VLAN
## Домашнее задание по теме _VLAN и LACP_  
> 1. в Office1 в тестовой подсети появляется сервера с доп интерфесами и адресами  
в internal сети testLAN:  
> - testClient1 - 10.10.10.254  
> - testClient2 - 10.10.10.254  
> - testServer1- 10.10.10.1   
> - testServer2- 10.10.10.1  
>
> 2. Равести вланами:  
testClient1 <-> testServer1  
testClient2 <-> testServer2  
>
> 3. Между centralRouter и inetRouter "пробросить" 2 линка (общая internal сеть)  
и объединить их в бонд, проверить работу c отключением интерфейсов

### Решение ДЗ.  
1. Развертывание автоматическое, при помощи _Vagrant+Ansbible_. Сделано по методичке с некоторыми изменениями. 
_Ansible_ использует роли и в _Vagrantfile_ я уменьшил оперативную память на хостах, иначе мой ноутбук не справлялся(   

2. Настройка _VLAN_ осуществляется при помощи конфигурационных файлов. После запуска всех машин проверяем _Vlan1_  
на _testClient1_ и _testServer1_ при помощи команды ```ping 10.10.10.254``` c _testClient1_:

![](https://github.com/Vitaliy7/VLAN/blob/main/screenshots/1.png?raw=true)

Как видно, влан поднялся.  
Теперь проверяем _Vlan2_ на _testClient2_ и _testServer2_.  Для этого на _testClient2_ запустим ```ping 10.10.10.254```  

![](https://github.com/Vitaliy7/VLAN/blob/main/screenshots/2.png?raw=true)

Убеждаемся, что и этот влан отработал корректно.  


3. Это задание также выполняется автоматически при развертывании виртуальных машин.  
Для проверки запустим ```ping  192.168.255.2``` на хосте _inetRouter_.  

![](https://github.com/Vitaliy7/VLAN/blob/main/screenshots/3.png?raw=true)

Видим, что обмен пакетами есть. Не останавливая пинг, подключаемся к хосту _centralRouter_ и выключаем там интерфейс _eth1_  
командой ```ip link set down eth1```. Пинг не пропал, из чего делаем вывод, что трафик пошел по другому (_eth2_) интерфейсу, то есть
_bonding_ работает.  

В результате выполнения у нас получилась такая схема сети:

![](https://github.com/Vitaliy7/VLAN/blob/main/screenshots/sheme.png?raw=true)
