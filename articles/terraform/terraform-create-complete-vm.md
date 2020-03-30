---
title: Quickstart - Azure'da eksiksiz bir Linux VM oluşturmak için Terraform'u kullanın
description: Bu hızlı başlangıçta, Azure'da eksiksiz bir Linux sanal makine ortamı oluşturmak ve yönetmek için Terraform'u kullanırsınız
keywords: azure devops terraform linux vm sanal makine
ms.topic: quickstart
ms.date: 03/15/2020
ms.openlocfilehash: f262734cc16d97e4d73af371410403a4cbb8815e
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79415470"
---
# <a name="quickstart-create-a-complete-linux-virtual-machine-infrastructure-in-azure-with-terraform"></a>Quickstart: Terraform ile Azure'da eksiksiz bir Linux sanal makine altyapısı oluşturun

Terraform, Azure'da eksiksiz altyapı dağıtımları tanımlamanıza ve oluşturmanıza olanak tanır. Terraform şablonlarını, Azure kaynaklarını tutarlı ve tekrarlanabilir bir şekilde oluşturan ve yapılandıran, insan tarafından okunabilir bir biçimde oluşturursunuz. Bu makalede, terraform ile tam bir Linux ortamı ve destekleyici kaynaklar oluşturmak için nasıl gösterir. Ayrıca [Terraform'u nasıl yükleyip yapılandırabileceğinizi](terraform-install-configure.md)de öğrenebilirsiniz.

> [!NOTE]
> Terraform'a özel destek için lütfen topluluk kanallarından birini kullanarak Terraform'a doğrudan ulaşın:
>
>    * Topluluk portalının [Terraform bölümü](https://discuss.hashicorp.com/c/terraform-core) sorular, kullanım örnekleri ve yararlı desenler içerir.
>
>    * Sağlayıcıyla ilgili sorularınız için lütfen topluluk portalının [Terraform Sağlayıcıları](https://discuss.hashicorp.com/c/terraform-providers) bölümünü ziyaret edin.


## <a name="create-azure-connection-and-resource-group"></a>Azure bağlantısı ve kaynak grubu oluşturma

Terraform şablonunun her bölümüne bakalım. Ayrıca kopyalayıp yapıştırabileceğiniz [Terraform şablonunun](#complete-terraform-script) tam sürümünü de görebilirsiniz.

Bölüm, Terraform'a `provider` bir Azure sağlayıcısı kullanmasını söyler. ,, , `subscription_id` `client_id` `client_secret`, ve `tenant_id`, için değerler almak için [Terraform'u Yükleyin ve yapılandırın.](terraform-install-configure.md) 

> [!TIP]
> Değerler için ortam değişkenleri oluşturuyorsanız veya [Azure Bulut Ubash deneyimini](/azure/cloud-shell/overview) kullanıyorsanız, değişken bildirimlerini bu bölüme eklemeniz gerekmez.

```hcl
provider "azurerm" {
    # The "feature" block is required for AzureRM provider 2.x. 
    # If you're using version 1.x, the "features" block is not allowed.
    version = "~>2.0"
    features {}
    
    subscription_id = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    client_id       = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    client_secret   = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    tenant_id       = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
}
```

Aşağıdaki `eastus` bölümde, konumda adı `myResourceGroup` geçen bir kaynak grubu oluşturulur:

```hcl
resource "azurerm_resource_group" "myterraformgroup" {
    name     = "myResourceGroup"
    location = "eastus"

    tags = {
        environment = "Terraform Demo"
    }
}
```

Ek bölümlerde, kaynak grubuna `${azurerm_resource_group.myterraformgroup.name}`başvurursunuz.

## <a name="create-virtual-network"></a>Sanal ağ oluşturma
Aşağıdaki bölüm `myVnet` `10.0.0.0/16` adres alanında adlı bir sanal ağ oluşturur:

```hcl
resource "azurerm_virtual_network" "myterraformnetwork" {
    name                = "myVnet"
    address_space       = ["10.0.0.0/16"]
    location            = "eastus"
    resource_group_name = azurerm_resource_group.myterraformgroup.name

    tags = {
        environment = "Terraform Demo"
    }
}
```

Aşağıdaki bölüm `mySubnet` `myVnet` sanal ağda bir alt ağ oluşturur:

```hcl
resource "azurerm_subnet" "myterraformsubnet" {
    name                 = "mySubnet"
    resource_group_name  = azurerm_resource_group.myterraformgroup.name
    virtual_network_name = azurerm_virtual_network.myterraformnetwork.name
    address_prefix       = "10.0.2.0/24"
}
```


## <a name="create-public-ip-address"></a>Ortak IP adresi oluşturma
Internet'teki kaynaklara erişmek için VM'inize genel bir IP adresi oluşturun ve atayın. Aşağıdaki bölüm, genel bir IP `myPublicIP`adresi oluşturur:

```hcl
resource "azurerm_public_ip" "myterraformpublicip" {
    name                         = "myPublicIP"
    location                     = "eastus"
    resource_group_name          = azurerm_resource_group.myterraformgroup.name
    allocation_method            = "Dynamic"

    tags = {
        environment = "Terraform Demo"
    }
}
```


## <a name="create-network-security-group"></a>Ağ Güvenlik Grubu Oluşturma
Ağ Güvenlik Grupları, VM'nize giriş ve çıkışlı ağ trafiğinin akışını denetler. Aşağıdaki bölüm, tcp bağlantı `myNetworkSecurityGroup` noktası 22'de SSH trafiğine izin verecek bir kural adlandırılmış bir ağ güvenlik grubu oluşturur ve bir kural tanımlar:

```hcl
resource "azurerm_network_security_group" "myterraformnsg" {
    name                = "myNetworkSecurityGroup"
    location            = "eastus"
    resource_group_name = azurerm_resource_group.myterraformgroup.name
    
    security_rule {
        name                       = "SSH"
        priority                   = 1001
        direction                  = "Inbound"
        access                     = "Allow"
        protocol                   = "Tcp"
        source_port_range          = "*"
        destination_port_range     = "22"
        source_address_prefix      = "*"
        destination_address_prefix = "*"
    }

    tags = {
        environment = "Terraform Demo"
    }
}
```


## <a name="create-virtual-network-interface-card"></a>Sanal ağ arabirim kartı oluşturma
Sanal ağ arabirim kartı (NIC), VM'nizi belirli bir sanal ağa, genel IP adresine ve ağ güvenlik grubuna bağlar. Terraform şablonundaki aşağıdaki bölüm, oluşturduğunuz `myNIC` sanal ağ kaynaklarına bağlı olarak sanal bir NIC oluşturur:

```hcl
resource "azurerm_network_interface" "myterraformnic" {
    name                        = "myNIC"
    location                    = "eastus"
    resource_group_name         = azurerm_resource_group.myterraformgroup.name

    ip_configuration {
        name                          = "myNicConfiguration"
        subnet_id                     = "${azurerm_subnet.myterraformsubnet.id}"
        private_ip_address_allocation = "Dynamic"
        public_ip_address_id          = "${azurerm_public_ip.myterraformpublicip.id}"
    }

    tags = {
        environment = "Terraform Demo"
    }
}

# Connect the security group to the network interface
resource "azurerm_network_interface_security_group_association" "example" {
    network_interface_id      = azurerm_network_interface.myterraformnic.id
    network_security_group_id = azurerm_network_security_group.myterraformnsg.id
}
```


## <a name="create-storage-account-for-diagnostics"></a>Tanılama için depolama hesabı oluşturma
VM için önyükleme tanılamadepolamak için bir depolama hesabı gerekir. Bu önyükleme tanılama sorunları gidermek ve VM durumunu izlemek yardımcı olabilir. Oluşturduğunuz depolama hesabı yalnızca önyükleme tanılama verilerini depolamak içindir. Her depolama hesabının benzersiz bir adı olması gerektiğinden, aşağıdaki bölüm rastgele bir metin oluşturur:

```hcl
resource "random_id" "randomId" {
    keepers = {
        # Generate a new ID only when a new resource group is defined
        resource_group = azurerm_resource_group.myterraformgroup.name
    }
    
    byte_length = 8
}
```

Artık bir depolama hesabı oluşturabilirsiniz. Aşağıdaki bölüm, adı önceki adımda oluşturulan rasgele metne dayalı bir depolama hesabı oluşturur:

```hcl
resource "azurerm_storage_account" "mystorageaccount" {
    name                        = "diag${random_id.randomId.hex}"
    resource_group_name         = azurerm_resource_group.myterraformgroup.name
    location                    = "eastus"
    account_replication_type    = "LRS"
    account_tier                = "Standard"

    tags = {
        environment = "Terraform Demo"
    }
}
```


## <a name="create-virtual-machine"></a>Sanal makine oluşturma

Son adım bir VM oluşturmak ve oluşturulan tüm kaynakları kullanmaktır. Aşağıdaki bölümde bir VM `myVM` adlı oluşturur ve adlı `myNIC`sanal NIC bağlanır. En `Ubuntu 16.04-LTS` son görüntü kullanılır ve `azureuser` parola kimlik doğrulaması devre dışı bırakılmış olarak adlandırılmış bir kullanıcı oluşturulur.

 `ssh_keys` Bölümünde SSH anahtar verileri sağlanmaktadır. `key_data` Alanında ortak bir SSH anahtarı sağlayın.

```hcl
resource "azurerm_linux_virtual_machine" "myterraformvm" {
    name                  = "myVM"
    location              = "eastus"
    resource_group_name   = azurerm_resource_group.myterraformgroup.name
    network_interface_ids = [azurerm_network_interface.myterraformnic.id]
    size                  = "Standard_DS1_v2"

    os_disk {
        name              = "myOsDisk"
        caching           = "ReadWrite"
        storage_account_type = "Premium_LRS"
    }

    source_image_reference {
        publisher = "Canonical"
        offer     = "UbuntuServer"
        sku       = "16.04.0-LTS"
        version   = "latest"
    }

    computer_name  = "myvm"
    admin_username = "azureuser"
    disable_password_authentication = true
        
    admin_ssh_key {
        username       = "azureuser"
        public_key     = file("/home/azureuser/.ssh/authorized_keys")
    }

    boot_diagnostics {
        storage_account_uri = azurerm_storage_account.mystorageaccount.primary_blob_endpoint
    }

    tags = {
        environment = "Terraform Demo"
    }
}
```

## <a name="complete-terraform-script"></a>Tam Terraform komut dosyası

Tüm bu bölümleri bir araya getirmek ve Terraform'u iş başında görmek için, aşağıdaki içeriği adında `terraform_azure.tf` bir dosya oluşturun ve yapıştırın:

```hcl
# Configure the Microsoft Azure Provider
provider "azurerm" {
    # The "feature" block is required for AzureRM provider 2.x. 
    # If you're using version 1.x, the "features" block is not allowed.
    version = "~>2.0"
    features {}

    subscription_id = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    client_id       = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    client_secret   = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    tenant_id       = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
}

# Create a resource group if it doesn't exist
resource "azurerm_resource_group" "myterraformgroup" {
    name     = "myResourceGroup"
    location = "eastus"

    tags = {
        environment = "Terraform Demo"
    }
}

# Create virtual network
resource "azurerm_virtual_network" "myterraformnetwork" {
    name                = "myVnet"
    address_space       = ["10.0.0.0/16"]
    location            = "eastus"
    resource_group_name = azurerm_resource_group.myterraformgroup.name

    tags = {
        environment = "Terraform Demo"
    }
}

# Create subnet
resource "azurerm_subnet" "myterraformsubnet" {
    name                 = "mySubnet"
    resource_group_name  = azurerm_resource_group.myterraformgroup.name
    virtual_network_name = azurerm_virtual_network.myterraformnetwork.name
    address_prefix       = "10.0.1.0/24"
}

# Create public IPs
resource "azurerm_public_ip" "myterraformpublicip" {
    name                         = "myPublicIP"
    location                     = "eastus"
    resource_group_name          = azurerm_resource_group.myterraformgroup.name
    allocation_method            = "Dynamic"

    tags = {
        environment = "Terraform Demo"
    }
}

# Create Network Security Group and rule
resource "azurerm_network_security_group" "myterraformnsg" {
    name                = "myNetworkSecurityGroup"
    location            = "eastus"
    resource_group_name = azurerm_resource_group.myterraformgroup.name
    
    security_rule {
        name                       = "SSH"
        priority                   = 1001
        direction                  = "Inbound"
        access                     = "Allow"
        protocol                   = "Tcp"
        source_port_range          = "*"
        destination_port_range     = "22"
        source_address_prefix      = "*"
        destination_address_prefix = "*"
    }

    tags = {
        environment = "Terraform Demo"
    }
}

# Create network interface
resource "azurerm_network_interface" "myterraformnic" {
    name                      = "myNIC"
    location                  = "eastus"
    resource_group_name       = azurerm_resource_group.myterraformgroup.name

    ip_configuration {
        name                          = "myNicConfiguration"
        subnet_id                     = azurerm_subnet.myterraformsubnet.id
        private_ip_address_allocation = "Dynamic"
        public_ip_address_id          = azurerm_public_ip.myterraformpublicip.id
    }

    tags = {
        environment = "Terraform Demo"
    }
}

# Connect the security group to the network interface
resource "azurerm_network_interface_security_group_association" "example" {
    network_interface_id      = azurerm_network_interface.myterraformnic.id
    network_security_group_id = azurerm_network_security_group.myterraformnsg.id
}

# Generate random text for a unique storage account name
resource "random_id" "randomId" {
    keepers = {
        # Generate a new ID only when a new resource group is defined
        resource_group = azurerm_resource_group.myterraformgroup.name
    }
    
    byte_length = 8
}

# Create storage account for boot diagnostics
resource "azurerm_storage_account" "mystorageaccount" {
    name                        = "diag${random_id.randomId.hex}"
    resource_group_name         = azurerm_resource_group.myterraformgroup.name
    location                    = "eastus"
    account_tier                = "Standard"
    account_replication_type    = "LRS"

    tags = {
        environment = "Terraform Demo"
    }
}

# Create virtual machine
resource "azurerm_linux_virtual_machine" "myterraformvm" {
    name                  = "myVM"
    location              = "eastus"
    resource_group_name   = azurerm_resource_group.myterraformgroup.name
    network_interface_ids = [azurerm_network_interface.myterraformnic.id]
    size                  = "Standard_DS1_v2"

    os_disk {
        name              = "myOsDisk"
        caching           = "ReadWrite"
        storage_account_type = "Premium_LRS"
    }

    source_image_reference {
        publisher = "Canonical"
        offer     = "UbuntuServer"
        sku       = "16.04.0-LTS"
        version   = "latest"
    }

    computer_name  = "myvm"
    admin_username = "azureuser"
    disable_password_authentication = true
        
    admin_ssh_key {
        username       = "azureuser"
        public_key     = file("/home/azureuser/.ssh/authorized_keys")
    }

    boot_diagnostics {
        storage_account_uri = azurerm_storage_account.mystorageaccount.primary_blob_endpoint
    }

    tags = {
        environment = "Terraform Demo"
    }
}
```


## <a name="build-and-deploy-the-infrastructure"></a>Altyapıyı oluşturma ve dağıtma
Terraform şablonunuzun oluşturulmasıyla, ilk adım Terraform'u başlatmaktır. Bu adım, Terraform'un şablonunuzu Azure'da oluşturmak için tüm ön koşullara sahip olmasını sağlar.

```bash
terraform init
```

Bir sonraki adım Terraform gözden geçirmek ve şablonu doğrulamak için. Bu adım, istenen kaynakları Terraform tarafından kaydedilen durum bilgileriyle karşılaştırır ve ardından planlanan yürütmeyi dışarı çıkarır. Azure kaynakları bu noktada oluşturulmaz.

```bash
terraform plan
```

Önceki komutu çalıştırdıktan sonra aşağıdaki ekrana benzer bir şey görmeniz gerekir:

```console
Refreshing Terraform state in-memory prior to plan...
The refreshed state will be used to calculate this plan, but will not be
persisted to local or remote state storage.


...

Note: You didn't specify an "-out" parameter to save this plan, so when
"apply" is called, Terraform can't guarantee this is what will execute.
  + azurerm_resource_group.myterraform
      <snip>
  + azurerm_virtual_network.myterraformnetwork
      <snip>
  + azurerm_network_interface.myterraformnic
      <snip>
  + azurerm_network_security_group.myterraformnsg
      <snip>
  + azurerm_public_ip.myterraformpublicip
      <snip>
  + azurerm_subnet.myterraformsubnet
      <snip>
  + azurerm_virtual_machine.myterraformvm
      <snip>
Plan: 7 to add, 0 to change, 0 to destroy.
```

Her şey doğru görünüyorsa ve Azure'da altyapı oluşturmaya hazırsanız, şablonu Terraform'da uygulayın:

```bash
terraform apply
```

Terraform tamamlandığında, VM altyapınız hazır dır. [Az vm show](/cli/azure/vm)ile VM'nizin genel IP adresini edinin:

```azurecli-interactive
az vm show --resource-group myResourceGroup --name myVM -d --query [publicIps] --o tsv
```

Daha sonra VM'nize SSH yapabilirsiniz:

```bash
ssh azureuser@<publicIps>
```

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Azure'da Terraform'u kullanma hakkında daha fazla bilgi edinin](/azure/terraform)