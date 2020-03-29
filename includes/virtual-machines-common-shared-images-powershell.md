---
title: include dosyası
description: include dosyası
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 05/21/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: bae66078a1bcb1d80f0798b1d501598fa785fb80
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "66241223"
---
## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell'i başlatma

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. 

Cloud Shell'i açmak için kod bloğunun sağ üst köşesinden **Deneyin**'i seçmeniz yeterlidir. Ayrıca bulut shell'i ayrı bir tarayıcı [https://shell.azure.com/powershell](https://shell.azure.com/powershell)sekmesinde başlatabilirsiniz. **Kopyala**’yı seçerek kod bloğunu kopyalayın, Cloud Shell’e yapıştırın ve Enter tuşuna basarak çalıştırın.


## <a name="get-the-managed-image"></a>Yönetilen görüntüyü elde edin

[Get-AzImage'ı](https://docs.microsoft.com/powershell/module/az.compute/get-azimage)kullanarak kaynak grubunda bulunan resimlerin listesini görebilirsiniz. Görüntü adını ve hangi kaynak grubunda olduğunu bildiğinizde, `Get-AzImage` görüntü nesnesini almak ve daha sonra kullanmak üzere bir değişkende depolamak için yeniden kullanabilirsiniz. Bu örnek, "myResourceGroup" kaynak grubundan *myImage* adlı bir görüntü alır ve *$managedImage*değişkenine atar. 

```azurepowershell-interactive
$managedImage = Get-AzImage `
   -ImageName myImage `
   -ResourceGroupName myResourceGroup
```

## <a name="create-an-image-gallery"></a>Resim galerisi oluşturma 

Resim galerisi, görüntü paylaşımını etkinleştirmek için kullanılan birincil kaynaktır. Galeri adı için izin verilen karakterler büyük veya küçük harfler, basamaklar, noktalar ve dönemlerdir. Galeri adı tire içeremez. Galeri adları aboneliğinizde benzersiz olmalıdır. 

[New-AzGallery'yi](https://docs.microsoft.com/powershell/module/az.compute/new-azgallery)kullanarak bir resim galerisi oluşturun. Aşağıdaki örnek, *myGalleryRG* kaynak grubunda *myGallery* adlı bir galeri oluşturur.

```azurepowershell-interactive
$resourceGroup = New-AzResourceGroup `
   -Name 'myGalleryRG' `
   -Location 'West Central US'  
$gallery = New-AzGallery `
   -GalleryName 'myGallery' `
   -ResourceGroupName $resourceGroup.ResourceGroupName `
   -Location $resourceGroup.Location `
   -Description 'Shared Image Gallery for my organization'  
```
   
## <a name="create-an-image-definition"></a>Görüntü tanımı oluşturma 

Görüntü tanımları görüntüler için mantıksal bir gruplandırma oluşturur. Bunlar, içlerinde oluşturulan görüntü sürümleri hakkındaki bilgileri yönetmek için kullanılır. Resim tanım adları büyük veya küçük harflerden, basamaklardan, noktalardan, tirelerden ve dönemlerden oluşur. Görüntü tanımı için belirtebileceğiniz değerler hakkında daha fazla bilgi için [Resim tanımlarına](https://docs.microsoft.com/azure/virtual-machines/windows/shared-image-galleries#image-definitions)bakın.

[New-AzGalleryImageDefinition](https://docs.microsoft.com/powershell/module/az.compute/new-azgalleryimageversion)kullanarak resim tanımı oluşturun. Bu örnekte, galeri resmi *myGalleryImage*olarak adlandırılır.

```azurepowershell-interactive
$galleryImage = New-AzGalleryImageDefinition `
   -GalleryName $gallery.Name `
   -ResourceGroupName $resourceGroup.ResourceGroupName `
   -Location $gallery.Location `
   -Name 'myImageDefinition' `
   -OsState generalized `
   -OsType Windows `
   -Publisher 'myPublisher' `
   -Offer 'myOffer' `
   -Sku 'mySKU'
```


## <a name="create-an-image-version"></a>Resim sürümü oluşturma

[New-AzGalleryImageVersion'u](https://docs.microsoft.com/powershell/module/az.compute/new-azgalleryimageversion)kullanarak yönetilen bir görüntüden görüntü sürümü oluşturun. 

Görüntü sürümü için izin verilen karakterler sayılar ve dönemlerdir. Sayılar 32 bitlik bir tamsayı aralığında olmalıdır. Biçim: *MajorVersion*. *MinorVersion*. *Yama*.

Bu örnekte, görüntü sürümü *1.0.0'dır* ve hem *Batı Orta ABD* hem de Güney Orta *ABD* veri merkezlerine çoğaltılır. Çoğaltma için hedef bölgeleri seçerken, *kaynak* bölgeyi çoğaltma hedefi olarak da eklemeniz gerektiğini unutmayın.


```azurepowershell-interactive
$region1 = @{Name='South Central US';ReplicaCount=1}
$region2 = @{Name='West Central US';ReplicaCount=2}
$targetRegions = @($region1,$region2)
$job = $imageVersion = New-AzGalleryImageVersion `
   -GalleryImageDefinitionName $galleryImage.Name `
   -GalleryImageVersionName '1.0.0' `
   -GalleryName $gallery.Name `
   -ResourceGroupName $resourceGroup.ResourceGroupName `
   -Location $resourceGroup.Location `
   -TargetRegion $targetRegions  `
   -Source $managedImage.Id.ToString() `
   -PublishingProfileEndOfLifeDate '2020-01-01' `
   -asJob 
```

Görüntüyü tüm hedef bölgelere kopyalamak biraz zaman alabilir, bu yüzden ilerlemeyi izleyebilmemiz için bir iş yarattık. İşin ilerlemesini görmek için `$job.State`yazın.

```azurepowershell-interactive
$job.State
```

> [!NOTE]
> Başka bir görüntü sürümü oluşturmak için aynı yönetilen görüntüyü kullanabilmeniz için önce görüntü sürümünün tamamen oluşturulmasını ve çoğaltılmasını beklemeniz gerekir. 
>
> Ayrıca, resim sürümünü oluştururken ekleyerek `-StorageAccountType Standard_ZRS` resim sürümünüzü Bölge [Yedekli Depolama'da](https://docs.microsoft.com/azure/storage/common/storage-redundancy-zrs) da saklayabilirsiniz.
>


## <a name="share-the-gallery"></a>Galeriyi paylaşın

Görüntü galerisi düzeyinde erişimi paylaşmanızı öneririz. Kullanıcı için nesne kimliğini almak için bir e-posta adresi ve [Get-AzADUser](/powershell/module/az.resources/get-azaduser) cmdlet kullanın, ardından galeriye erişim sağlamak için [New-AzRoleAssignment'ı](/powershell/module/Az.Resources/New-AzRoleAssignment) kullanın. Bu alinne_montes@contoso.com örnekteki örnek e-postayı kendi bilgilerinizle değiştirin.

```azurepowershell-interactive
# Get the object ID for the user
$user = Get-AzADUser -StartsWith alinne_montes@contoso.com
# Grant access to the user for our gallery
New-AzRoleAssignment `
   -ObjectId $user.Id `
   -RoleDefinitionName Reader `
   -ResourceName $gallery.Name `
   -ResourceType Microsoft.Compute/galleries `
   -ResourceGroupName $resourceGroup.ResourceGroupName
```