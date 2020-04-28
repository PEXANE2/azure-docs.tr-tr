---
title: include dosyası
description: include dosyası
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 09/20/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 1ec3ecdafb8e475f5f13372789528612ccd7b8b9
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "66226051"
---
## <a name="using-rbac-to-share-images"></a>Görüntüleri paylaşmak için RBAC kullanma

Rol tabanlı Access Control (RBAC) kullanarak, abonelikler arasında görüntü paylaşabilirsiniz. Abonelikler arasında bile, bir görüntü sürümü üzerinde okuma izinlerine sahip olan tüm kullanıcılar, görüntü sürümünü kullanarak bir sanal makine dağıtacaktır.

RBAC kullanarak kaynakları paylaşma hakkında daha fazla bilgi için bkz. [RBAC ve Azure CLI kullanarak erişimi yönetme](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli).


## <a name="list-information"></a>Liste bilgileri

[Az SIG List](/cli/azure/sig#az-sig-list)kullanarak kullanılabilir görüntü galerileriyle ilgili konum, durum ve diğer bilgileri alın.

```azurecli-interactive 
az sig list -o table
```

İşletim sistemi türü ve durumu hakkında bilgiler de dahil olmak üzere, bir galerisindeki görüntü tanımlarını listeleyin; [az Sig Image-Definition List](/cli/azure/sig/image-definition#az-sig-image-definition-list)kullanılarak.

```azurecli-interactive 
az sig image-definition list --resource-group myGalleryRG --gallery-name myGallery -o table
```

Paylaşılan görüntü sürümlerini [az Sig Image-Version List](/cli/azure/sig/image-version#az-sig-image-version-list)kullanarak bir galeride listeleyin.

```azurecli-interactive
az sig image-version list --resource-group myGalleryRG --gallery-name myGallery --gallery-image-definition myImageDefinition -o table
```

[Az Sig Image-Version Show](/cli/azure/sig/image-version#az-sig-image-version-show)kullanarak bir görüntü sürümünün kimliğini alın.

```azurecli-interactive
az sig image-version show \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --gallery-image-version 1.0.0 \
   --query "id"
```