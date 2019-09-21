---
title: Azure Storage 'ı Terrayform arka ucu olarak kullanma
description: Azure depolama 'da Terrayform durumunu depolamanın bir girişi.
services: terraform
author: tomarchermsft
ms.service: azure
ms.topic: article
ms.date: 09/20/2019
ms.author: tarcher
ms.openlocfilehash: e9b447f4f4dc9d0ee090da9729e483cc17ac7c15
ms.sourcegitcommit: f2771ec28b7d2d937eef81223980da8ea1a6a531
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71169934"
---
# <a name="store-terraform-state-in-azure-storage"></a>Azure depolama 'da Terrayform durumunu depolama

Terrayform, dağıtılmış kaynakları Teraform yapılandırmalarına göre mutabık kılmak için kullanılır. Terrayform, durumu kullanarak ekleme, güncelleştirme veya silme için Azure kaynaklarını biliyor. Terrayform *Apply*, varsayılan olarak, terrayform durumu yerel olarak depolanır. Bu yapılandırma bazı nedenlerle ideal değildir:

- Yerel durum, bir takımda veya işbirliği ortamında iyi çalışmıyor
- Terrayform durumu hassas bilgiler içerebilir
- Durumu yerel olarak depolama, yanlışlıkla silme olasılığını artırır

Terrayform, Terrayform durumu için uzak depolama olan bir durum arka ucu kavramını içerir. Durum arka ucu kullanılırken, durum dosyası Azure depolama gibi bir veri deposunda depolanır. Bu belgede, Azure Storage 'ın bir Terraystate arka ucu olarak nasıl yapılandırılacağı ve kullanılacağı ayrıntılı olarak anlatılmaktadır.

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

Terrayform *init*çalıştırılırken terrayform durumunun arka ucu yapılandırılır. Durum arka ucunu yapılandırmak için aşağıdaki veriler gereklidir.

- storage_account_name-Azure Storage hesabının adı.
- container_name-blob kapsayıcısının adı.
- anahtar-oluşturulacak durum depolama dosyasının adı.
- access_key-depolama erişim anahtarı.

Bu değerlerin her biri Teraform yapılandırma dosyasında veya komut satırında belirtilebilir, ancak için `access_key`bir ortam değişkeni kullanılması önerilir. Bir ortam değişkeni kullanmak, anahtarın diske yazılmasını engeller.

Azure depolama erişim anahtarı değeriyle `ARM_ACCESS_KEY` adlı bir ortam değişkeni oluşturun.

```bash
export ARM_ACCESS_KEY=<storage access key>
```

Azure depolama hesabı erişim anahtarını daha fazla korumak için Azure Key Vault ' de saklayın. Daha sonra, ortam değişkeni aşağıdakine benzer bir komut kullanılarak ayarlanabilir. Azure Key Vault hakkında daha fazla bilgi için [Azure Key Vault belgelerine][azure-key-vault]bakın.

```bash
export ARM_ACCESS_KEY=$(az keyvault secret show --name terraform-backend-key --vault-name myKeyVault --query value -o tsv)
```

Terupform 'u arka ucu kullanacak şekilde yapılandırmak için, Terrayform yapılandırmasında *azurerd* türüne sahip bir *arka uç* yapılandırması ekleyin. Yapılandırma bloğuna *storage_account_name*, *container_name*ve *anahtar* değerleri ekleyin.

Aşağıdaki örnek bir Terrayform arka ucunu yapılandırır ve bir Azure Kaynak grubu oluşturur. Değerleri ortamınızdaki değerlerle değiştirin.

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

Şimdi, bu yapılandırmayı *terarform init* ile başlatıp yapılandırmayı *terrayform Apply*ile çalıştırın. Tamamlandığında, Azure Depolama Blobu durum dosyasını bulabilirsiniz.

## <a name="state-locking"></a>Durum kilitleme

Durum depolaması için bir Azure Depolama Blobu kullanırken, blob, durum yazan herhangi bir işlemden önce otomatik olarak kilitlenir. Bu yapılandırma birden çok eş zamanlı durum işlemini engeller, bu da bozulmaya neden olabilir. Daha fazla bilgi için bkz. Terrayform belgelerindeki [durum kilitleme][terraform-state-lock] .

Azure portal veya diğer Azure yönetim araçları aracılığıyla blob incelenirken kilit görülebilir.

![Kilitleme ile Azure blobu](media/terraform-backend/lock.png)

## <a name="encryption-at-rest"></a>Bekleme sırasında şifreleme

Varsayılan olarak, bir Azure Blobuna depolanan veriler depolama altyapısına kalıcı olmadan önce şifrelenir. Terrampaform 'un durumu gerektiğinde, arka uçta alınır ve geliştirme sisteminizde bellekte depolanır. Bu yapılandırmada durum, Azure depolama 'da güvenli hale getirilir ve yerel diskinize yazılmaz.

Azure depolama şifrelemesi hakkında daha fazla bilgi için bkz. [bekleyen veri Için azure depolama hizmeti şifrelemesi][azure-storage-encryption].

## <a name="next-steps"></a>Sonraki adımlar

[Terlarform][terraform-backend]arka uç belgelerinde Terrayform arka uç yapılandırması hakkında daha fazla bilgi edinin.

<!-- LINKS - internal -->
[azure-key-vault]: ../key-vault/quick-create-cli.md
[azure-storage-encryption]: ../storage/common/storage-service-encryption.md

<!-- LINKS - external -->
[terraform-azurerm]: https://www.terraform.io/docs/backends/types/azurerm.html
[terraform-backend]: https://www.terraform.io/docs/backends/
[terraform-state-lock]: https://www.terraform.io/docs/state/locking.html
