---
title: Azure 'daki kiracılar genelinde Galeri görüntülerini paylaşma
description: Paylaşılan görüntü galerileri kullanarak Azure kiracılarının tamamında VM görüntülerini paylaşmayı öğrenin.
author: axayjo
ms.service: virtual-machines
ms.subservice: imaging
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 05/04/2019
ms.author: akjosh
ms.reviewer: cynthn
ms.openlocfilehash: 6560bf452f04ae5d88168b8d3fad300feb90b16f
ms.sourcegitcommit: f844603f2f7900a64291c2253f79b6d65fcbbb0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2020
ms.locfileid: "86220533"
---
# <a name="share-gallery-vm-images-across-azure-tenants"></a>Azure kiracılar genelinde Galeri VM görüntülerini paylaşma

Paylaşılan görüntü galerileri, RBAC kullanarak görüntüleri paylaşmanıza olanak sağlar. RBAC kullanarak kiracınızdaki görüntüleri paylaşabilir ve hatta kiracınızın dışındaki bireyler ekleyebilirsiniz. Bu basit paylaşım seçeneği hakkında daha fazla bilgi için [Galeriyi paylaşma](/azure/virtual-machines/linux/shared-images-portal#share-the-gallery)bölümüne bakın.

[!INCLUDE [virtual-machines-share-images-across-tenants](../../../includes/virtual-machines-share-images-across-tenants.md)]

> [!IMPORTANT]
> Başka bir Azure kiracısındaki görüntüden bir VM dağıtmak için portalını kullanamazsınız. Kiracılar arasında paylaşılan bir görüntüden VM oluşturmak için Azure CLı veya [PowerShell](../windows/share-images-across-tenants.md)kullanmanız gerekir.

## <a name="create-a-vm-using-azure-cli"></a>Azure CLı kullanarak VM oluşturma

AppID, uygulama anahtarı ve kiracı 1 KIMLIĞI kullanarak kiracı 1 için hizmet sorumlusu 'nda oturum açın. `az account show --query "tenantId"`Gerekirse kiracı kimliklerini almak için ' i kullanabilirsiniz.

```azurecli-interactive
az account clear
az login --service-principal -u '<app ID>' -p '<Secret>' --tenant '<tenant 1 ID>'
az account get-access-token 
```
 
AppID, uygulama anahtarı ve kiracı 2 ' nin KIMLIĞINI kullanarak kiracı 2 için hizmet sorumlusu 'nda oturum açın:

```azurecli-interactive
az login --service-principal -u '<app ID>' -p '<Secret>' --tenant '<tenant 2 ID>'
az account get-access-token
```

VM 'yi oluşturun. Örnekteki bilgileri kendi ile değiştirin.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --image "/subscriptions/<Tenant 1 subscription>/resourceGroups/<Resource group>/providers/Microsoft.Compute/galleries/<Gallery>/images/<Image definition>/versions/<version>" \
  --admin-username azureuser \
  --generate-ssh-keys
```

## <a name="next-steps"></a>Sonraki adımlar

Herhangi bir sorunla karşılaşırsanız [paylaşılan görüntü galerilerine sorun giderebilirsiniz](troubleshooting-shared-images.md).