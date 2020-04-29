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
ms.sourcegitcommit: be32c9a3f6ff48d909aabdae9a53bd8e0582f955
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2020
ms.locfileid: "66241223"
---
## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell'i başlatma

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. 

Cloud Shell'i açmak için kod bloğunun sağ üst köşesinden **Deneyin**'i seçmeniz yeterlidir. Ayrıca, ' a giderek ayrı bir tarayıcı sekmesinde Cloud Shell de başlatabilirsiniz [https://shell.azure.com/powershell](https://shell.azure.com/powershell). **Kopyala**’yı seçerek kod bloğunu kopyalayın, Cloud Shell’e yapıştırın ve Enter tuşuna basarak çalıştırın.


## <a name="get-the-managed-image"></a>Yönetilen görüntüyü al

[Get-Azımage](https://docs.microsoft.com/powershell/module/az.compute/get-azimage)kullanarak bir kaynak grubunda kullanılabilen görüntülerin listesini görebilirsiniz. Görüntü adını ve içindeki kaynak grubunu öğrendikten sonra, görüntü nesnesini almak ve daha sonra kullanmak üzere `Get-AzImage` bir değişkende depolamak için yeniden kullanabilirsiniz. Bu örnek, "myResourceGroup" kaynak grubundan *MyImage* adlı bir görüntü alır ve onu *$managedImage*değişkenine atar. 

```azurepowershell-interactive
$managedImage = Get-AzImage `
   -ImageName myImage `
   -ResourceGroupName myResourceGroup
```

## <a name="create-an-image-gallery"></a>Görüntü galerisi oluşturma 

Görüntü Galerisi, görüntü paylaşımını etkinleştirmek için kullanılan birincil kaynaktır. Galeri adı için izin verilen karakterler büyük veya küçük harflerden, rakamlardan, noktalardan ve noktalardan oluşur. Galeri adı tire içeremez. Galeri adları, aboneliğiniz dahilinde benzersiz olmalıdır. 

[New-AzGallery](https://docs.microsoft.com/powershell/module/az.compute/new-azgallery)kullanarak bir görüntü galerisi oluşturun. Aşağıdaki örnek *Mygallerrg* kaynak grubunda *MyGallery* adlı bir galeri oluşturur.

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

Görüntü tanımları görüntüler için bir mantıksal gruplama oluşturur. Bunlar içinde oluşturulan görüntü sürümleri hakkındaki bilgileri yönetmek için kullanılır. Görüntü tanımı adları büyük veya küçük harflerden, rakamlardan, noktalardan, çizgilerden ve noktalardan oluşabilir. Bir görüntü tanımı için belirtebileceğiniz değerler hakkında daha fazla bilgi için bkz. [görüntü tanımları](https://docs.microsoft.com/azure/virtual-machines/windows/shared-image-galleries#image-definitions).

[New-Azgallerımagedefinition](https://docs.microsoft.com/powershell/module/az.compute/new-azgalleryimageversion)kullanarak görüntü tanımını oluşturun. Bu örnekte, Galeri görüntüsü *Mygallerımage*olarak adlandırılmıştır.

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


## <a name="create-an-image-version"></a>Görüntü sürümü oluşturma

[New-Azgallerımageversion](https://docs.microsoft.com/powershell/module/az.compute/new-azgalleryimageversion)kullanarak yönetilen görüntüden bir görüntü sürümü oluşturun. 

Görüntü sürümü için izin verilen karakterler rakamlardan ve dönemlerdir. Sayılar 32 bitlik bir tamsayı aralığında olmalıdır. Biçim: *MajorVersion*. *MinorVersion*. *Düzeltme Eki*.

Bu örnekte, görüntü sürümü *1.0.0* ve hem *Orta Batı ABD* hem de *Orta Güney ABD* veri merkezlerine çoğaltılır. Çoğaltma için hedef bölge seçerken, *kaynak* bölgeyi çoğaltma için hedef olarak da dahil etmeniz gerektiğini unutmayın.


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

Görüntünün tüm hedef bölgelere çoğaltılması biraz zaman alabilir, bu nedenle ilerlemeyi izleyebilmemiz için bir iş oluşturduk. İşin ilerlemesini görmek için, yazın `$job.State`.

```azurepowershell-interactive
$job.State
```

> [!NOTE]
> Farklı bir görüntü sürümü oluşturmak için aynı yönetilen görüntüyü kullanabilmeniz için görüntü sürümünün oluşturulması ve çoğaltılması tamamen bitmesini beklemeniz gerekir. 
>
> Görüntü sürümünü oluştururken ekleyerek `-StorageAccountType Standard_ZRS` görüntü sürümünüzü [bölge yedekli depolama](https://docs.microsoft.com/azure/storage/common/storage-redundancy-zrs) alanında da saklayabilirsiniz.
>


## <a name="share-the-gallery"></a>Galeriyi paylaşma

Görüntü Galerisi düzeyinde erişimi paylaşmanızı öneririz. Kullanıcının nesne KIMLIĞINI almak için bir e-posta adresi ve [Get-AzADUser](/powershell/module/az.resources/get-azaduser) cmdlet 'ini kullanın, ardından galeriye erişim sağlamak için [New-azroleatama](/powershell/module/Az.Resources/New-AzRoleAssignment) kullanın. Örnek e-postayı, alinne_montes@contoso.com Bu örnekte kendi bilgileriniz ile değiştirin.

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