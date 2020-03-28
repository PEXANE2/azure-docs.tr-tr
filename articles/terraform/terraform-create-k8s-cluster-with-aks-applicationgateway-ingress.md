---
title: Öğretici - Azure Kubernetes Hizmetinde Uygulama Ağ Geçidi denetleyicisi oluşturma
description: Bu eğitimde, Giriş denetleyicisi olarak Uygulama Ağ Geçidi ile Azure Kubernetes Hizmeti ile bir Kubernetes Kümesi oluşturursunuz
keywords: masmavi devops terraform uygulama ağ geçidi giriş aks kubernetes
ms.topic: tutorial
ms.date: 03/09/2020
ms.openlocfilehash: 6b48d0acb654f0b0643c0754e53f6bc6ea76bb45
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78945313"
---
# <a name="tutorial-create-an-application-gateway-ingress-controller-in-azure-kubernetes-service"></a>Öğretici: Azure Kubernetes Hizmeti'nde Uygulama Ağ Geçidi denetleyicisi oluşturma

[Azure Kubernetes Service (AKS),](/azure/aks/) barındırılan Kubernetes ortamınızı yönetir. AKS, konteyner düzenleme uzmanlığı olmadan konteyner uygulamalarının hızlı ve kolay bir şekilde dağıtılmasını ve yönetilmesini sağlar. AKS ayrıca, operasyonel ve bakım görevleri için uygulamaları çevrimdışı alma yükünü de ortadan kaldırır. AKS kullanılarak, kaynakların sağlanması, yükseltilmesi ve ölçeklendirilmesi de dahil olmak üzere bu görevler isteğe bağlı olarak gerçekleştirilebilir.

Giriş denetleyicisi Kubernetes hizmetleri için çeşitli özellikler sağlar. Bu özellikler ters proxy, yapılandırılabilir trafik yönlendirme ve TLS sonlandırma içerir. Kubernetes giriş kaynakları, tek tek Kubernetes hizmetleri için giriş kurallarını yapılandırmak için kullanılır. Tek bir IP adresi, giriş denetleyicisi ve giriş kurallarını kullanarak trafiği Bir Kubernetes kümesindeki birden çok hizmete yönlendirebilir. Tüm bu işlevler Azure [Application Gateway](/azure/Application-Gateway/)tarafından sağlanır ve bu da azure'daki Kubernetes için ideal bir Giriş denetleyicisi dir. 

Bu öğreticide, aşağıdaki görevleri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Giriş Denetleyicisi olarak Uygulama Ağ Geçidi ile AKS kullanarak bir [Kubernetes](https://www.redhat.com/en/topics/containers/what-is-kubernetes) kümesi oluşturun.
> * Bir Kubernetes kümesini tanımlamak için HCL (HashiCorp Dili) kullanın.
> * Uygulama Ağ Geçidi kaynağı oluşturmak için Terraform'u kullanın.
> * Bir Kubernetes kümesi oluşturmak için Terraform ve AKS'yi kullanın.
> * Bir Kubernetes kümesinin kullanılabilirliğini test etmek için kubectl aracını kullanın.

## <a name="prerequisites"></a>Ön koşullar

- **Azure aboneliği**: Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.

- **Terraform'u yapılandırma**: [Terraform'u yükleme ve Azure erişimini yapılandırma](terraform-install-configure.md) makalesindeki yönergeleri izleyin

- **Azure kaynak grubu**: Demo için kullanabileceğiniz bir Azure kaynak grubunuz yoksa, [bir Azure kaynak grubu oluşturun.](/azure/azure-resource-manager/manage-resource-groups-portal#create-resource-groups) Bu değerler demoda kullanıldığından kaynak grubu adını ve konumunu not alın.

- **Azure hizmet sorumlusu**: [Azure CLI ile Azure hizmet sorumlusu oluşturma](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest) makalesinin **Hizmet sorumlusunu oluşturma** bölümündeki yönergeleri izleyin. appId, displayName ve parola değerlerine dikkat edin.

- **Hizmet Sorumlusu Nesne Kimliğini Edinin**: Bulut Kabuğu'nda aşağıdaki komutu çalıştırın:`az ad sp list --display-name <displayName>`

## <a name="create-the-directory-structure"></a>Dizin yapısını oluşturma

İlk adım, bu alıştırmadaki Terraform yapılandırma dosyalarınızı barındıracak olan dizini oluşturmaktır.

1. [Azure portalına](https://portal.azure.com)göz atın.

1. [Azure Cloud Shell](/azure/cloud-shell/overview)'i açın.

1. `clouddrive` dizinine geçin.

    ```bash
    cd clouddrive
    ```

1. `terraform-aks-appgw-ingress` adlı bir dizin oluşturun.

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
    code main.tf
    ```

1. Aşağıdaki kodu düzenleyiciye yapıştırın:

    ```hcl
    provider "azurerm" {
      # The "feature" block is required for AzureRM provider 2.x. 
      # If you are using version 1.x, the "features" block is not allowed.
      version = "~>2.0"
      features {}
    }

    terraform {
        backend "azurerm" {}
    }
    ```

1. Dosyayı kaydedin (**&lt;Ctrl>S)** ve düzenleyiciden çıkın (**&lt;Ctrl>Q**).

## <a name="define-input-variables"></a>Giriş değişkenlerini tanımlama

Bu dağıtım için gereken tüm değişkenleri listeleyen Terraform yapılandırma dosyasını oluşturun.

1. Cloud Shell'de `variables.tf` adlı bir dosya oluşturun.

    ```bash
    code variables.tf
    ```

1. Aşağıdaki kodu düzenleyiciye yapıştırın:
    
    ```hcl
    variable "resource_group_name" {
      description = "Name of the resource group."
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
      description = "Disk size (in GB) to provision for each of the agent pool nodes. This value ranges from 0 to 1023. Specifying 0 applies the default disk size for that agentVMSize."
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

1. Dosyayı kaydedin (**&lt;Ctrl>S)** ve düzenleyiciden çıkın (**&lt;Ctrl>Q**).

## <a name="define-the-resources"></a>Kaynakları tanımlama 
Tüm kaynakları oluşturan Terraform yapılandırma dosyası oluşturun. 

1. Cloud Shell'de `resources.tf` adlı bir dosya oluşturun.

    ```bash
    code resources.tf
    ```

1. Yeniden kullanmak üzere hesaplanan değişkenler için yerel bir blok oluşturmak için aşağıdaki kod bloğunu yapıştırın:

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

1. Kaynak grubu, yeni Kullanıcı kimliği için bir veri kaynağı oluşturmak için aşağıdaki kod bloğunu yapıştırın:

    ```hcl
    data "azurerm_resource_group" "rg" {
      name = var.resource_group_name
    }

    # User Assigned Identities 
    resource "azurerm_user_assigned_identity" "testIdentity" {
      resource_group_name = data.azurerm_resource_group.rg.name
      location            = data.azurerm_resource_group.rg.location

      name = "identity1"

      tags = var.tags
    }
    ```

1. Temel ağ kaynakları oluşturmak için aşağıdaki kod bloğunu yapıştırın:

    ```hcl
    resource "azurerm_virtual_network" "test" {
      name                = var.virtual_network_name
      location            = data.azurerm_resource_group.rg.location
      resource_group_name = data.azurerm_resource_group.rg.name
      address_space       = [var.virtual_network_address_prefix]

      subnet {
        name           = var.aks_subnet_name
        address_prefix = var.aks_subnet_address_prefix
      }

      subnet {
        name           = "appgwsubnet"
        address_prefix = var.app_gateway_subnet_address_prefix
      }

      tags = var.tags
    }

    data "azurerm_subnet" "kubesubnet" {
      name                 = var.aks_subnet_name
      virtual_network_name = azurerm_virtual_network.test.name
      resource_group_name  = data.azurerm_resource_group.rg.name
    }

    data "azurerm_subnet" "appgwsubnet" {
      name                 = "appgwsubnet"
      virtual_network_name = azurerm_virtual_network.test.name
      resource_group_name  = data.azurerm_resource_group.rg.name
    }

    # Public Ip 
    resource "azurerm_public_ip" "test" {
      name                         = "publicIp1"
      location                     = data.azurerm_resource_group.rg.location
      resource_group_name          = data.azurerm_resource_group.rg.name
      allocation_method            = "Static"
      sku                          = "Standard"

      tags = var.tags
    }
    ```

1. Uygulama Ağ Geçidi kaynağı oluşturmak için aşağıdaki kod bloğunu yapıştırın:

    ```hcl
    resource "azurerm_application_gateway" "network" {
      name                = var.app_gateway_name
      resource_group_name = data.azurerm_resource_group.rg.name
      location            = data.azurerm_resource_group.rg.location

      sku {
        name     = var.app_gateway_sku
        tier     = "Standard_v2"
        capacity = 2
      }

      gateway_ip_configuration {
        name      = "appGatewayIpConfig"
        subnet_id = data.azurerm_subnet.appgwsubnet.id
      }

      frontend_port {
        name = local.frontend_port_name
        port = 80
      }

      frontend_port {
        name = "httpsPort"
        port = 443
      }

      frontend_ip_configuration {
        name                 = local.frontend_ip_configuration_name
        public_ip_address_id = azurerm_public_ip.test.id
      }

      backend_address_pool {
        name = local.backend_address_pool_name
      }

      backend_http_settings {
        name                  = local.http_setting_name
        cookie_based_affinity = "Disabled"
        port                  = 80
        protocol              = "Http"
        request_timeout       = 1
      }

      http_listener {
        name                           = local.listener_name
        frontend_ip_configuration_name = local.frontend_ip_configuration_name
        frontend_port_name             = local.frontend_port_name
        protocol                       = "Http"
      }

      request_routing_rule {
        name                       = local.request_routing_rule_name
        rule_type                  = "Basic"
        http_listener_name         = local.listener_name
        backend_address_pool_name  = local.backend_address_pool_name
        backend_http_settings_name = local.http_setting_name
      }

      tags = var.tags

      depends_on = ["azurerm_virtual_network.test", "azurerm_public_ip.test"]
    }
    ```

1. Rol atamaları oluşturmak için aşağıdaki kod bloğunu yapıştırın:

    ```hcl
    resource "azurerm_role_assignment" "ra1" {
      scope                = data.azurerm_subnet.kubesubnet.id
      role_definition_name = "Network Contributor"
      principal_id         = var.aks_service_principal_object_id 

      depends_on = ["azurerm_virtual_network.test"]
    }

    resource "azurerm_role_assignment" "ra2" {
      scope                = azurerm_user_assigned_identity.testIdentity.id
      role_definition_name = "Managed Identity Operator"
      principal_id         = var.aks_service_principal_object_id
      depends_on           = ["azurerm_user_assigned_identity.testIdentity"]
    }

    resource "azurerm_role_assignment" "ra3" {
      scope                = azurerm_application_gateway.network.id
      role_definition_name = "Contributor"
      principal_id         = azurerm_user_assigned_identity.testIdentity.principal_id
      depends_on           = ["azurerm_user_assigned_identity.testIdentity", "azurerm_application_gateway.network"]
    }

    resource "azurerm_role_assignment" "ra4" {
      scope                = data.azurerm_resource_group.rg.id
      role_definition_name = "Reader"
      principal_id         = azurerm_user_assigned_identity.testIdentity.principal_id
      depends_on           = ["azurerm_user_assigned_identity.testIdentity", "azurerm_application_gateway.network"]
    }
    ```

1. Kubernetes kümesini oluşturmak için aşağıdaki kod bloğunu yapıştırın:

    ```hcl
    resource "azurerm_kubernetes_cluster" "k8s" {
      name       = var.aks_name
      location   = data.azurerm_resource_group.rg.location
      dns_prefix = var.aks_dns_prefix

      resource_group_name = data.azurerm_resource_group.rg.name

      linux_profile {
        admin_username = var.vm_user_name

        ssh_key {
          key_data = file(var.public_ssh_key_path)
        }
      }

      addon_profile {
        http_application_routing {
          enabled = false
        }
      }

      default_node_pool {
        name            = "agentpool"
        node_count      = var.aks_agent_count
        vm_size         = var.aks_agent_vm_size
        os_disk_size_gb = var.aks_agent_os_disk_size
        vnet_subnet_id  = data.azurerm_subnet.kubesubnet.id
      }

      service_principal {
        client_id     = var.aks_service_principal_app_id
        client_secret = var.aks_service_principal_client_secret
      }

      network_profile {
        network_plugin     = "azure"
        dns_service_ip     = var.aks_dns_service_ip
        docker_bridge_cidr = var.aks_docker_bridge_cidr
        service_cidr       = var.aks_service_cidr
      }

      depends_on = ["azurerm_virtual_network.test", "azurerm_application_gateway.network"]
      tags       = var.tags
    }

    ```

1. Dosyayı kaydedin (**&lt;Ctrl>S)** ve düzenleyiciden çıkın (**&lt;Ctrl>Q**).

Bu bölümde sunulan kod kümenin adını, konumunu ve resource_group_name. Kümeye `dns_prefix` erişmek için kullanılan tam nitelikli alan adı (FQDN) parçasını oluşturan değer ayarlanır.

Kayıt, `linux_profile` SSH kullanarak alt düğümlere oturum açmayı sağlayan ayarları yapılandırmanızı sağlar.

AKS ile yalnızca çalışan düğümleri için ödeme yaparsınız. Kayıt, `agent_pool_profile` bu alt düğümlerin ayrıntılarını yapılandırır. Oluşturulacak `agent_pool_profile record` alt düğüm sayısını ve işçi düğümlerinin türünü içerir. Gelecekte kümeyi büyütmeniz veya küçültmeniz gerekiyorsa, bu `count` kayıttaki değeri değiştirirsiniz.

## <a name="create-a-terraform-output-file"></a>Terraform çıkış dosyası oluşturma

[Terraform çıktıları,](https://www.terraform.io/docs/configuration/outputs.html) Terraform bir plan uyguladığında kullanıcıya vurgulanan değerleri tanımlamanıza olanak sağlar `terraform output` ve komutu kullanarak sorgulanabilir. Bu bölümde [kubectl](https://kubernetes.io/docs/reference/kubectl/overview/) ile kümeye erişmenizi sağlayan bir çıkış dosyası oluşturacaksınız.

1. Cloud Shell'de `output.tf` adlı bir dosya oluşturun.

    ```bash
    code output.tf
    ```

1. Aşağıdaki kodu düzenleyiciye yapıştırın:

    ```hcl
    output "client_key" {
        value = azurerm_kubernetes_cluster.k8s.kube_config.0.client_key
    }

    output "client_certificate" {
        value = azurerm_kubernetes_cluster.k8s.kube_config.0.client_certificate
    }

    output "cluster_ca_certificate" {
        value = azurerm_kubernetes_cluster.k8s.kube_config.0.cluster_ca_certificate
    }

    output "cluster_username" {
        value = azurerm_kubernetes_cluster.k8s.kube_config.0.username
    }

    output "cluster_password" {
        value = azurerm_kubernetes_cluster.k8s.kube_config.0.password
    }

    output "kube_config" {
        value = azurerm_kubernetes_cluster.k8s.kube_config_raw
    }

    output "host" {
        value = azurerm_kubernetes_cluster.k8s.kube_config.0.host
    }

    output "identity_resource_id" {
        value = azurerm_user_assigned_identity.testIdentity.id
    }

    output "identity_client_id" {
        value = azurerm_user_assigned_identity.testIdentity.client_id
    }
    ```

1. Dosyayı kaydedin (**&lt;Ctrl>S)** ve düzenleyiciden çıkın (**&lt;Ctrl>Q**).

## <a name="configure-azure-storage-to-store-terraform-state"></a>Azure depolama alanını Terraform durumunu depolamak için yapılandırma

Terraform, durumu `terraform.tfstate` dosyasıyla yerel olarak izler. Bu model tek kişilik bir ortamda iyi çalışır. Ancak, daha pratik çok kişili bir ortamda, [Azure depolama](/azure/storage/)yı kullanarak sunucudaki durumu izlemeniz gerekir. Bu bölümde, gerekli depolama hesabı bilgilerini almayı ve bir depolama kapsayıcısı oluşturmayı öğrenirsiniz. Terraform durum bilgileri daha sonra bu kapta saklanır.

1. Azure portalında, **Azure hizmetleri**altında **Depolama hesapları'nı**seçin. (Depolama **hesapları** seçeneği ana sayfada görünmüyorsa, **Daha fazla hizmet** seçin ve ardından bulun ve seçin.)

1. Depolama **hesapları** sayfasında, Terraform'un durumu depoladığı depolama hesabının adını seçin. Örneğin Cloud Shell'i ilk açtığınızda oluşturulmuş olan depolama hesabını kullanabilirsiniz.  Cloud Shell tarafından oluşturulan depolama hesabı genellikle `cs` ile başlar ve sonrasında rastgele sayı ve harf dizesi bulunur. 

    Daha sonra ihtiyacınız olduğu için seçtiğiniz depolama hesabını not alın.

1. Depolama hesabı sayfasında **Erişim anahtarları**'nı seçin.

    ![Depolama hesabı adı](./media/terraform-k8s-cluster-appgw-with-tf-aks/storage-account.png)

1. **key1** **anahtar** değerini not edin. (Anahtarın sağ tarafındaki simgeyi seçtiğinizde değer panoya kopyalanır.)

    ![Depolama hesabı erişim anahtarları](./media/terraform-k8s-cluster-appgw-with-tf-aks/storage-account-access-key.png)

1. Cloud Shell'de, Azure depolama hesabınızda bir kapsayıcı oluşturun. Yer tutucuları Azure depolama hesabınız için uygun değerlerle değiştirin.

    ```azurecli
    az storage container create -n tfstate --account-name <YourAzureStorageAccountName> --account-key <YourAzureStorageAccountKey>
    ```

## <a name="create-the-kubernetes-cluster"></a>Kubernetes kümesi oluşturma
Bu bölümde `terraform init` komutunu kullanarak önceki bölümlerde oluşturduğunuz yapılandırma dosyalarında tanımlanan kaynakları oluşturmayı öğreneceksiniz.

1. Cloud Shell'de Terraform'u başlangıç olarak ele ala. Yer tutucuları Azure depolama hesabınız için uygun değerlerle değiştirin.

    ```bash
    terraform init -backend-config="storage_account_name=<YourAzureStorageAccountName>" -backend-config="container_name=tfstate" -backend-config="access_key=<YourStorageAccountAccessKey>" -backend-config="key=codelab.microsoft.tfstate" 
    ```
  
    Komut, `terraform init` arka uç ve sağlayıcı eklentisini başlatma başarısını görüntüler:

    !["terraform init" komutunun sonuçları](./media/terraform-k8s-cluster-appgw-with-tf-aks/terraform-init-complete.png)

1. Bulut Kabuğu'nda: `terraform.tfvars`

    ```bash
    code terraform.tfvars
    ```

1. Daha önce oluşturulan aşağıdaki değişkenleri editöre yapıştırın. Ortamınızın konum değerini almak için `az account list-locations`.

    ```hcl
    resource_group_name = "<Name of the Resource Group already created>"

    location = "<Location of the Resource Group>"
      
    aks_service_principal_app_id = "<Service Principal AppId>"
      
    aks_service_principal_client_secret = "<Service Principal Client Secret>"
      
    aks_service_principal_object_id = "<Service Principal Object Id>"
        
    ```

1. Dosyayı kaydedin (**&lt;Ctrl>S)** ve düzenleyiciden çıkın (**&lt;Ctrl>Q**).

1. `terraform plan` komutunu çalıştırarak altyapı öğelerini tanımlayan Terraform planını oluşturun. 

    ```bash
    terraform plan -out out.plan
    ```

    Komut, `terraform plan` `terraform apply` komutu çalıştırdığınızda oluşturulan kaynakları görüntüler:

    !["terraform plan" komutunun sonuçları](./media/terraform-k8s-cluster-appgw-with-tf-aks/terraform-plan-complete.png)

1. Kubernetes kümesini oluşturma planını uygulamak için `terraform apply` komutunu çalıştırın. Bir Kubernetes kümesi oluşturma işlemi birkaç dakika sürebilir ve bulut bulut oturumunun zamanlaması ile sonuçlanır. Bulut Kabuğu oturumu zaman ları dolursa, öğreticiyi tamamlamanızı sağlamak için "Bulut Kabuğu zaman anından kurtarın" bölümündeki adımları izleyebilirsiniz.

    ```bash
    terraform apply out.plan
    ```

    `terraform apply` komutu, yapılandırma dosyalarınızda tanımlı kaynakların oluşturulmasının sonuçlarını gösterir:

    !["terraform apply" komutunun sonuçları](./media/terraform-k8s-cluster-appgw-with-tf-aks/terraform-apply-complete.png)

1. Azure portalında, seçili kaynak grubunda yeni Kubernetes kümeniz için oluşturulan kaynakları görmek için sol menüdeki **Kaynak Grupları'nı** seçin.

    ![Cloud Shell istemi](./media/terraform-k8s-cluster-appgw-with-tf-aks/k8s-resources-created.png)

## <a name="recover-from-a-cloud-shell-timeout"></a>Zaman aşımına uğrayan Cloud Shell oturumunu kurtarma

Bulut Kabuğu oturumu zaman ları dolursa, kurtarmak için aşağıdaki adımları kullanabilirsiniz:

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

## <a name="install-azure-ad-pod-identity"></a>Azure AD Pod Kimliğini Yükleme

Azure Etkin Dizin Pod Kimliği, [Azure Kaynak Yöneticisi'ne](/azure/azure-resource-manager/resource-group-overview)belirteç tabanlı erişim sağlar.

[Azure AD Pod Kimliği,](https://github.com/Azure/aad-pod-identity) Kubernetes kümenize aşağıdaki bileşenleri ekler:

  - Kubernetes [CRDs](https://kubernetes.io/docs/tasks/access-kubernetes-api/custom-resources/custom-resource-definitions/): `AzureIdentity`, `AzureAssignedIdentity`,`AzureIdentityBinding`
  - [Yönetilen Kimlik Denetleyicisi (MIC)](https://github.com/Azure/aad-pod-identity#managed-identity-controllermic) bileşeni
  - [Düğüm Yönetilen Kimlik (NMI)](https://github.com/Azure/aad-pod-identity#node-managed-identitynmi) bileşeni

RBAC **etkinse,** cluster'ınıza Azure AD Pod Kimliği yüklemek için aşağıdaki komutu çalıştırın:

```bash
kubectl create -f https://raw.githubusercontent.com/Azure/aad-pod-identity/master/deploy/infra/deployment-rbac.yaml
```

RBAC **devre dışı bırakılırsa,** cluster'ınıza Azure AD Pod Kimliği yüklemek için aşağıdaki komutu çalıştırın:

```bash
kubectl create -f https://raw.githubusercontent.com/Azure/aad-pod-identity/master/deploy/infra/deployment.yaml
```

## <a name="install-helm"></a>Miğferi Yükle

Bu bölümdeki `application-gateway-kubernetes-ingress` kod, paketi yüklemek için [Helm](/azure/aks/kubernetes-helm) - Kubernetes paket yöneticisini kullanır:

1. RBAC **etkinse,** Miğferi yüklemek ve yapılandırmak için aşağıdaki komut kümesini çalıştırın:

    ```bash
    kubectl create serviceaccount --namespace kube-system tiller-sa
    kubectl create clusterrolebinding tiller-cluster-rule --clusterrole=cluster-admin --serviceaccount=kube-system:tiller-sa
    helm init --tiller-namespace kube-system --service-account tiller-sa
    ```

1. RBAC **devre dışı bırakılırsa,** Miğferi yüklemek ve yapılandırmak için aşağıdaki komutu çalıştırın:

    ```bash
    helm init
    ```

1. AGIC Miğfer deposunu ekleyin:

    ```bash
    helm repo add application-gateway-kubernetes-ingress https://appgwingress.blob.core.windows.net/ingress-azure-helm-package/
    helm repo update
    ```

## <a name="install-ingress-controller-helm-chart"></a>Ingress Controller Dümen Grafiği ni yükleyin

1. AGIC yapılandırmak için indirin: `helm-config.yaml`

    ```bash
    wget https://raw.githubusercontent.com/Azure/application-gateway-kubernetes-ingress/master/docs/examples/sample-helm-config.yaml -O helm-config.yaml
    ```

1. Edin `helm-config.yaml` ve uygun `appgw` değerleri `armAuth` girin ve bölümler.

    ```bash
    code helm-config.yaml
    ```

    Değerler aşağıdaki gibi açıklanmıştır:

    - `verbosityLevel`: AGIC günlük altyapısının özkaynak düzeyini ayarlar. Olası değerler için [Günlük Düzeyleri'ne](https://github.com/Azure/application-gateway-kubernetes-ingress/blob/463a87213bbc3106af6fce0f4023477216d2ad78/docs/troubleshooting.md#logging-levels) bakın.
    - `appgw.subscriptionId`: Uygulama Ağ Geçidi için Azure Abonelik Kimliği. Örnek: `a123b234-a3b4-557d-b2df-a0bc12de1234`
    - `appgw.resourceGroup`: App Ağ Geçidi'nin oluşturulduğu Azure Kaynak Grubu'nun adı. 
    - `appgw.name`: Uygulama Ağ Geçidinin Adı. Örnek: `applicationgateway1`.
    - `appgw.shared`: Bu boolean bayrak varsayılan `false`olmalıdır . Paylaşılan `true` Uygulama Ağ [Geçidine](https://github.com/Azure/application-gateway-kubernetes-ingress/blob/072626cb4e37f7b7a1b0c4578c38d1eadc3e8701/docs/setup/install-existing.md#multi-cluster--shared-app-gateway)ihtiyacınız olması için ayarlayın.
    - `kubernetes.watchNamespace`: AGIC'in izlemesi gereken isim alanını belirtin. Ad alanı tek bir dize değeri veya virgülle ayrılmış ad alanları listesi olabilir. Bu değişkeni dışarıda bırakmak veya boş veya boş dize lere ayarlamak, erişilebilen tüm ad alanlarını gözlemleyerek Giriş Denetleyicisi'ne neden olur.
    - `armAuth.type`: Ya bir `aadPodIdentity` `servicePrincipal`değer ya da .
    - `armAuth.identityResourceID`: Yönetilen kimliğin kaynak kimliği.
    - `armAuth.identityClientId`: Kimliğin Müşteri Kimliği.
    - `armAuth.secretJSON`: Yalnızca Servis Müdürü Gizli türü `armAuth.type` seçildiğinde `servicePrincipal`(ayarlandığında) gereklidir.

    Anahtar notlar:
    - Değer `identityResourceID` terraform komut dosyasında oluşturulur ve çalıştırılarak `echo "$(terraform output identity_resource_id)"`bulunabilir: .
    - Değer `identityClientID` terraform komut dosyasında oluşturulur ve çalıştırılarak `echo "$(terraform output identity_client_id)"`bulunabilir: .
    - Değer, `<resource-group>` Uygulama Ağ Geçidinizin kaynak grubudur.
    - Değer, `<identity-name>` oluşturulan kimliğin adıdır.
    - Belirli bir abonelik için tüm kimlikler şu şekilde listelenebilir: `az identity list`.

1. Uygulama Ağ Geçidi giriş denetleyici paketini yükleyin:

    ```bash
    helm install -f helm-config.yaml application-gateway-kubernetes-ingress/ingress-azure
    ```

### <a name="install-a-sample-app"></a>Örnek bir uygulama yükleme

App Gateway, AKS ve AGIC'i yükledikten sonra [Azure Cloud Shell](https://shell.azure.com/)üzerinden örnek bir uygulama yükleyebilirsiniz:

1. YAML dosyasını indirmek için kıvırma komutunu kullanın:

    ```bash
    curl https://raw.githubusercontent.com/Azure/application-gateway-kubernetes-ingress/master/docs/examples/aspnetapp.yaml -o aspnetapp.yaml
    ```

2. YAML dosyasını uygulayın:

    ```bash
    kubectl apply -f aspnetapp.yaml
    ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, bu makalede oluşturulan kaynakları silin.  

Yer tutucuyu uygun değerle değiştirin. Belirtilen kaynak grubundaki tüm kaynaklar silinir.

```azurecli
az group delete -n <resource-group>
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Application Gateway Giriş Denetleyicisi](https://azure.github.io/application-gateway-kubernetes-ingress/)