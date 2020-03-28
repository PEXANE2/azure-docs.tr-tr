---
title: Öğretici - Terraform durumunu Azure Depolama'da saklayın
description: Terraform durumunu Azure Depolama'da depolamaya giriş.
ms.topic: tutorial
ms.date: 11/07/2019
ms.openlocfilehash: 1cc475e5070b21a7ea96585f2183c07d258acdc5
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75708433"
---
# <a name="tutorial-store-terraform-state-in-azure-storage"></a>Öğretici: Terraform durumunu Azure Depolama'da depolama

Terraform durumu, dağıtılan kaynakları Terraform yapılandırmalarıyla bağdaştırmak için kullanılır. Durum, Terraform'un hangi Azure kaynaklarını ekleyeceğini, güncelleyeceğini veya silmesini bilmesini sağlar. Varsayılan olarak, `terraform apply` komutu çalıştırdığınızda Terraform durumu yerel olarak depolanır. Bu yapılandırma aşağıdaki nedenlerle ideal değildir:

- Yerel durum, bir takım veya işbirlikçi ortamda iyi çalışmaz.
- Terraform durumu hassas bilgileri içerebilir.
- Durumu yerel olarak depolamak, yanlışlıkla silme olasılığını artırır.

Terraform, uzak depolamada devletin kalıcılığı destekler. Bu tür desteklenen arka uç Azure Depolama olduğunu. Bu belge, azure depolamayı bu amaçla nasıl yapılandırıştırıp kullanacağımı gösterir.

## <a name="configure-storage-account"></a>Depolama hesabını yapılandırma

Azure Depolama'yı arka uç olarak kullanmadan önce bir depolama hesabı oluşturmanız gerekir. Depolama hesabı Azure portalı, PowerShell, Azure CLI veya Terraform'un kendisiyle oluşturulabilir. Depolama hesabını Azure CLI ile yapılandırmak için aşağıdaki örneği kullanın.

```azurecli
#!/bin/bash

RESOURCE_GROUP_NAME=tstate
STORAGE_ACCOUNT_NAME=tstate$RANDOM
CONTAINER_NAME=tstate

# Create resource group
az group create --name $RESOURCE_GROUP_NAME --location eastus

# Create storage account
az storage account create --resource-group $RESOURCE_GROUP_NAME --name $STORAGE_ACCOUNT_NAME --sku Standard_LRS --encryption-services blob

# Get storage account key
ACCOUNT_KEY=$(az storage account keys list --resource-group $RESOURCE_GROUP_NAME --account-name $STORAGE_ACCOUNT_NAME --query [0].value -o tsv)

# Create blob container
az storage container create --name $CONTAINER_NAME --account-name $STORAGE_ACCOUNT_NAME --account-key $ACCOUNT_KEY

echo "storage_account_name: $STORAGE_ACCOUNT_NAME"
echo "container_name: $CONTAINER_NAME"
echo "access_key: $ACCOUNT_KEY"
```

Depolama hesabı adını, kapsayıcı adını ve depolama erişim anahtarını dikkate alın. Uzak durumu yapılandırdığınızda bu değerler gereklidir.

## <a name="configure-state-back-end"></a>Durumu arka uca yapılandırma

`terraform init` Terraform durumu arka uç komutu çalıştırdığınızda yapılandırılır. Durumu arka uca yapılandırmak için aşağıdaki veriler gereklidir:

- **storage_account_name**: Azure Depolama hesabının adı.
- **container_name**: Blob kabının adı.
- **key**: Oluşturulacak durum deposu dosyasının adı.
- **access_key**: Depolama erişim anahtarı.

Bu değerlerin her biri Terraform yapılandırma dosyasında veya komut satırında belirtilebilir. Değer için bir ortam değişkeni `access_key` kullanmanızı öneririz. Bir ortam değişkeninin kullanılması anahtarın diske yazılmasını engeller.

Azure Depolama erişim `ARM_ACCESS_KEY` anahtarının değeriyle birlikte bir ortam değişkeni oluşturun.

```bash
export ARM_ACCESS_KEY=<storage access key>
```

Azure Depolama hesabı erişim anahtarını daha fazla korumak için Azure Key Vault'ta saklayın. Ortam değişkeni daha sonra aşağıdakine benzer bir komut kullanılarak ayarlanabilir. Azure Anahtar Kasası hakkında daha fazla bilgi için [Azure Anahtar Kasası belgelerine](../key-vault/quick-create-cli.md)bakın.

```bash
export ARM_ACCESS_KEY=$(az keyvault secret show --name terraform-backend-key --vault-name myKeyVault --query value -o tsv)
```

Terraform'u arka ucu kullanacak şekilde yapılandırmak için aşağıdaki adımların yapılması gerekir:
- Bir `backend` tür `azurerm`.
- Yapılandırma `storage_account_name` bloğuna değer katın.
- Yapılandırma `container_name` bloğuna değer katın.
- Yapılandırma `key` bloğuna değer katın.

Aşağıdaki örnek, bir Terraform arka ucunu yapılandırır ve bir Azure kaynak grubu oluşturur.

```hcl
terraform {
  backend "azurerm" {
    resource_group_name   = "tstate"
    storage_account_name  = "tstate09762"
    container_name        = "tstate"
    key                   = "terraform.tfstate"
  }
}

resource "azurerm_resource_group" "state-demo-secure" {
  name     = "state-demo"
  location = "eastus"
}
```

Aşağıdaki adımları yaparak yapılandırmayı başlatma:

1. `terraform init` komutunu çalıştırın.
1. `terraform apply` komutunu çalıştırın.

Artık durum dosyasını Azure Depolama blob'unda bulabilirsiniz.

## <a name="state-locking"></a>Durum kilitleme

Azure Depolama lekeleri, durumu yazan herhangi bir işlemden önce otomatik olarak kilitlenir. Bu desen, bozulmaya neden olabilecek eşzamanlı durum işlemlerini engeller. 

Daha fazla bilgi için Terraform belgelerinde [Eyalet kilitleme'ye](https://www.terraform.io/docs/state/locking.html) bakın.

Azure portalı veya diğer Azure yönetim aracı aracılığıyla blob'u incelediğinizde kilidi görebilirsiniz.

![Kilitli masmavi blob](media/terraform-backend/lock.png)

## <a name="encryption-at-rest"></a>Bekleme sırasında şifreleme

Azure blob'unda depolanan veriler, kalıcı olmadan önce şifrelenir. Gerektiğinde Terraform durumu arka uçtan alır ve yerel bellekte saklar. Bu deseni kullanarak, durum hiçbir zaman yerel diskinize yazılı değildir.

Azure Depolama şifrelemesi hakkında daha fazla bilgi için, [veriler için Azure Depolama hizmet şifrelemesi'ne](../storage/common/storage-service-encryption.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Azure'da Terraform'u kullanma hakkında daha fazla bilgi edinin](/azure/terraform)
