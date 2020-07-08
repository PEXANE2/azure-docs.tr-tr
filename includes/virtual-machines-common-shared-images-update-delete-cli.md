---
title: dosya dahil etme
description: dosya dahil etme
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 04/25/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 8d0f9866864ca4b02ca6238be2ac44537a586c2d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "67188383"
---
## <a name="update-resources"></a>Kaynakları güncelleştirme

Nelerin güncelleştirilemeyebilir hakkında bazı sınırlamalar vardır. Aşağıdaki öğeler güncelleştirilemeyebilir: 

Paylaşılan görüntü Galerisi:
- Açıklama

Görüntü tanımı:
- Önerilen vCPU 'Lar
- Önerilen bellek
- Açıklama
- Yaşam tarihi sonu

Görüntü sürümü:
- Bölgesel çoğaltma sayısı
- Hedef bölgeler
- En son dışarıda bırakma
- Yaşam tarihi sonu

Çoğaltma bölgelerini eklemeyi planlıyorsanız, kaynak yönetilen görüntüyü silmeyin. Kaynak yönetilen görüntü, görüntü sürümünü ek bölgelere çoğaltmak için gereklidir. 

([Az SIG Update](https://docs.microsoft.com/cli/azure/sig?view=azure-cli-latest#az-sig-update)) kullanarak bir galerinin açıklamasını güncelleştirin. 

```azurecli-interactive
az sig update \
   --gallery-name myGallery \
   --resource-group myGalleryRG \
   --set description="My updated description."
```


[Az Sig Image-Definition Update](https://docs.microsoft.com/cli/azure/sig/image-definition?view=azure-cli-latest#az-sig-image-definition-update)kullanarak bir görüntü tanımının açıklamasını güncelleştirin.

```azurecli-interactive
az sig image-definition update \
   --gallery-name myGallery\
   --resource-group myGalleryRG \
   --gallery-image-definition myImageDefinition \
   --set description="My updated description."
```

[Az Sig Image-Version Update](https://docs.microsoft.com/cli/azure/sig/image-definition?view=azure-cli-latest#az-sig-image-definition-update)kullanılarak çoğaltılacak bir bölge eklemek için görüntü sürümünü güncelleştirin. Görüntü yeni bölgeye çoğaltılmasıyla bu değişiklik biraz zaman alır.

```azurecli-interactive
az sig image-version update \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --gallery-image-version 1.0.0 \
   --add publishingProfile.targetRegions  name=eastus
```

## <a name="delete-resources"></a>Kaynakları silme

Önce görüntü sürümünü silerek kaynakları ters sırada silmeniz gerekir. Tüm görüntü sürümlerini sildikten sonra, görüntü tanımını silebilirsiniz. Tüm görüntü tanımlarını sildikten sonra, galeriyi silebilirsiniz. 

[Az Sig Image-Version Delete](https://docs.microsoft.com/cli/azure/sig/image-version?view=azure-cli-latest#az-sig-image-version-delete)kullanarak bir görüntü sürümünü silin.

```azurecli-interactive
az sig image-version delete \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --gallery-image-version 1.0.0 
```

[Az Sig Image-Definition Delete](https://docs.microsoft.com/cli/azure/sig/image-definition?view=azure-cli-latest#az-sig-image-definition-delete)kullanarak bir görüntü tanımını silin.

```azurecli-interactive
az sig image-definition delete \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition
```


[Az Sig Delete](https://docs.microsoft.com/cli/azure/sig?view=azure-cli-latest#az-sig-delete)kullanarak bir görüntü Galerisi silin.

```azurecli-interactive
az sig delete \
   --resource-group myGalleryRG \
   --gallery-name myGallery
```
