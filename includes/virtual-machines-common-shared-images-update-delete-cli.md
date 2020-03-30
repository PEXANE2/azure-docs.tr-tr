---
title: include dosyası
description: include dosyası
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 04/25/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 8d0f9866864ca4b02ca6238be2ac44537a586c2d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188383"
---
## <a name="update-resources"></a>Kaynakları güncelleştirme

Güncelleştirilebilenler hakkında bazı sınırlamalar vardır. Aşağıdaki öğeler güncelleştirilebilir: 

Paylaşılan resim galerisi:
- Açıklama

Resim tanımı:
- Önerilen vCPUs
- Önerilen bellek
- Açıklama
- Yaşam sonu tarihi

Resim sürümü:
- Bölgesel yineleme sayısı
- Hedef bölgeler
- En son dışlama
- Yaşam sonu tarihi

Yineleme bölgeleri eklemeyi planlıyorsanız, kaynak yönetilen görüntüyü silmeyin. Kaynak yönetilen görüntü ek bölgelere görüntü sürümü çoğaltmak için gereklidir. 

Kullanarak bir galeri açıklamasını[güncelleştirin](https://docs.microsoft.com/cli/azure/sig?view=azure-cli-latest#az-sig-update)( az sig güncelleme . 

```azurecli-interactive
az sig update \
   --gallery-name myGallery \
   --resource-group myGalleryRG \
   --set description="My updated description."
```


Az sig görüntü tanım [güncelleştirmesini](https://docs.microsoft.com/cli/azure/sig/image-definition?view=azure-cli-latest#az-sig-image-definition-update)kullanarak görüntü tanımının açıklamasını güncelleştirin.

```azurecli-interactive
az sig image-definition update \
   --gallery-name myGallery\
   --resource-group myGalleryRG \
   --gallery-image-definition myImageDefinition \
   --set description="My updated description."
```

[Az sig image-version güncelleştirmesini](https://docs.microsoft.com/cli/azure/sig/image-definition?view=azure-cli-latest#az-sig-image-definition-update)kullanarak çoğaltmak için bir bölge eklemek için bir resim sürümünü güncelleştirin. Görüntü yeni bölgeye çoğaltıldınkça bu değişiklik biraz zaman alacaktır.

```azurecli-interactive
az sig image-version update \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --gallery-image-version 1.0.0 \
   --add publishingProfile.targetRegions  name=eastus
```

## <a name="delete-resources"></a>Kaynakları silme

Önce resim sürümünü silerek kaynakları ters sırada silmeniz gerekir. Tüm resim sürümlerini sildikten sonra, resim tanımını silebilirsiniz. Tüm resim tanımlarını sildikten sonra galeriyi silebilirsiniz. 

[Az sig image-version delete](https://docs.microsoft.com/cli/azure/sig/image-version?view=azure-cli-latest#az-sig-image-version-delete)kullanarak bir resim sürümünü silin.

```azurecli-interactive
az sig image-version delete \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --gallery-image-version 1.0.0 
```

[Az sig resim tanımı nı kullanarak](https://docs.microsoft.com/cli/azure/sig/image-definition?view=azure-cli-latest#az-sig-image-definition-delete)resim tanımını silme silin.

```azurecli-interactive
az sig image-definition delete \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition
```


Az sig delete kullanarak resim [galerisini silin.](https://docs.microsoft.com/cli/azure/sig?view=azure-cli-latest#az-sig-delete)

```azurecli-interactive
az sig delete \
   --resource-group myGalleryRG \
   --gallery-name myGallery
```
