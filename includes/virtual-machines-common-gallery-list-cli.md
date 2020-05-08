---
title: include dosyası
description: include dosyası
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 01/28/2020
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 0e8972b1b2bfaac12baee1ea823429749ed70461
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/05/2020
ms.locfileid: "82792760"
---
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