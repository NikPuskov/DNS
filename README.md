# DNS

---------------------------------------------------------------------------------------------------------------------------

Цель домашнего задания:

Создать домашнюю сетевую лабораторию. Изучить основы DNS, научиться работать с технологией Split-DNS в Linux-based системах

---------------------------------------------------------------------------------------------------------------------------

Описание домашнего задания:

1. взять стенд https://github.com/erlong15/vagrant-bind 

  - добавить еще один сервер client2

  - завести в зоне dns.lab имена:

    a) web1 - смотрит на клиент1

    b) web2  смотрит на клиент2

  - завести еще одну зону newdns.lab

  - завести в ней запись

    a) www - смотрит на обоих клиентов

2. настроить split-dns

  - клиент1 - видит обе зоны, но в зоне dns.lab только web1

  - клиент2 видит только dns.lab

-----------------------------------------------------------------------------------------------------------------------------

Выполнение домашнего задания:

После развёртывания стенда из Vagrantfile будут сделаны все необходимые настройки для успешного выполнения заданий (с помощью Ansible).

Проверка на client:

`ping www.newdns.lab`

`ping web1.dns.lab`

`ping web2.dns.lab`

Видим, что client видит обе зоны (dns.lab и newdns.lab), однако информацию о хосте web2.dns.lab он получить не может.

Проверка на client2:

`ping www.newdns.lab`

`ping web1.dns.lab`

`ping web2.dns.lab`

Видим, что client2 видит всю зону dns.lab и не видит зону newdns.lab

--------------------------------------------------------------------------------------------------------------------------------

Для синхронизации зон между master и slave нужно:

- обновить SOA serial

- сделать `rndc reload` на master

- master отправит notify об изменении зоны

- slave запросит zone transfer. BIND не отслеживает изменения файлов. Это нормально, что он не делает этого автоматически. Нужно запускать перезагрузку rndc на ведущем устройстве после каждого изменения. Ведомое устройство не может заставить ведущего перезагрузить конфигурацию/зоны.
