---
title: Azure CLı ile yönetilen bir görüntüden görüntü sürümüne geçiş
description: Yönetilen bir görüntüden Azure CLı kullanarak paylaşılan görüntü galerisindeki görüntü sürümüne geçiş yapmayı öğrenin.
author: cynthn
ms.service: virtual-machines
ms.subservice: imaging
ms.topic: how-to
ms.workload: infrastructure
ms.date: 05/04/2020
ms.author: cynthn
ms.reviewer: akjosh
ms.openlocfilehash: 6f49ece874ea52227e6531193fc53b3bea525702
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/05/2020
ms.locfileid: "82796609"
---
# <a name="migrate-from-a-managed-image-to-an-image-version-using-the-azure-cli"></a>Azure CLı kullanarak yönetilen görüntüden görüntü sürümüne geçiş
Paylaşılan görüntü galerisine geçirmek istediğiniz mevcut bir yönetilen görüntünüz varsa, doğrudan yönetilen görüntüden paylaşılan bir görüntü Galerisi görüntüsü oluşturabilirsiniz. Yeni görüntünüzü sınadıktan sonra, kaynak yönetilen görüntüyü silebilirsiniz. Ayrıca, [PowerShell](image-version-managed-image-powershell.md)kullanarak yönetilen bir görüntüden paylaşılan görüntü galerisine geçiş yapabilirsiniz.

Görüntü galerisindeki görüntülerin iki bileşeni vardır ve bu örnekte oluşturacağız:
- **Görüntü tanımı** , görüntü ve kullanma gereksinimleri hakkında bilgi taşır. Bu, görüntünün Windows veya Linux, özelleştirilmiş veya Genelleştirilmiş, sürüm notları ve en düşük ve en yüksek bellek gereksinimlerini içerir. Bu, bir görüntü türünün tanımıdır. 
- **Görüntü sürümü** , paylaşılan görüntü GALERISI kullanılırken VM oluşturmak için kullanılır. Ortamınız için gerektiğinde bir görüntünün birden fazla sürümüne sahip olabilirsiniz. Bir VM oluşturduğunuzda, sanal makine için yeni diskler oluşturmak üzere görüntü sürümü kullanılır. Görüntü sürümleri birden çok kez kullanılabilir.


## <a name="before-you-begin"></a>Başlamadan önce

Bu makaleyi tamamlayabilmeniz için, var olan bir [paylaşılan görüntü galeriniz](shared-images-cli.md)olması gerekir. 

Bu makaledeki örneği tamamlayabilmeniz için, genelleştirilmiş bir VM 'nin mevcut bir yönetilen görüntüsüne sahip olmanız gerekir. Daha fazla bilgi için bkz. [yönetilen görüntü yakalama](./linux/capture-image.md). Yönetilen görüntü bir veri diski içeriyorsa, veri diski boyutu 1 TB 'den fazla olamaz.

Bu makalede çalışırken, kaynak grubu ve VM adlarını gerektiği yerde değiştirin.



## <a name="create-an-image-definition"></a>Görüntü tanımı oluşturma

Yönetilen görüntüler her zaman Genelleştirilmiş görüntüler olduğundan, genelleştirilmiş bir görüntü için kullanarak `--os-state generalized` bir görüntü tanımı oluşturacaksınız.

Görüntü tanımı adları büyük veya küçük harflerden, rakamlardan, noktalardan, çizgilerden ve noktalardan oluşabilir. 

Bir görüntü tanımı için belirtebileceğiniz değerler hakkında daha fazla bilgi için bkz. [görüntü tanımları](https://docs.microsoft.com/azure/virtual-machines/linux/shared-image-galleries#image-definitions).

Galeride [az Sig Image-Definition Create](/cli/azure/sig/image-definition#az-sig-image-definition-create)kullanarak bir görüntü tanımı oluşturun.

Bu örnekte, görüntü tanımı *Myımagedefinition*olarak adlandırılır ve [Genelleştirilmiş](./linux/shared-image-galleries.md#generalized-and-specialized-images) bir Linux işletim sistemi görüntüsü içindir. Windows işletim sistemi kullanan görüntülerin tanımını oluşturmak için kullanın `--os-type Windows`. 

```azurecli-interactive 
resourceGroup=myGalleryRG
gallery=myGallery
imageDef=myImageDefinition
az sig image-definition create \
   --resource-group $resourceGroup \
   --gallery-name $gallery \
   --gallery-image-definition $imageDef \
   --publisher myPublisher \
   --offer myOffer \
   --sku mySKU \
   --os-type Linux \
   --os-state generalized
```


## <a name="create-the-image-version"></a>Görüntü sürümü oluşturma

[Az Image Gallery Create-Image-Version](/cli/azure/sig/image-version#az-sig-image-version-create)kullanarak sürümler oluşturun. Görüntü sürümü oluşturmak için taban çizgisi olarak kullanılacak yönetilen görüntünün KIMLIĞINI geçirmeniz gerekir. Görüntülerinizin kimliklerini almak için [az Image List](/cli/azure/image?view#az-image-list) kullanabilirsiniz. 

```azurecli-interactive
az image list --query "[].[name, id]" -o tsv
```

Görüntü sürümü için izin verilen karakterler rakamlardan ve dönemlerdir. Sayılar 32 bitlik bir tamsayı aralığında olmalıdır. Biçim: *MajorVersion*. *MinorVersion*. *Düzeltme Eki*.

Bu örnekte, görüntüimizin sürümü *1.0.0* ve bölgesel olarak yedekli depolama alanı kullanarak *Orta Güney ABD* bölgesinde 1 çoğaltma ve *Doğu ABD 2* bölgesinde 1 çoğaltma oluşturacağız. Çoğaltma için hedef bölge seçerken, *kaynak* bölgeyi çoğaltma için hedef olarak da dahil etmeniz gerektiğini unutmayın.

Yönetilen görüntünün KIMLIĞINI `--managed-image` parametreye geçirin.


```azurecli-interactive 
imageID="/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/images/myImage"
az sig image-version create \
   --resource-group $resourceGroup \
   --gallery-name $gallery \
   --gallery-image-definition $imageDef \
   --gallery-image-version 1.0.0 \
   --target-regions "southcentralus=1" "eastus2=1=standard_zrs" \
   --replica-count 2 \
   --managed-image $imageID
```

> [!NOTE]
> Farklı bir görüntü sürümü oluşturmak için aynı yönetilen görüntüyü kullanabilmeniz için görüntü sürümünün oluşturulması ve çoğaltılması tamamen bitmesini beklemeniz gerekir.
>
> Görüntü sürümünü oluştururken ekleyerek `--storage-account-type standard_zrs` görüntü sürümü çoğaltmalarınızı, bölgesel olarak [yedekli depolama](https://docs.microsoft.com/azure/storage/common/storage-redundancy-zrs) alanında da saklayabilirsiniz.
>

## <a name="next-steps"></a>Sonraki adımlar

[Genelleştirilmiş görüntü sürümünden](vm-generalized-image-version-cli.md)bir VM oluşturun.