# Fault-tolerant website with load balancing using Yandex Application Load Balancer

Create and set up a website with [Application Load Balancer](https://cloud.yandex.ru/docs/application-load-balancer/concepts/)-assisted load balancing among three availability zones and fault tolerance in one zone.

With [Terraform](https://www.terraform.io/), you can quickly create a cloud infrastructure in Yandex Cloud and manage it with configuration files. These files store the infrastructure description written in HashiCorp Configuration Language (HCL). Terraform and its providers are distributed under the [Business Source License](https://github.com/hashicorp/terraform/blob/main/LICENSE).

For more information about the provider resources, see the documentation on the [Terraform](https://www.terraform.io/docs/providers/yandex/index.html) website or [its mirror](https://terraform-provider.yandexcloud.net/).

If you change the configuration files, Terraform automatically detects which part of your configuration is already deployed, and what should be added or removed.

To host a fault-tolerant load-balanced website in an instance group using Terraform:

1. [Install Terraform](https://cloud.yandex.ru/docs/tutorials/infrastructure-management/terraform-quickstart#install-terraform), [get the authentication credentials](https://cloud.yandex.ru/docs/tutorials/infrastructure-management/terraform-quickstart#get-credentials), and specify the source for installing the Yandex Cloud provider (see [Step 1: Configuring a provider](https://cloud.yandex.ru/docs/tutorials/infrastructure-management/terraform-quickstart#configure-provider)).
1. Prepare files with the infrastructure description:

    1. Clone the repository with configuration files:

        ```bash
        git clone https://github.com/yandex-cloud-examples/yc-terraform-alb-website.git
        ```

    1. Go to the directory with the repository. Make sure it contains the following files:
        * `application-load-balancer-website.tf`: New infrastructure configuration.
        * `application-load-balancer-website.auto.tfvars`: User data file.

        For more information about the parameters of resources used in Terraform, see the relevant provider documentation:
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

1. In the `application-load-balancer-website.auto.tfvars` file, set the following user-defined properties:

    * `folder_id`: [Folder ID](https://cloud.yandex.ru/docs/resource-manager/operations/folder/get-id).
    * `vm_user`: VM user name.
    * `ssh_key_path`: Path to the file with a public SSH key to authenticate the user on the VM. For more details, see [Creating an SSH key pair](https://cloud.yandex.ru/docs/compute/operations/vm-connect/ssh#creating-ssh-keys).
    * `domain`: Domain name, e.g., `alb-example.com`.

1. Create resources:

    1. In the terminal, go to the folder where you edited the configuration file.

    1. Make sure the configuration file is correct using this command:

        ```bash
        terraform validate
        ```
        
        If the configuration is correct, you will get this message:

        ```text
        Success! The configuration is valid.
        ```

    1. Run this command:

        ```bash
        terraform plan
        ```
        
        The terminal will display a list of resources with parameters. No changes will be made at this step. If there are errors in the configuration, Terraform will point them out.

    1. Apply the configuration changes:

        ```bash
        terraform apply
        ```

    1. Confirm the changes: type `yes` into the terminal and press **Enter**.

1. [Upload the website files](https://cloud.yandex.ru/docs/tutorials/web/application-load-balancer-website#upload-files).
1. [Run a fault tolerance test](https://cloud.yandex.ru/docs/tutorials/web/application-load-balancer-website#test-ha).
