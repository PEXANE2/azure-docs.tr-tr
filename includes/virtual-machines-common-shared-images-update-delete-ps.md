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
ms.openlocfilehash: d2a85f3947e9993e5d1853e45c6d03586a074cf6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188334"
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

Galerinin açıklamasını güncelleştirmek için [Update-AzGallery'yi](https://docs.microsoft.com/powershell/module/az.compute/update-azgallery)kullanın.

```azurepowershell-interactive
Update-AzGallery `
   -Name $gallery.Name ` 
   -ResourceGroupName $resourceGroup.Name
```

Bu örnek, görüntü tanımımız için kullanım ömrünün bitiş tarihini güncelleştirmek için [Update-AzGalleryImageDefinition'in](https://docs.microsoft.com/powershell/module/az.compute/update-azgalleryimagedefinition) nasıl kullanılacağını gösterir.

```azurepowershell-interactive
Update-AzGalleryImageDefinition `
   -GalleryName $gallery.Name `
   -Name $galleryImage.Name `
   -ResourceGroupName $resourceGroup.Name `
   -EndOfLifeDate 01/01/2030
```

Bu örnek, bu resim sürümünün *en son* resim olarak kullanılmasını engellemek için [Update-AzGalleryImageVersion'un](https://docs.microsoft.com/powershell/module/az.compute/update-azgalleryimageversion) nasıl kullanılacağını gösterir.

```azurepowershell-interactive
Update-AzGalleryImageVersion `
   -GalleryImageDefinitionName $galleryImage.Name `
   -GalleryName $gallery.Name `
   -Name $galleryVersion.Name `
   -ResourceGroupName $resourceGroup.Name `
   -PublishingProfileExcludeFromLatest
```


## <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynakları silerken, iç içe geçen kaynaklardaki son öğeyle başlamanız gerekir - resim sürümü. Sürümler silindikten sonra, resim tanımını silebilirsiniz. Altındaki tüm kaynaklar silinene kadar galeriyi silemezsiniz.

```azurepowershell-interactive
$resourceGroup = "myResourceGroup"
$gallery = "myGallery"
$imageDefinition = "myImageDefinition"
$imageVersion = "myImageVersion"

Remove-AzGalleryImageVersion `
   -GalleryImageDefinitionName $imageDefinition `
   -GalleryName $gallery `
   -Name $imageVersion `
   -ResourceGroupName $resourceGroup

Remove-AzGalleryImageDefinition `
   -ResourceGroupName $resourceGroup `
   -GalleryName $gallery `
   -GalleryImageDefinitionName $imageDefinition

Remove-AzGallery `
   -Name $gallery `
   -ResourceGroupName $resourceGroup

Remove-AzResourceGroup -Name $resourceGroup
```

