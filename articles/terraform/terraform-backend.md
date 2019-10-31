---
title: Öğretici-Azure Storage 'da Terrayform durumunu depolama
description: Azure depolama 'da Terrayform durumunu depolamanın bir girişi.
ms.service: terraform
author: tomarchermsft
ms.author: tarcher
ms.topic: tutorial
ms.date: 10/26/2019
ms.openlocfilehash: f024fd7886ec6f192c440cca6951e3aeb66ad22d
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73177818"
---
# <a name="tutorial-store-terraform-state-in-azure-storage"></a>Öğretici: Azure Storage 'da Terrayform durumunu depolama

Terrayform, dağıtılmış kaynakları Teraform yapılandırmalarına göre mutabık kılmak için kullanılır. Durum, Terüform 'un hangi Azure kaynaklarını eklemek, güncelleştirmek veya silmek gerektiğini bilmesini sağlar. Varsayılan olarak, `terraform apply` komutu çalıştırılırken Terrayform durumu yerel olarak depolanır. Bu yapılandırma aşağıdaki nedenlerle ideal değildir:

- Yerel durum, bir takımda veya işbirliği ortamında iyi çalışmıyor
- Terrayform durumu hassas bilgiler içerebilir
- Durumu yerel olarak depolama, yanlışlıkla silme olasılığını artırır

Terrayform, uzak depolamada durumu kalıcı hale getirmeyi destekler. Desteklenen bir arka uç, Azure Depolama ' dir. Bu belgede, Azure depolama 'nın bu amaçla nasıl yapılandırılacağı ve kullanılacağı gösterilmektedir.

## <a name="configure-storage-account"></a>Depolama hesabını yapılandırma

Azure Storage 'ı arka uç olarak kullanmadan önce bir depolama hesabının oluşturulması gerekir. Depolama hesabı Azure portal, PowerShell, Azure CLı veya Terrayform ile oluşturulabilir. Azure CLı ile depolama hesabını yapılandırmak için aşağıdaki örneği kullanın.

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

Depolama hesabı adı, kapsayıcı adı ve depolama erişim anahtarı ' nı bir yere göz atın. Bu değerler, uzak durum yapılandırılırken gereklidir.

## <a name="configure-state-backend"></a>Durum arka ucunu yapılandırma

`terraform init` komutu çalıştırılırken Terrayform durumu arka ucu yapılandırılır. durum arka ucunu yapılandırmak için aşağıdaki veriler gereklidir:

- storage_account_name-Azure Storage hesabının adı.
- container_name-blob kapsayıcısının adı.
- anahtar-oluşturulacak durum depolama dosyasının adı.
- access_key-depolama erişim anahtarı.

Bu değerlerin her biri Teraform yapılandırma dosyasında veya komut satırında belirtilebilir, ancak `access_key`için bir ortam değişkeni kullanılması önerilir. Bir ortam değişkeni kullanmak, anahtarın diske yazılmasını engeller.

Azure depolama erişim anahtarı değeriyle `ARM_ACCESS_KEY` adlı bir ortam değişkeni oluşturun.

```bash
export ARM_ACCESS_KEY=<storage access key>
```

Azure depolama hesabı erişim anahtarını daha fazla korumak için Azure Key Vault ' de saklayın. Daha sonra, ortam değişkeni aşağıdakine benzer bir komut kullanılarak ayarlanabilir. Azure Key Vault hakkında daha fazla bilgi için [Azure Key Vault belgelerine](../key-vault/quick-create-cli.md)bakın.

```bash
export ARM_ACCESS_KEY=$(az keyvault secret show --name terraform-backend-key --vault-name myKeyVault --query value -o tsv)
```

Terrayform 'u arka ucunu kullanacak şekilde yapılandırmak için aşağıdaki adımların gerçekleştirilmesi gerekir:
- Bir tür `azurerm`olan `backend` bir yapılandırma bloğu ekleyin.
- Yapılandırma bloğuna bir `storage_account_name` değeri ekleyin.
- Yapılandırma bloğuna bir `container_name` değeri ekleyin.
- Yapılandırma bloğuna bir `key` değeri ekleyin.

Aşağıdaki örnek bir Terrayform arka ucunu yapılandırır ve bir Azure Kaynak grubu oluşturur.

```hcl
terraform {
  backend "azurerm" {
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

Aşağıdaki adımları uygulayarak yapılandırmayı başlatın:

1. `terraform init` komutunu çalıştırın.
1. `terraform apply` komutunu çalıştırın.

Artık Azure Depolama Blobu durum dosyasını bulabilirsiniz.

## <a name="state-locking"></a>Durum kilitleme

Azure depolama Blobları, durum yazan herhangi bir işlemden önce otomatik olarak kilitlenir. Bu model, eş zamanlı durum işlemlerini önler ve bu da bozulmaya neden olabilir. 

Daha fazla bilgi için bkz. Terrayform belgelerindeki [durum kilitleme](https://www.terraform.io/docs/state/locking.html) .

Azure portal veya diğer Azure yönetim araçları aracılığıyla blob incelenirken kilit görülebilir.

![Kilitleme ile Azure blobu](media/terraform-backend/lock.png)

## <a name="encryption-at-rest"></a>Bekleme sırasında şifreleme

Bir Azure Blobuna depolanan veriler kalıcı olmadan önce şifrelenir. Gerekli olduğunda Terrayform, durumu arka uca alır ve yerel bellekte depolar. Bu düzenin kullanıldığı durum, yerel diskinize hiçbir şekilde yazılmaz.

Azure depolama şifrelemesi hakkında daha fazla bilgi için bkz. [bekleyen veri Için azure depolama hizmeti şifrelemesi](../storage/common/storage-service-encryption.md).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Azure 'da terrayform](/azure/ansible/)
