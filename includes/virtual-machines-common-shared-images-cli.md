---
title: include dosyası
description: include dosyası
services: virtual-machines
author: axayjo
ms.service: virtual-machines
ms.topic: include
ms.date: 05/21/2019
ms.author: akjosh; cynthn
ms.custom: include file
ms.openlocfilehash: 57736a3cd553e83294d5290867e261b626cb035f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "66814785"
---
## <a name="before-you-begin"></a>Başlamadan önce

Bu makaledeki örneği tamamlayabilmeniz için, genelleştirilmiş bir VM 'nin mevcut bir yönetilen görüntüsüne sahip olmanız gerekir. Daha fazla bilgi için bkz. [öğretici: Azure CLI Ile Azure VM 'nin özel bir görüntüsünü oluşturma](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-custom-images). Yönetilen görüntü bir veri diski içeriyorsa, veri diski boyutu 1 TB 'den fazla olamaz.

## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell'i başlatma

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. 

Cloud Shell'i açmak için kod bloğunun sağ üst köşesinden **Deneyin**'i seçmeniz yeterlidir. Ayrıca, ' a giderek ayrı bir tarayıcı sekmesinde Cloud Shell de başlatabilirsiniz [https://shell.azure.com/bash](https://shell.azure.com/bash). **Kopyala**’yı seçerek kod bloğunu kopyalayın, Cloud Shell’e yapıştırın ve Enter tuşuna basarak çalıştırın.

CLı 'yi yerel olarak yükleyip kullanmayı tercih ediyorsanız bkz. [Azure CLI 'Yı yüklemek](/cli/azure/install-azure-cli).

## <a name="create-an-image-gallery"></a>Görüntü galerisi oluşturma 

Görüntü Galerisi, görüntü paylaşımını etkinleştirmek için kullanılan birincil kaynaktır. Galeri adı için izin verilen karakterler büyük veya küçük harflerden, rakamlardan, noktalardan ve noktalardan oluşur. Galeri adı tire içeremez.   Galeri adları, aboneliğiniz dahilinde benzersiz olmalıdır. 

[Az Sig Create](/cli/azure/sig#az-sig-create)kullanarak bir görüntü galerisi oluşturun. Aşağıdaki örnek *Mygallerrg*Içinde *MyGallery* adlı bir galeri oluşturur.

```azurecli-interactive
az group create --name myGalleryRG --location WestCentralUS
az sig create --resource-group myGalleryRG --gallery-name myGallery
```

## <a name="create-an-image-definition"></a>Görüntü tanımı oluşturma

Görüntü tanımları görüntüler için bir mantıksal gruplama oluşturur. Bunlar içinde oluşturulan görüntü sürümleri hakkındaki bilgileri yönetmek için kullanılır. Görüntü tanımı adları büyük veya küçük harflerden, rakamlardan, noktalardan, çizgilerden ve noktalardan oluşabilir. Bir görüntü tanımı için belirtebileceğiniz değerler hakkında daha fazla bilgi için bkz. [görüntü tanımları](https://docs.microsoft.com/azure/virtual-machines/linux/shared-image-galleries#image-definitions).

Galeride [az Sig Image-Definition Create](/cli/azure/sig/image-definition#az-sig-image-definition-create)kullanarak bir başlangıç görüntüsü tanımı oluşturun.

```azurecli-interactive 
az sig image-definition create \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --publisher myPublisher \
   --offer myOffer \
   --sku 16.04-LTS \
   --os-type Linux 
```


## <a name="create-an-image-version"></a>Görüntü sürümü oluşturma 

[Az Image Gallery Create-Image-Version](/cli/azure/sig/image-version#az-sig-image-version-create)kullanarak gereken görüntü sürümlerini oluşturun. Görüntü sürümü oluşturmak için taban çizgisi olarak kullanılacak yönetilen görüntünün KIMLIĞINI geçirmeniz gerekir. Kaynak grubundaki görüntüler hakkında bilgi almak için [az Image List](/cli/azure/image?view#az-image-list) kullanabilirsiniz. 

Görüntü sürümü için izin verilen karakterler rakamlardan ve dönemlerdir. Sayılar 32 bitlik bir tamsayı aralığında olmalıdır. Biçim: *MajorVersion*. *MinorVersion*. *Düzeltme Eki*.

Bu örnekte, görüntüimizin sürümü *1.0.0* , *Orta Batı ABD* bölgesinde 2 çoğaltma, *Orta Güney ABD* bölgesinde 1 çoğaltma ve bölgesel olarak yedekli depolama alanı kullanarak *Doğu ABD 2* bölgesindeki 1 çoğaltma oluşturacağız.


```azurecli-interactive 
az sig image-version create \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --gallery-image-version 1.0.0 \
   --target-regions "WestCentralUS" "SouthCentralUS=1" "EastUS2=1=Standard_ZRS" \
   --replica-count 2 \
   --managed-image "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/images/myImage"
```

> [!NOTE]
> Farklı bir görüntü sürümü oluşturmak için aynı yönetilen görüntüyü kullanabilmeniz için görüntü sürümünün oluşturulması ve çoğaltılması tamamen bitmesini beklemeniz gerekir.
>
> Görüntü sürümünü oluştururken ekleyerek `--storage-account-type standard_zrs` görüntü sürümü çoğaltmalarınızı, bölgesel olarak [yedekli depolama](https://docs.microsoft.com/azure/storage/common/storage-redundancy-zrs) alanında da saklayabilirsiniz.
>

## <a name="share-the-gallery"></a>Galeriyi paylaşma

Galeri düzeyindeki diğer kullanıcılarla paylaşmanızı öneririz. Galerinizin nesne KIMLIĞINI almak için [az SIG Show](/cli/azure/sig#az-sig-show)' ı kullanın.

```azurecli-interactive
az sig show \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --query id
```

Nesne KIMLIĞINI bir e-posta adresi ile birlikte kullanın ve bir kullanıcıya paylaşılan görüntü galerisine erişim sağlamak için [az role atama oluştur](/cli/azure/role/assignment#az-role-assignment-create) ' u kullanın.

```azurecli-interactive
az role assignment create --role "Reader" --assignee <email address> --scope <gallery ID>
```


