# Отказоустойчивый сайт с балансировкой нагрузки с помощью Yandex Application Load Balancer

Создайте и настройте веб-сайт с балансировкой нагрузки через [Application Load Balancer](https://cloud.yandex.ru/docs/application-load-balancer/concepts/) между тремя зонами доступности, защищенный от сбоев в одной зоне.

[Terraform](https://www.terraform.io/) позволяет быстро создать облачную инфраструктуру в Yandex Cloud и управлять ею с помощью файлов конфигураций. В файлах конфигураций хранится описание инфраструктуры на языке HCL (HashiCorp Configuration Language). Terraform и его провайдеры распространяются под лицензией [Business Source License](https://github.com/hashicorp/terraform/blob/main/LICENSE).

Подробную информацию о ресурсах провайдера смотрите в документации на сайте [Terraform](https://www.terraform.io/docs/providers/yandex/index.html) или в [зеркале](https://terraform-provider.yandexcloud.net/).

При изменении файлов конфигураций Terraform автоматически определяет, какая часть вашей конфигурации уже развернута, что следует добавить или удалить.

Чтобы разместить в группе виртуальных машин отказоустойчивый сайт с балансировкой нагрузки через Application Load Balancer с помощью Terraform:

1. [Установите Terraform](https://cloud.yandex.ru/docs/tutorials/infrastructure-management/terraform-quickstart#install-terraform), [получите данные для аутентификации](https://cloud.yandex.ru/docs/tutorials/infrastructure-management/terraform-quickstart#get-credentials) и укажите источник для установки провайдера Yandex Cloud (раздел [Настройте провайдер](https://cloud.yandex.ru/docs/tutorials/infrastructure-management/terraform-quickstart#configure-provider), шаг 1).
1. Подготовьте файлы с описанием инфраструктуры:

    1. Склонируйте репозиторий с конфигурационными файлами:

        ```bash
        git clone https://github.com/yandex-cloud-examples/yc-terraform-alb-website.git
        ```

    1. Перейдите в директорию с репозиторием. В ней должны появиться файлы:
        * `application-load-balancer-website.tf` — конфигурация создаваемой инфраструктуры;
        * `application-load-balancer-website.auto.tfvars` — файл с пользовательскими данными.

        Более подробную информацию о параметрах используемых ресурсов в Terraform см. в документации провайдера:
        * [yandex_iam_service_account](https://terraform-provider.yandexcloud.net/Resources/iam_service_account)
        * [yandex_resourcemanager_folder_iam_member](https://terraform-provider.yandexcloud.net/Resources/resourcemanager_folder_iam_binding)
        * [yandex_vpc_network](https://terraform-provider.yandexcloud.net/Resources/vpc_network)
        * [yandex_vpc_subnet](https://terraform-provider.yandexcloud.net/Resources/vpc_subnet)
        * [yandex_vpc_security_group](https://terraform-provider.yandexcloud.net/Resources/vpc_security_group)
        * [yandex_compute_image](https://terraform-provider.yandexcloud.net/Resources/compute_image)
        * [yandex_compute_instance_group](https://terraform-provider.yandexcloud.net/Resources/compute_instance_group)
        * [yandex_alb_backend_group](https://terraform-provider.yandexcloud.net/Resources/alb_backend_group)
        * [yandex_alb_http_router](https://terraform-provider.yandexcloud.net/Resources/alb_http_router)
        * [yandex_alb_virtual_host](https://terraform-provider.yandexcloud.net/Resources/alb_virtual_host)
        * [yandex_alb_load_balancer](https://terraform-provider.yandexcloud.net/Resources/alb_load_balancer)
        * [yandex_dns_zone](https://terraform-provider.yandexcloud.net/Resources/dns_zone)
        * [yandex_dns_recordset](https://terraform-provider.yandexcloud.net/Resources/dns_recordset)

1. В файле `application-load-balancer-website.auto.tfvars` задайте пользовательские параметры:

    * `folder_id` — [идентификатор каталога](https://cloud.yandex.ru/docs/resource-manager/operations/folder/get-id).
    * `vm_user` — имя пользователя ВМ.
    * `ssh_key_path` — путь к файлу с открытым SSH-ключом для аутентификации пользователя на ВМ. Подробнее см. [Создание пары ключей SSH](https://cloud.yandex.ru/docs/compute/operations/vm-connect/ssh#creating-ssh-keys).
    * `domain` — название домена, например `alb-example.com`.

1. Создайте ресурсы:

    1. В терминале перейдите в папку, где вы отредактировали конфигурационный файл.

    1. Проверьте корректность конфигурационного файла с помощью команды:

        ```bash
        terraform validate
        ```
        
        Если конфигурация является корректной, появится сообщение:

        ```text
        Success! The configuration is valid.
        ```

    1. Выполните команду:

        ```bash
        terraform plan
        ```
        
        В терминале будет выведен список ресурсов с параметрами. На этом этапе изменения не будут внесены. Если в конфигурации есть ошибки, Terraform на них укажет.

    1. Примените изменения конфигурации:

        ```bash
        terraform apply
        ```

    1. Подтвердите изменения: введите в терминале слово `yes` и нажмите **Enter**.

1. [Загрузите файлы веб-сайта](https://cloud.yandex.ru/docs/tutorials/web/application-load-balancer-website#upload-files).
1. [Протестируйте отказоустойчивость](https://cloud.yandex.ru/docs/tutorials/web/application-load-balancer-website#test-ha).
