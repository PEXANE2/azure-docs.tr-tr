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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "66226051"
---
## <a name="using-rbac-to-share-images"></a>Görüntüleri paylaşmak için RBAC'ı kullanma

Role Based Access Control (RBAC) kullanarak görüntüleri abonelikler arasında paylaşabilirsiniz. Abonelikler arasında bile bir resim sürümü için izinleri okuyan herhangi bir kullanıcı, görüntü sürümünü kullanarak bir Sanal Makine dağıtabilir.

RBAC kullanarak kaynakların nasıl paylaşılabildiğini öğrenmek için [Bkz. RBAC ve Azure CLI kullanarak erişimi yönet.](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli)


## <a name="list-information"></a>Liste bilgileri

[Az sig listesini](/cli/azure/sig#az-sig-list)kullanarak kullanılabilir resim galerileri hakkında konum, durum ve diğer bilgileri alın.

```azurecli-interactive 
az sig list -o table
```

[Az sig resim tanım listesini](/cli/azure/sig/image-definition#az-sig-image-definition-list)kullanarak işletim sistemi türü ve durumu hakkında bilgiler de dahil olmak üzere bir galerideki resim tanımlarını listeleyin.

```azurecli-interactive 
az sig image-definition list --resource-group myGalleryRG --gallery-name myGallery -o table
```

[Az sig image-version listesini](/cli/azure/sig/image-version#az-sig-image-version-list)kullanarak paylaşılan resim sürümlerini galeride listeleyin.

```azurecli-interactive
az sig image-version list --resource-group myGalleryRG --gallery-name myGallery --gallery-image-definition myImageDefinition -o table
```

[az sig image-version show](/cli/azure/sig/image-version#az-sig-image-version-show)kullanarak bir resim sürümünün kimliğini alın.

```azurecli-interactive
az sig image-version show \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --gallery-image-version 1.0.0 \
   --query "id"
```