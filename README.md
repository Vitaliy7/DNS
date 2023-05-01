# DNS
## Домашнее задание по теме _DNS- настройка и обслуживание_  
> взять стенд _https://github.com/erlong15/vagrant-bind_  
добавить еще один сервер client2  
завести в зоне dns.lab имена:  
web1 - смотрит на клиент1  
web2 смотрит на клиент2  
завести еще одну зону newdns.lab  
завести в ней запись:  
www - смотрит на обоих клиентов  
настроить split-dns  
клиент1 - видит обе зоны, но в зоне dns.lab только web1  
клиент2 видит только dns.lab  
настроить все без выключения selinux*  

### Решение ДЗ  
Выполнение происходит автоматически, при помощи _Vagrant+Ansible_. Задание сделано согласно методичке, с небольшими изменениями.  
_Ansible.playbook_ разбит по ролям на каждую машину. 
После того, как все машины запустятся, для проверки выполнения ДЗ следует зайти на _client_ и выполнить ```dig @192.168.50.10 web1.dns.lab```  

![](https://github.com/Vitaliy7/DNS/blob/main/screenshots/1.png?raw=true)

В секции _ANSWER SECTION_ видно, что ответ от ДНС-сервера корректный. Поскольку у нас уже настроен _split-dns_, то с _client_  
информацию о хосте web2.dns.lab он получить не может, для этого следует зайти на _client2_ и выполнить  
```dig @192.168.50.11 web2.dns.lab```:  

![](https://github.com/Vitaliy7/DNS/blob/main/screenshots/2.png?raw=true)

Проверяем последнюю задачу. Из _client_ пингуем зоны по очереди:  

![](https://github.com/Vitaliy7/DNS/blob/main/screenshots/3.png?raw=true)

Как видно, _client_ видит обе зоны (_dns.lab_ и _newdns.lab_), однако информацию о хосте _web2.dns.lab_ он получить не может.

Теперь также на _client2_  

![](https://github.com/Vitaliy7/DNS/blob/main/screenshots/4.png?raw=true)

Из этого следует, что _client2_ видит всю зону _dns.lab_ и не видит зону _newdns.lab_
