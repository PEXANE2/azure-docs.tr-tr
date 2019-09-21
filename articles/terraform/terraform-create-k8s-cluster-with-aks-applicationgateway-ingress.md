---
title: Azure Kubernetes hizmeti (AKS) ile giriş denetleyicisi olarak Application Gateway bir Kubernetes kümesi oluşturma
description: Azure Kubernetes hizmeti ile bir Kubernetes kümesinin giriş denetleyicisi olarak Application Gateway ile nasıl oluşturulacağını gösteren öğretici
services: terraform
ms.service: azure
keywords: terkform, DevOps, sanal makine, Azure, Kubernetes, giriş, uygulama ağ geçidi
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 09/20/2019
ms.openlocfilehash: 0373b254a900fd34232bb6863c93802fa7b51aab
ms.sourcegitcommit: f2771ec28b7d2d937eef81223980da8ea1a6a531
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71169958"
---
# <a name="create-a-kubernetes-cluster-with-application-gateway-ingress-controller-using-azure-kubernetes-service-and-terraform"></a>Azure Kubernetes hizmeti ve Terkform kullanarak Application Gateway giriş denetleyicisi ile bir Kubernetes kümesi oluşturma
[Azure Kubernetes hizmeti (AKS)](/azure/aks/) , barındırılan Kubernetes ortamınızı yönetir. AKS, kapsayıcı düzenleme uzmanlığı olmadan Kapsayıcılı uygulamaları dağıtmayı ve yönetmeyi hızlı ve kolay hale getirir. Ayrıca, kaynakları isteğe bağlı olarak sağlama, yükseltme ve ölçeklendirme işlemlerini uygulamalarınızı çevrimdışı duruma geçirmeden yaparak sürekliliği olan işlemlerin ve bakımların yükünü ortadan kaldırır.

Giriş denetleyicisi, Kubernetes Hizmetleri için ters proxy, yapılandırılabilir trafik yönlendirme ve TLS sonlandırma sağlayan bir yazılım parçasıdır. Kubernetes giriş kaynakları, bağımsız bir Kubernetes hizmeti için giriş kurallarını ve rotaları yapılandırmak üzere kullanılır. Bir giriş denetleyicisi ve giriş kuralları kullanarak, bir Kubernetes kümesinde trafiği birden çok hizmete yönlendirmek için tek bir IP adresi kullanılabilir. Yukarıdaki tüm işlevler Azure [Application Gateway](/azure/Application-Gateway/)tarafından sağlanır ve bu, Azure 'Da Kubernetes için Ideal bir giriş denetleyicisi oluşturur. 

Bu öğreticide, bir [Kubernetes](https://www.redhat.com/en/topics/containers/what-is-kubernetes) kümesi oluşturma konusunda, giriş denetleyicisi olarak Application Gateway ile aks kullanarak aşağıdaki görevleri nasıl gerçekleştireceğinizi öğreneceksiniz:

> [!div class="checklist"]
> * HCL (HashiCorp Language) ile Kubernetes kümesi tanımlama
> * Application Gateway kaynak oluşturmak için Terrayform kullanma
> * Terraform ve AKS ile Kubernetes kümesi oluşturma
> * kubectl aracıyla bir Kubernetes kümesinin kullanılabilirlik durumunu test etme

## <a name="prerequisites"></a>Önkoşullar

- **Azure aboneliği**: Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.

- **Teroyform 'U Yapılandır**: Bu makaledeki yönergeleri izleyin, [Terrayform ve Azure erişimini yapılandırma](/azure/virtual-machines/linux/terraform-install-configure)

- **Azure hizmet sorumlusu**: [Azure CLI ile bir Azure hizmet sorumlusu oluşturma](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest)konusunun **hizmet sorumlusu oluşturma** bölümündeki yönergeleri izleyin. AppID, displayName ve Password değerlerini göz önünde atın.
  - Aşağıdaki komutu çalıştırarak hizmet sorumlusunun nesne KIMLIĞINI aklınızda yapın

    ```azurecli
    az ad sp list --display-name <displayName>
    ```

## <a name="create-the-directory-structure"></a>Dizin yapısını oluşturma
İlk adım, bu alıştırmadaki Terraform yapılandırma dosyalarınızı barındıracak olan dizini oluşturmaktır.

1. [Azure portala](https://portal.azure.com) gidin.

1. [Azure Cloud Shell](/azure/cloud-shell/overview)'i açın. Önceden bir ortam seçmediyseniz **Bash** ortamını seçin.

    ![Cloud Shell istemi](./media/terraform-k8s-cluster-appgw-with-tf-aks/azure-portal-cloud-shell-button-min.png)

1. `clouddrive` dizinine geçin.

    ```bash
    cd clouddrive
    ```

1. `terraform-aks-k8s` adlı bir dizin oluşturun.

    ```bash
    mkdir terraform-aks-appgw-ingress
    ```

1. Dizinleri yeni dizinle değiştirin:

    ```bash
    cd terraform-aks-appgw-ingress
    ```

## <a name="declare-the-azure-provider"></a>Azure sağlayıcısını tanımlama
Azure sağlayıcısını tanımlayan Terraform yapılandırma dosyasını yapılandırın.

1. Cloud Shell'de `main.tf` adlı bir dosya oluşturun.

    ```bash
    vi main.tf
    ```

1. I tuşunu seçerek ekleme moduna geçin.

1. Aşağıdaki kodu düzenleyiciye yapıştırın:

    ```hcl
    provider "azurerm" {
        version = "~>1.18"
    }

    terraform {
        backend "azurerm" {}
    }
    ```

1. **Esc** tuşuna basarak ekleme modundan çıkın.

1. Dosyayı kaydedin ve aşağıdaki komutu girerek VI düzenleyicisini kapatın:

    ```bash
    :wq
    ```

## <a name="define-input-variables"></a>Giriş değişkenlerini tanımlama
Bu dağıtım için gerekli tüm değişkenleri listeleyen Terrayform yapılandırma dosyasını oluşturun.

1. Cloud Shell'de `variables.tf` adlı bir dosya oluşturun.

    ```bash
    vi variables.tf
    ```

1. I tuşunu seçerek ekleme moduna geçin.

1. Aşağıdaki kodu düzenleyiciye yapıştırın:
    
    ```hcl
    variable "resource_group_name" {
      description = "Name of the resource group already created."
    }
    
    variable "location" {
      description = "Location of the cluster."
    }
    
    variable "aks_service_principal_app_id" {
      description = "Application ID/Client ID  of the service principal. Used by AKS to manage AKS related resources on Azure like vms, subnets."
    }
    
    variable "aks_service_principal_client_secret" {
      description = "Secret of the service principal. Used by AKS to manage Azure."
    }
    
    variable "aks_service_principal_object_id" {
      description = "Object ID of the service principal."
    }
    
    variable "virtual_network_name" {
      description = "Virtual network name"
      default     = "aksVirtualNetwork"
    }
    
    variable "virtual_network_address_prefix" {
      description = "Containers DNS server IP address."
      default     = "15.0.0.0/8"
    }
    
    variable "aks_subnet_name" {
      description = "AKS Subnet Name."
      default     = "kubesubnet"
    }
    
    variable "aks_subnet_address_prefix" {
      description = "Containers DNS server IP address."
      default     = "15.0.0.0/16"
    }
    
    variable "app_gateway_subnet_address_prefix" {
      description = "Containers DNS server IP address."
      default     = "15.1.0.0/16"
    }
    
    variable "app_gateway_name" {
      description = "Name of the Application Gateway."
      default = "ApplicationGateway1"
    }
    
    variable "app_gateway_sku" {
      description = "Name of the Application Gateway SKU."
      default = "Standard_v2"
    }
    
    
    variable "app_gateway_tier" {
      description = "Tier of the Application Gateway SKU."
      default = "Standard_v2"
    }
    
    
    variable "aks_name" {
      description = "Name of the AKS cluster."
      default     = "aks-cluster1"
    }
    variable "aks_dns_prefix" {
      description = "Optional DNS prefix to use with hosted Kubernetes API server FQDN."
      default     = "aks"
    }
    
    
    variable "aks_agent_os_disk_size" {
      description = "Disk size (in GB) to provision for each of the agent pool nodes. This value ranges from 0 to 1023. Specifying 0 will apply the default disk size for that agentVMSize."
      default     = 40
    }
    
    variable "aks_agent_count" {
      description = "The number of agent nodes for the cluster."
      default     = 3
    }
    
    variable "aks_agent_vm_size" {
      description = "The size of the Virtual Machine."
      default     = "Standard_D3_v2"
    }
    
    variable "kubernetes_version" {
      description = "The version of Kubernetes."
      default     = "1.11.5"
    }
    
    variable "aks_service_cidr" {
      description = "A CIDR notation IP range from which to assign service cluster IPs."
      default     = "10.0.0.0/16"
    }
    
    variable "aks_dns_service_ip" {
      description = "Containers DNS server IP address."
      default     = "10.0.0.10"
    }
    
    variable "aks_docker_bridge_cidr" {
      description = "A CIDR notation IP for Docker bridge."
      default     = "172.17.0.1/16"
    }
    
    variable "aks_enable_rbac" {
      description = "Enable RBAC on the AKS cluster. Defaults to false."
      default     = "false"
    }
    
    variable "vm_user_name" {
      description = "User name for the VM"
      default     = "vmuser1"
    }
    
    variable "public_ssh_key_path" {
      description = "Public key path for SSH."
      default     = "~/.ssh/id_rsa.pub"
    }
    
    variable "tags" {
      type = "map"
    
      default = {
        source = "terraform"
      }
    }
    ```

## <a name="define-the-resources"></a>Kaynakları tanımlama 
Tüm kaynakları oluşturan Terrayform yapılandırma dosyası oluşturun. 

1. Cloud Shell'de `resources.tf` adlı bir dosya oluşturun.

    ```bash
    vi resources.tf
    ```

1. I tuşunu seçerek ekleme moduna geçin.

1. Aşağıdaki kod bloklarını düzenleyiciye yapıştırın:

    a. Hesaplanan değişkenler için yeniden kullanmak üzere bir Yereller bloğu oluşturun.

    ```hcl
    # # Locals block for hardcoded names. 
    locals {
        backend_address_pool_name      = "${azurerm_virtual_network.test.name}-beap"
        frontend_port_name             = "${azurerm_virtual_network.test.name}-feport"
        frontend_ip_configuration_name = "${azurerm_virtual_network.test.name}-feip"
        http_setting_name              = "${azurerm_virtual_network.test.name}-be-htst"
        listener_name                  = "${azurerm_virtual_network.test.name}-httplstn"
        request_routing_rule_name      = "${azurerm_virtual_network.test.name}-rqrt"
        app_gateway_subnet_name = "appgwsubnet"
    }
    ```

    b. Yeni Kullanıcı kimliği kaynak grubu için bir veri kaynağı oluşturun.

    ```hcl
    data "azurerm_resource_group" "rg" {
      name = "${var.resource_group_name}"
    }
    
    # User Assigned Idntities 
    resource "azurerm_user_assigned_identity" "testIdentity" {
      resource_group_name = "${data.azurerm_resource_group.rg.name}"
      location            = "${data.azurerm_resource_group.rg.location}"
    
      name = "identity1"
    
      tags = "${var.tags}"
    }
    ```

    c. Temel ağ kaynakları oluşturun.

    ```hcl
    resource "azurerm_virtual_network" "test" {
      name                = "${var.virtual_network_name}"
      location            = "${data.azurerm_resource_group.rg.location}"
      resource_group_name = "${data.azurerm_resource_group.rg.name}"
      address_space       = ["${var.virtual_network_address_prefix}"]
    
      subnet {
        name           = "${var.aks_subnet_name}"
        address_prefix = "${var.aks_subnet_address_prefix}" 
      }
    
      subnet {
        name           = "appgwsubnet"
        address_prefix = "${var.app_gateway_subnet_address_prefix}"
      }
    
      tags = "${var.tags}"
    }
    
    data "azurerm_subnet" "kubesubnet" {
      name                 = "${var.aks_subnet_name}"
      virtual_network_name = "${azurerm_virtual_network.test.name}"
      resource_group_name  = "${data.azurerm_resource_group.rg.name}"
    }
    
    data "azurerm_subnet" "appgwsubnet" {
      name                 = "appgwsubnet"
      virtual_network_name = "${azurerm_virtual_network.test.name}"
      resource_group_name  = "${data.azurerm_resource_group.rg.name}"
    }
    
    # Public Ip 
    resource "azurerm_public_ip" "test" {
      name                         = "publicIp1"
      location                     = "${data.azurerm_resource_group.rg.location}"
      resource_group_name          = "${data.azurerm_resource_group.rg.name}"
      public_ip_address_allocation = "static"
      sku                          = "Standard"
    
      tags = "${var.tags}"
    }
    ```

    d. Application Gateway kaynağı oluşturun.

    ```hcl
    resource "azurerm_application_gateway" "network" {
      name                = "${var.app_gateway_name}"
      resource_group_name = "${data.azurerm_resource_group.rg.name}"
      location            = "${data.azurerm_resource_group.rg.location}"
    
      sku {
        name     = "${var.app_gateway_sku}"
        tier     = "Standard_v2"
        capacity = 2
      }
    
      gateway_ip_configuration {
        name      = "appGatewayIpConfig"
        subnet_id = "${data.azurerm_subnet.appgwsubnet.id}"
      }
    
      frontend_port {
        name = "${local.frontend_port_name}"
        port = 80
      }
    
      frontend_port {
        name = "httpsPort"
        port = 443
      }
    
      frontend_ip_configuration {
        name                 = "${local.frontend_ip_configuration_name}"
        public_ip_address_id = "${azurerm_public_ip.test.id}"
      }
    
      backend_address_pool {
        name = "${local.backend_address_pool_name}"
      }
    
      backend_http_settings {
        name                  = "${local.http_setting_name}"
        cookie_based_affinity = "Disabled"
        port                  = 80
        protocol              = "Http"
        request_timeout       = 1
      }
    
      http_listener {
        name                           = "${local.listener_name}"
        frontend_ip_configuration_name = "${local.frontend_ip_configuration_name}"
        frontend_port_name             = "${local.frontend_port_name}"
        protocol                       = "Http"
      }
    
      request_routing_rule {
        name                       = "${local.request_routing_rule_name}"
        rule_type                  = "Basic"
        http_listener_name         = "${local.listener_name}"
        backend_address_pool_name  = "${local.backend_address_pool_name}"
        backend_http_settings_name = "${local.http_setting_name}"
      }
    
      tags = "${var.tags}"
    
      depends_on = ["azurerm_virtual_network.test", "azurerm_public_ip.test"]
    }
    ```

    e. Rol atamaları oluşturun.

    ```hcl
    resource "azurerm_role_assignment" "ra1" {
      scope                = "${data.azurerm_subnet.kubesubnet.id}"
      role_definition_name = "Network Contributor"
      principal_id         = "${var.aks_service_principal_object_id }"
    
      depends_on = ["azurerm_virtual_network.test"]
    }
    
    resource "azurerm_role_assignment" "ra2" {
      scope                = "${azurerm_user_assigned_identity.testIdentity.id}"
      role_definition_name = "Managed Identity Operator"
      principal_id         = "${var.aks_service_principal_object_id}"
      depends_on           = ["azurerm_user_assigned_identity.testIdentity"]
    }
    
    resource "azurerm_role_assignment" "ra3" {
      scope                = "${azurerm_application_gateway.network.id}"
      role_definition_name = "Contributor"
      principal_id         = "${azurerm_user_assigned_identity.testIdentity.principal_id}"
      depends_on           = ["azurerm_user_assigned_identity.testIdentity", "azurerm_application_gateway.network"]
    }
    
    resource "azurerm_role_assignment" "ra4" {
      scope                = "${data.azurerm_resource_group.rg.id}"
      role_definition_name = "Reader"
      principal_id         = "${azurerm_user_assigned_identity.testIdentity.principal_id}"
      depends_on           = ["azurerm_user_assigned_identity.testIdentity", "azurerm_application_gateway.network"]
    }
    ```

    f. Kubernetes kümesi oluşturun.

    ```hcl
    resource "azurerm_kubernetes_cluster" "k8s" {
      name       = "${var.aks_name}"
      location   = "${data.azurerm_resource_group.rg.location}"
      dns_prefix = "${var.aks_dns_prefix}"
    
      resource_group_name = "${data.azurerm_resource_group.rg.name}"
    
      linux_profile {
        admin_username = "${var.vm_user_name}"
    
        ssh_key {
          key_data = "${file(var.public_ssh_key_path)}"
        }
      }
    
      addon_profile {
        http_application_routing {
          enabled = false
        }
      }
    
      agent_pool_profile {
        name            = "agentpool"
        count           = "${var.aks_agent_count}"
        vm_size         = "${var.aks_agent_vm_size}"
        os_type         = "Linux"
        os_disk_size_gb = "${var.aks_agent_os_disk_size}"
        vnet_subnet_id  = "${data.azurerm_subnet.kubesubnet.id}"
      }
    
      service_principal {
        client_id     = "${var.aks_service_principal_app_id}"
        client_secret = "${var.aks_service_principal_client_secret}"
      }
    
      network_profile {
        network_plugin     = "azure"
        dns_service_ip     = "${var.aks_dns_service_ip}"
        docker_bridge_cidr = "${var.aks_docker_bridge_cidr}"
        service_cidr       = "${var.aks_service_cidr}"
      }
    
      depends_on = ["azurerm_virtual_network.test", "azurerm_application_gateway.network"]
      tags       = "${var.tags}"
    }
    
    ```

    Yukarıdaki kod kümenin adını, konumunu ve resource_group_name değerini belirler. Ayrıca kümeye erişmek için kullanılan tam etki alanı adının (FQDN) bir bölümünü oluşturan dns_prefix değeri de ayarlanır.

    **linux_profile** kaydı, SSH kullanarak çalışan düğümlerinde oturum açmanızı sağlayan ayarları yapılandırmanızı sağlar.

    AKS ile yalnızca çalışan düğümleri için ödeme yaparsınız. **agent_pool_profile** kaydı bu çalışan düğümlerinin ayrıntılarını yapılandırmanızı sağlar. **agent_pool_profile record** ile oluşturulacak çalışan düğümü sayısı ve çalışan düğümlerinin türü belirtilir. İleride kümenin ölçeğini artırmanız veya azaltmanız gerekirse bu kaydın içindeki **count** değerini değiştirebilirsiniz.

1. **Esc** tuşuna basarak ekleme modundan çıkın.

1. Dosyayı kaydedin ve aşağıdaki komutu girerek VI düzenleyicisini kapatın:

    ```bash
    :wq
    ```


## <a name="create-a-terraform-output-file"></a>Terraform çıkış dosyası oluşturma
[Terraform çıkışları](https://www.terraform.io/docs/configuration/outputs.html), Terraform bir plan uyguladığında kullanıcı için vurgulanabilecek değerleri tanımlamanızı sağlar. Bu değerler `terraform output` komutuyla sorgulanabilir. Bu bölümde [kubectl](https://kubernetes.io/docs/reference/kubectl/overview/) ile kümeye erişmenizi sağlayan bir çıkış dosyası oluşturacaksınız.

1. Cloud Shell'de `output.tf` adlı bir dosya oluşturun.

    ```bash
    vi output.tf
    ```

1. I tuşunu seçerek ekleme moduna geçin.

1. Aşağıdaki kodu düzenleyiciye yapıştırın:

    ```hcl
    output "client_key" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config.0.client_key}"
    }

    output "client_certificate" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config.0.client_certificate}"
    }

    output "cluster_ca_certificate" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config.0.cluster_ca_certificate}"
    }

    output "cluster_username" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config.0.username}"
    }

    output "cluster_password" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config.0.password}"
    }

    output "kube_config" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config_raw}"
    }

    output "host" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config.0.host}"
    }
    ```

1. **Esc** tuşuna basarak ekleme modundan çıkın.

1. Dosyayı kaydedin ve aşağıdaki komutu girerek VI düzenleyicisini kapatın:

    ```bash
    :wq
    ```

## <a name="set-up-azure-storage-to-store-terraform-state"></a>Terraform durumunu depolamak için Azure depolama alanı ayarlama
Terraform, durumu `terraform.tfstate` dosyasıyla yerel olarak izler. Bu model tek kişilik bir ortamda iyi çalışır. Ancak, daha pratik bir çoklu kişi ortamında, [Azure depolama](/azure/storage/)'yı kullanarak sunucuda durumu izlemeniz gerekir. Bu bölümde gerekli depolama hesabı bilgilerini (hesap adı ve hesap anahtarı) alacak ve Terraform durum bilgilerinin depolanacağı bir depolama kapsayıcısı oluşturacaksınız.

1. Azure portalda sol taraftaki menüden **Tüm hizmetler**'i seçin.

1. **Depolama hesapları**’nı seçin.

1. **Depolama hesapları** sekmesinde Terraform durum bilgilerinin depolanacağı depolama hesabının adını seçin. Örneğin Cloud Shell'i ilk açtığınızda oluşturulmuş olan depolama hesabını kullanabilirsiniz.  Cloud Shell tarafından oluşturulan depolama hesabı genellikle `cs` ile başlar ve sonrasında rastgele sayı ve harf dizesi bulunur. **Daha sonra ihtiyaç duyduğumuz için, seçtiğiniz depolama hesabının adını göz önüne alın.**

1. Depolama hesabı sekmesinde **Erişim anahtarları**'nı seçin.

    ![Depolama hesabı adı](./media/terraform-k8s-cluster-appgw-with-tf-aks/storage-account.png)

1. **key1** **anahtar** değerini not edin. (Anahtarın sağ tarafındaki simgeyi seçtiğinizde değer panoya kopyalanır.)

    ![Depolama hesabı erişim anahtarları](./media/terraform-k8s-cluster-appgw-with-tf-aks/storage-account-access-key.png)

1. Cloud Shell'de Azure depolama hesabınızda bir kapsayıcı oluşturun (&lt;YourAzureStorageAccountName> ve &lt;YourAzureStorageAccountAccessKey> yer tutucularının yerine Azure depolama hesabınıza ait değerleri girin).

    ```azurecli
    az storage container create -n tfstate --account-name <YourAzureStorageAccountName> --account-key <YourAzureStorageAccountKey>
    ```

## <a name="create-the-kubernetes-cluster"></a>Kubernetes kümesi oluşturma
Bu bölümde `terraform init` komutunu kullanarak önceki bölümlerde oluşturduğunuz yapılandırma dosyalarında tanımlanan kaynakları oluşturmayı öğreneceksiniz.

1. Cloud Shell'de Terraform'u başlatın (&lt;YourAzureStorageAccountName> ve &lt;YourAzureStorageAccountAccessKey> yer tutucularının yerine Azure depolama hesabınıza ait değerleri girin).

    ```bash
    terraform init -backend-config="storage_account_name=<YourAzureStorageAccountName>" -backend-config="container_name=tfstate" -backend-config="access_key=<YourStorageAccountAccessKey>" -backend-config="key=codelab.microsoft.tfstate" 
    ```
    
    `terraform init` komutu arka uç ve sağlayıcı eklentisinin başarıyla başlatıldığını gösterir:

    !["terraform init" komutunun sonuçları](./media/terraform-k8s-cluster-appgw-with-tf-aks/terraform-init-complete.png)

1. Cloud Shell giriş değerleri sağlamak için bir değişkenler dosyası oluşturun, adlı `main.tf`bir dosya oluşturun.

    ```bash
    vi terraform.tfvars
    ```

1. I tuşunu seçerek ekleme moduna geçin.

1. Daha önce oluşturulan aşağıdaki değişkenleri düzenleyiciye yapıştırın:

    ```hcl
      resource_group_name = <Name of the Resource Group already created>

      location = <Location of the Resource Group>
        
      aks_service_principal_app_id = <Service Principal AppId>
        
      aks_service_principal_client_secret = <Service Principal Client Secret>
        
      aks_service_principal_object_id = <Service Principal Object Id>
        
    ```

1. **Esc** tuşuna basarak ekleme modundan çıkın.

1. Dosyayı kaydedin ve aşağıdaki komutu girerek VI düzenleyicisini kapatın:

    ```bash
    :wq
    ```

1. `terraform plan` komutunu çalıştırarak altyapı öğelerini tanımlayan Terraform planını oluşturun. 

    ```bash
    terraform plan -out out.plan
    ```

    `terraform plan` komutu, `terraform apply` komutunu çalıştırdığınızda oluşturularak kaynakları gösterir:

    !["terraform plan" komutunun sonuçları](./media/terraform-k8s-cluster-appgw-with-tf-aks/terraform-plan-complete.png)

1. Kubernetes kümesini oluşturma planını uygulamak için `terraform apply` komutunu çalıştırın. Kubernetes kümesi oluşturma işlemi birkaç dakika sürebilir ve bu durum Cloud Shell oturumunun zaman aşımına uğramasına neden olabilir. Cloud Shell oturum zaman aşımına uğrarsa öğreticiyi tamamlamanızı sağlamak için "Cloud Shell zaman aşımından kurtarma" bölümündeki adımları izleyebilirsiniz.

    ```bash
    terraform apply out.plan
    ```

    `terraform apply` komutu, yapılandırma dosyalarınızda tanımlı kaynakların oluşturulmasının sonuçlarını gösterir:

    !["terraform apply" komutunun sonuçları](./media/terraform-k8s-cluster-appgw-with-tf-aks/terraform-apply-complete.png)

1. Azure portal, seçili kaynak grubundaki yeni Kubernetes kümeniz için oluşturulan kaynakları görmek için sol menüdeki **kaynak grupları** ' nı seçin.

    ![Cloud Shell istemi](./media/terraform-k8s-cluster-appgw-with-tf-aks/k8s-resources-created.png)

## <a name="recover-from-a-cloud-shell-timeout"></a>Zaman aşımına uğrayan Cloud Shell oturumunu kurtarma
Cloud Shell oturumu zaman aşımına uğrarsa, kurtarmak için aşağıdaki adımları kullanabilirsiniz:

1. Cloud Shell oturumu başlatın.

1. Terraform yapılandırma dosyalarınızı içeren dizine geçin.

    ```bash
    cd /clouddrive/terraform-aks-k8s
    ```

1. Şu komutu çalıştırın:

    ```bash
    export KUBECONFIG=./azurek8s
    ```
    
## <a name="test-the-kubernetes-cluster"></a>Kubernetes kümesini test etme
Yeni oluşturulan kümeyi doğrulamak için Kubernetes araçlarını kullanabilirsiniz.

1. Terraform durumundaki Kubernetes yapılandırmasını alın ve kubectl tarafından okunabilecek bir dosyaya kaydedin.

    ```bash
    echo "$(terraform output kube_config)" > ./azurek8s
    ```

1. kubectl aracının doğru yapılandırmayı alabilmesi için bir ortam değişkeni ayarlayın.

    ```bash
    export KUBECONFIG=./azurek8s
    ```

1. Kümenin durumunu doğrulayın.

    ```bash
    kubectl get nodes
    ```

    Çalışan düğümlerinin ayrıntılarını görmeniz ve bu düğümlerin durumunun aşağıdaki görüntüde olduğu gibi **Ready** (Hazır) olması gerekir:

    ![kubectl aracı, Kubernetes kümenizin durumunu doğrulamanızı sağlar](./media/terraform-k8s-cluster-appgw-with-tf-aks/kubectl-get-nodes.png)


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede Terraform ve AKS ile Kubernetes kümesi oluşturmayı öğrendiniz. Azure 'da Terrampaform hakkında daha fazla bilgi edinmenize yardımcı olacak bazı ek kaynaklar aşağıda verilmiştir.
 
 > [!div class="nextstepaction"] 
 > [Microsoft.com Terraform Hub'ı](https://docs.microsoft.com/azure/terraform/)
 
