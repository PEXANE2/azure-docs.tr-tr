---
title: VM’den görüntü oluşturma
description: Azure 'daki bir VM 'den paylaşılan görüntü galerisinde görüntü oluşturmayı öğrenin.
author: cynthn
ms.service: virtual-machines
ms.subservice: imaging
ms.topic: how-to
ms.workload: infrastructure
ms.date: 05/01/2020
ms.author: cynthn
ms.reviewer: akjosh
ms.openlocfilehash: f53a6b63c744b0e3e41f7ad22270cd842da57674
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/05/2020
ms.locfileid: "82796583"
---
# <a name="create-an-image-version-from-a-vm-in-azure-using-the-azure-cli"></a>Azure CLı kullanarak Azure 'daki bir VM 'den görüntü sürümü oluşturma

Birden çok, aynı VM 'Leri oluşturmak için kullanmak istediğiniz mevcut bir VM varsa, Azure CLı kullanarak paylaşılan bir görüntü galerisinde görüntü oluşturmak için bu VM 'yi kullanabilirsiniz. Ayrıca, [Azure PowerShell](image-version-vm-powershell.md)kullanarak bir VM 'den görüntü oluşturabilirsiniz.

**Görüntü sürümü** , paylaşılan görüntü GALERISI kullanılırken VM oluşturmak için kullandığınız şeydir. Ortamınız için gerektiğinde bir görüntünün birden fazla sürümüne sahip olabilirsiniz. Bir sanal makine oluşturmak için görüntü sürümü kullandığınızda, yeni VM için disk oluşturmak üzere görüntü sürümü kullanılır. Görüntü sürümleri birden çok kez kullanılabilir.


## <a name="before-you-begin"></a>Başlamadan önce

Bu makaleyi tamamlayabilmeniz için, var olan bir paylaşılan görüntü galeriniz olması gerekir. 

Ayrıca, galeriniz ile aynı bölgede Azure 'da var olan bir VM 'niz olmalıdır. 

VM 'ye eklenmiş bir veri diski varsa, veri diski boyutu 1 TB 'den fazla olamaz.

Bu makalede çalışırken, kaynak adlarını gereken yerde değiştirin.

## <a name="get-information-about-the-vm"></a>VM hakkında bilgi edinme

[Az VM List](/cli/azure/vm#az-vm-list)kullanılarak kullanılabilir olan VM 'lerin bir listesini görebilirsiniz. 

```azurecli-interactive
az vm list --output table
```

VM adını ve içindeki kaynak grubunu öğrendikten sonra [az VM Get-instance-View](/cli/azure/vm#az-vm-get-instance-view)kullanarak VM 'nin kimliğini alın. 

```azurecli-interactive
az vm get-instance-view -g MyResourceGroup -n MyVm --query id
```


## <a name="create-an-image-definition"></a>Görüntü tanımı oluşturma

Görüntü tanımları görüntüler için bir mantıksal gruplama oluşturur. Bunlar içinde oluşturulan görüntü sürümleri hakkındaki bilgileri yönetmek için kullanılır. 

Görüntü tanımı adları büyük veya küçük harflerden, rakamlardan, noktalardan, çizgilerden ve noktalardan oluşabilir. 

Görüntü tanımınızın doğru türde olduğundan emin olun. VM 'yi genelleştirdiğinizde (Windows için Sysprep veya Linux için waagent-deprovision), kullanarak `--os-state generalized`genelleştirilmiş bir görüntü tanımı oluşturmanız gerekir. VM 'yi mevcut kullanıcı hesaplarını kaldırmadan kullanmak istiyorsanız, kullanarak `--os-state specialized`özel bir görüntü tanımı oluşturun.

Bir görüntü tanımı için belirtebileceğiniz değerler hakkında daha fazla bilgi için bkz. [görüntü tanımları](https://docs.microsoft.com/azure/virtual-machines/linux/shared-image-galleries#image-definitions).

Galeride [az Sig Image-Definition Create](/cli/azure/sig/image-definition#az-sig-image-definition-create)kullanarak bir görüntü tanımı oluşturun.

Bu örnekte, görüntü tanımı *Myımagedefinition*olarak adlandırılır ve [özelleştirilmiş](https://docs.microsoft.com/azure/virtual-machines/linux/shared-image-galleries#generalized-and-specialized-images) bir Linux işletim sistemi görüntüsü içindir. Windows işletim sistemi kullanan görüntülerin tanımını oluşturmak için kullanın `--os-type Windows`. 

```azurecli-interactive 
az sig image-definition create \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --publisher myPublisher \
   --offer myOffer \
   --sku mySKU \
   --os-type Linux \
   --os-state specialized
```


## <a name="create-the-image-version"></a>Görüntü sürümü oluşturma

[Az Image Gallery Create-Image-Version](/cli/azure/sig/image-version#az-sig-image-version-create)kullanarak VM 'den bir görüntü sürümü oluşturun.  

Görüntü sürümü için izin verilen karakterler rakamlardan ve dönemlerdir. Sayılar 32 bitlik bir tamsayı aralığında olmalıdır. Biçim: *MajorVersion*. *MinorVersion*. *Düzeltme Eki*.

Bu örnekte, görüntüimizin sürümü *1.0.0* , *Orta Batı ABD* bölgesinde 2 çoğaltma, *Orta Güney ABD* bölgesinde 1 çoğaltma ve bölgesel olarak yedekli depolama alanı kullanarak *Doğu ABD 2* bölgesindeki 1 çoğaltma oluşturacağız. Çoğaltma bölgeleri, kaynak VM 'nin bulunduğu bölgeyi içermelidir.

Bu örnekteki değerini `--managed-image` , ÖNCEKI adımda sanal makinenizin kimliğiyle değiştirin.

```azurecli-interactive 
az sig image-version create \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --gallery-image-version 1.0.0 \
   --target-regions "westcentralus" "southcentralus=1" "eastus=1=standard_zrs" \
   --replica-count 2 \
   --managed-image "/subscriptions/<Subscription ID>/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM"
```

> [!NOTE]
> Farklı bir görüntü sürümü oluşturmak için aynı yönetilen görüntüyü kullanabilmeniz için görüntü sürümünün oluşturulması ve çoğaltılması tamamen bitmesini beklemeniz gerekir.
>
> Görüntünüzü, görüntü sürümünü oluştururken ekleyerek bir ekleme `--storage-account-type  premium_lrs`veya bölgesel olarak [yedekli depolama alanı](https://docs.microsoft.com/azure/storage/common/storage-redundancy-zrs) `--storage-account-type  standard_zrs` tarafından Premiun depolamada da saklayabilirsiniz.
>

## <a name="next-steps"></a>Sonraki adımlar

Azure CLı kullanarak [Genelleştirilmiş görüntüden](vm-generalized-image-version-cli.md) bir VM oluşturun.
