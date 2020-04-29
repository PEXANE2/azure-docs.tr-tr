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
ms.sourcegitcommit: be32c9a3f6ff48d909aabdae9a53bd8e0582f955
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2020
ms.locfileid: "67188334"
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

Bir galerinin açıklamasını güncelleştirmek için [Update-AzGallery](https://docs.microsoft.com/powershell/module/az.compute/update-azgallery)komutunu kullanın.

```azurepowershell-interactive
Update-AzGallery `
   -Name $gallery.Name ` 
   -ResourceGroupName $resourceGroup.Name
```

Bu örnek, görüntü tanımımız için son kullanım tarihini güncelleştirmek üzere [Update-Azgallerımagedefinition](https://docs.microsoft.com/powershell/module/az.compute/update-azgalleryimagedefinition) öğesinin nasıl kullanılacağını gösterir.

```azurepowershell-interactive
Update-AzGalleryImageDefinition `
   -GalleryName $gallery.Name `
   -Name $galleryImage.Name `
   -ResourceGroupName $resourceGroup.Name `
   -EndOfLifeDate 01/01/2030
```

Bu örnek, bu görüntü sürümünün *en son* görüntü olarak kullanılmasını dışlamak için [Update-azgallerımageversion](https://docs.microsoft.com/powershell/module/az.compute/update-azgalleryimageversion) ' ın nasıl kullanılacağını gösterir.

```azurepowershell-interactive
Update-AzGalleryImageVersion `
   -GalleryImageDefinitionName $galleryImage.Name `
   -GalleryName $gallery.Name `
   -Name $galleryVersion.Name `
   -ResourceGroupName $resourceGroup.Name `
   -PublishingProfileExcludeFromLatest
```


## <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynakları silerken, iç içe kaynaklardaki son öğeyle başlamanız gerekir-görüntü sürümü. Sürümler silindikten sonra, görüntü tanımını silebilirsiniz. Altındaki tüm kaynaklar silinene kadar galeriyi silemezsiniz.

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

