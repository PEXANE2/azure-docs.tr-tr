---
title: Azure'da galeri resimlerini kiracılar arasında paylaşın
description: Paylaşılan Resim Galerileri'ni kullanarak VM görüntülerini Azure kiracıları arasında nasıl paylaşılamayın gerektiğini öğrenin.
author: cynthn
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: conceptual
ms.date: 04/05/2019
ms.author: cynthn
ms.openlocfilehash: a29999102ad8a10d8965145b31a7d804675e0e57
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76276333"
---
# <a name="share-gallery-vm-images-across-azure-tenants"></a>Galeri VM görüntülerini Azure kiracıları arasında paylaşın

[!INCLUDE [virtual-machines-share-images-across-tenants](../../includes/virtual-machines-share-images-across-tenants.md)]


## <a name="create-a-scale-set-using-azure-cli"></a>Azure CLI kullanarak ölçek kümesi oluşturma

AppID, uygulama anahtarı ve kiracı kimliği 1'i kullanarak kiracı 1 için hizmet ilkesini oturum açın. Gerekirse kiracı `az account show --query "tenantId"` nın doğrulanmış larını almak için kullanabilirsiniz.

```azurecli-interactive
az account clear
az login --service-principal -u '<app ID>' -p '<Secret>' --tenant '<tenant 1 ID>'
az account get-access-token 
```
 
AppID,uygulama anahtarı ve kiracı kimliği 2'yi kullanarak kiracı 2 için hizmet ilkesini oturum açın:

```azurecli-interactive
az login --service-principal -u '<app ID>' -p '<Secret>' --tenant '<tenant 2 ID>'
az account get-access-token
```

Ölçek kümesini oluşturun. Örnekteki bilgileri kendi bilginizle değiştirin.

```azurecli-interactive
az vmss create \
  -g myResourceGroup \
  -n myScaleSet \
  --image "/subscriptions/<Tenant 1 subscription>/resourceGroups/<Resource group>/providers/Microsoft.Compute/galleries/<Gallery>/images/<Image definition>/versions/<version>" \
  --admin-username azureuser \
  --generate-ssh-keys
```

## <a name="next-steps"></a>Sonraki adımlar

Herhangi bir sorunla karşınıza çıkarsa, [paylaşılan resim galerilerini sorun giderebilirsiniz.](troubleshooting-shared-images.md)
