---
title: Görüntü sürümünden yönetilen disk oluşturma
description: Paylaşılan görüntü galerisinde bir görüntü sürümünden yönetilen disk oluşturun.
author: cynthn
ms.service: virtual-machines
ms.subservice: imaging
ms.topic: how-to
ms.workload: infrastructure
ms.date: 10/06/2020
ms.author: cynthn
ms.reviewer: olayemio
ms.openlocfilehash: bf4a1feb91a1ac4b0bca0d6afdbac41a8be3aa4f
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92049881"
---
# <a name="create-a-managed-disk-from-an-image-version"></a>Görüntü sürümünden yönetilen disk oluşturma

Gerekirse, paylaşılan görüntü galerisinde depolanan bir görüntü sürümünden yönetilen bir disk yapabilirsiniz.


## <a name="cli"></a>CLI

`source`Değişkeni görüntü sürümünün kimliği olarak ayarlayın, ardından, yönetilen diski oluşturmak için [az disk Create](/cli/azure/disk.md#az_disk_create) kullanın. 


[Az Sig Image-Version List](/cli/azure/sig/image-version.md#az_sig_image_version_list)kullanarak bir liste görüntüsü sürümlerini görebilirsiniz. Bu örnekte, *MyImage* görüntü galerisinde *myımagedefinition* görüntü tanımının bir parçası olan tüm görüntü sürümlerini arıyoruz.

```azurecli-interactive
az sig image-version list \
   --resource-group myResourceGroup\
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   -o table
```


Bu örnekte, *Myresourcegroup*adlı bir kaynak grubundaki *EastUS* bölgesinde *mymanageddisk*adlı bir yönetilen disk oluşturacağız. 

```azurecli-interactive
source="/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.Compute/galleries/<galleryName>/images/<galleryImageDefinition>/versions/<imageVersion>"

az disk create --resource-group myResourceGroup --location EastUS --name myManagedDisk --gallery-image-reference $source 
```

Disk bir veri diskise, `--gallery-image-reference-lun` LUN 'yi belirtmek için ekleyin.

## <a name="powershell"></a>PowerShell

[Get-AzResource](/powershell/module/az.resources/get-azresource)komutunu kullanarak tüm görüntü sürümlerini listeleyebilirsiniz. 

```azurepowershell-interactive
Get-AzResource `
   -ResourceType Microsoft.Compute/galleries/images/versions | `
   Format-Table -Property Name,ResourceId,ResourceGroupName
```



İhtiyacınız olan tüm bilgilere sahip olduktan sonra, kullanmak istediğiniz kaynak görüntü sürümünü almak ve bir değişkene atamak için [Get-Azgallerımageversion](/powershell/module/az.compute/get-azgalleryimageversion) kullanabilirsiniz. Bu örnekte, kaynak `1.0.0` `myImageDefinition` galerisindeki tanımın görüntü sürümünü `myGallery` `myResourceGroup` kaynak grubunda alıyoruz.

```azurepowershell-interactive
$sourceImgVer = Get-AzGalleryImageVersion `
   -GalleryImageDefinitionName myImageDefinition `
   -GalleryName myGallery `
   -ResourceGroupName myResourceGroup `
   -Name 1.0.0
```

Disk bilgileri için bazı değişkenler ayarlayın.

```azurepowershell-interactive
$location = "East US"
$resourceGroup = "myResourceGroup"
$diskName = "myDisk"
```

Kaynak görüntü sürümü KIMLIĞINI kullanarak bir disk yapılandırması ve ardından disk oluşturun. İçin `-GalleryImageReference` , LUN yalnızca kaynak bir veri diski ise gereklidir.

```azurepowershell-interactive
$diskConfig = New-AzDiskConfig `
   -Location $location `
   -CreateOption FromImage `
   -GalleryImageReference @{Id = $sourceImgVer.Id; Lun=1}
```

Diski oluşturun.

```azurepowershell-interactive
New-AzDisk -Disk $diskConfig `
   -ResourceGroupName $resourceGroup `
   -DiskName $diskName
```

## <a name="next-steps"></a>Sonraki adımlar

Ayrıca, [Azure CLI](image-version-managed-image-cli.md) veya [PowerShell](image-version-managed-image-powershell.md)kullanarak yönetilen bir diskten görüntü sürümü de oluşturabilirsiniz.


