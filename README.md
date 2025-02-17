# Домашнее задание к занятию «Вычислительные мощности. Балансировщики нагрузки» Шарапат Виктор

### Подготовка к выполнению задания

1. Домашнее задание состоит из обязательной части, которую нужно выполнить на провайдере Yandex Cloud, и дополнительной части в AWS (выполняется по желанию). 
2. Все домашние задания в блоке 15 связаны друг с другом и в конце представляют пример законченной инфраструктуры.  
3. Все задания нужно выполнить с помощью Terraform. Результатом выполненного домашнего задания будет код в репозитории. 
4. Перед началом работы настройте доступ к облачным ресурсам из Terraform, используя материалы прошлых лекций и домашних заданий.

---
## Задание 1. Yandex Cloud 

**Что нужно сделать**

1. Создать бакет Object Storage и разместить в нём файл с картинкой:

 - Создать бакет в Object Storage с произвольным именем (например, _имя_студента_дата_).
 - Положить в бакет файл с картинкой.
 - Сделать файл доступным из интернета.
 
2. Создать группу ВМ в public подсети фиксированного размера с шаблоном LAMP и веб-страницей, содержащей ссылку на картинку из бакета:

 - Создать Instance Group с тремя ВМ и шаблоном LAMP. Для LAMP рекомендуется использовать `image_id = fd827b91d99psvq5fjit`.
 - Для создания стартовой веб-страницы рекомендуется использовать раздел `user_data` в [meta_data](https://cloud.yandex.ru/docs/compute/concepts/vm-metadata).
 - Разместить в стартовой веб-странице шаблонной ВМ ссылку на картинку из бакета.
 - Настроить проверку состояния ВМ.
 
3. Подключить группу к сетевому балансировщику:

 - Создать сетевой балансировщик.
 - Проверить работоспособность, удалив одну или несколько ВМ.
4. (дополнительно)* Создать Application Load Balancer с использованием Instance group и проверкой состояния.

Полезные документы:

- [Compute instance group](https://registry.terraform.io/providers/yandex-cloud/yandex/latest/docs/resources/compute_instance_group).
- [Network Load Balancer](https://registry.terraform.io/providers/yandex-cloud/yandex/latest/docs/resources/lb_network_load_balancer).
- [Группа ВМ с сетевым балансировщиком](https://cloud.yandex.ru/docs/compute/operations/instance-groups/create-with-balancer).

---
## Задание 2*. AWS (задание со звёздочкой)

Это необязательное задание. Его выполнение не влияет на получение зачёта по домашней работе.

**Что нужно сделать**

Используя конфигурации, выполненные в домашнем задании из предыдущего занятия, добавить к Production like сети Autoscaling group из трёх EC2-инстансов с  автоматической установкой веб-сервера в private домен.

1. Создать бакет S3 и разместить в нём файл с картинкой:

 - Создать бакет в S3 с произвольным именем (например, _имя_студента_дата_).
 - Положить в бакет файл с картинкой.
 - Сделать доступным из интернета.
2. Сделать Launch configurations с использованием bootstrap-скрипта с созданием веб-страницы, на которой будет ссылка на картинку в S3. 
3. Загрузить три ЕС2-инстанса и настроить LB с помощью Autoscaling Group.

Resource Terraform:

- [S3 bucket](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket)
- [Launch Template](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/launch_template).
- [Autoscaling group](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/autoscaling_group).
- [Launch configuration](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/launch_configuration).

Пример bootstrap-скрипта:

```
#!/bin/bash
yum install httpd -y
service httpd start
chkconfig httpd on
cd /var/www/html
echo "<html><h1>My cool web-server</h1></html>" > index.html
```
### Правила приёма работы

Домашняя работа оформляется в своём Git репозитории в файле README.md. Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.
Файл README.md должен содержать скриншоты вывода необходимых команд, а также скриншоты результатов.
Репозиторий должен содержать тексты манифестов или ссылки на них в файле README.md.

---

## Решение 1. Yandex Cloud

1) проверяю конфиг - terraform init.

2) запускаю выполнени  - terraform apply.

3) сделал вывод IP адресов (outputs.tf)

![image](https://github.com/user-attachments/assets/7c850603-9a5b-4f07-8262-79ab1e28b94a)

4) настроил утилиту yc, проверяю выполнение кода через cloud и yc

![image](https://github.com/user-attachments/assets/a2178cd0-8d55-465d-ba6e-7e1403f9ebd6)


![image](https://github.com/user-attachments/assets/b695e6b5-c3d7-4ec5-967c-c9c3c7b6a5b5)

![image](https://github.com/user-attachments/assets/0c57c517-dcf6-4882-bcd0-a81e962c5a64)

![image](https://github.com/user-attachments/assets/749c2dba-aa2d-4b0a-8e80-f1249454d21a)

![image](https://github.com/user-attachments/assets/e0254d2e-884a-49c1-a0e2-a132cee608fe)

![image](https://github.com/user-attachments/assets/01828455-7aed-4dcb-b254-95d4050005af)

![image](https://github.com/user-attachments/assets/2de89a32-3617-496a-8dd8-c305a03c417b)

![image](https://github.com/user-attachments/assets/c0432328-a2c6-4b44-8bb1-098c333d5c0d)

5) Проверяю работу Object Storage и Apache 2 на 80 порту на всех ВМ.

![image](https://github.com/user-attachments/assets/294e791c-55c1-4f63-a49f-1d6a27b55166)

![image](https://github.com/user-attachments/assets/5a953e21-4156-4ef5-82a2-05f0209ae883)

![image](https://github.com/user-attachments/assets/203ae002-1d72-4206-b646-99c1805a3d8b)

5) Проверяю работу балансировщика.

![image](https://github.com/user-attachments/assets/bbc280b0-3e65-4f2b-851b-de9802f52fc9)


6) Чтобы убедиться, что балансировщик работает (не в этот раз, но на этом коде terraform), я настроил подключение ко все ВМ по ssh, изменил страницу index.html на всех ВМ и через curl смотрел запросы.

![image](https://github.com/user-attachments/assets/e1ff6082-15b9-49e1-bc0c-42b2b03e0407)

видно, что запросы идут на разные ВМ.

7) Проверка Healthcheck ВМ (каждые 10 секунд балансировщик отправляет HTTP-запрос на виртуальные машины, если ответ не пришел в течение 5 секунд, проверка считается неудачной, в ином случае успешно.

Выключил 1 ВМ

![image](https://github.com/user-attachments/assets/f1b9418e-5073-453b-8549-66dfbb4ed111)

Балансировщик работает

![image](https://github.com/user-attachments/assets/52eff5cf-a691-47a4-9b9d-d08eb8a8ec48)

Современем ВМ восстанавливается

![image](https://github.com/user-attachments/assets/167ba825-a50e-47e4-8d82-47dee77bed24)

---
### P.S. Факультативный вопрос с предыдущего ДЗ: каким ещё образом помимо NAT gateway можно было организовать доступ в интернет?

Ответ: использовать Bastion Host, VPN, Прокси-сервер.

Bastion Host - создать промежуточную ВМ с публичным IP, через которую будет осуществляться доступ в интернет для других машин в приватной сети.

Настроить VPN-соединение между Yandex Cloud и внешней сетью.

Прокси-сервер - развернуть на ВМ прокси-сервер, через которые будет проходить трафик в интернет.




