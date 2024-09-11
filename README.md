# Отказоустойчивый сайт с балансировкой нагрузки с помощью Yandex Application Load Balancer

Создайте и настройте веб-сайт с балансировкой нагрузки через [Application Load Balancer](https://cloud.yandex.ru/docs/application-load-balancer/concepts/) между тремя зонами доступности, защищенный от сбоев в одной зоне.

Подготовка инфраструктуры для отказоустойчивого веб-сайта с балансировкой нагрузки с помощью L7-балансировщика описана в [практическом руководстве](https://yandex.cloud/ru/docs/tutorials/security/alb-with-ddos-protection/), необходимые для настройки конфигурационные файлы `alb-with-ddos-protection.tf` и `alb-with-ddos-protection.auto.tfvars` расположены в этом репозитории.