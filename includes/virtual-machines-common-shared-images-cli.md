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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "66814785"
---
## <a name="before-you-begin"></a>Başlamadan önce

Bu makaledeki örneği tamamlamak için, genelleştirilmiş bir VM'nin varolan yönetilen bir görüntüsüne sahip olmalısınız. Daha fazla bilgi için [Bkz. Öğretici: Azure CLI ile azure VM'nin özel bir görüntüsünü oluşturun.](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-custom-images) Yönetilen görüntü bir veri diski içeriyorsa, veri diskboyutu 1 TB'den fazla olamaz.

## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell'i başlatma

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. 

Cloud Shell'i açmak için kod bloğunun sağ üst köşesinden **Deneyin**'i seçmeniz yeterlidir. Ayrıca bulut shell'i ayrı bir tarayıcı [https://shell.azure.com/bash](https://shell.azure.com/bash)sekmesinde başlatabilirsiniz. **Kopyala**’yı seçerek kod bloğunu kopyalayın, Cloud Shell’e yapıştırın ve Enter tuşuna basarak çalıştırın.

CLI'yi yerel olarak yüklemeyi ve kullanmayı tercih ediyorsanız, [Azure CLI'yi yükle'ye](/cli/azure/install-azure-cli)bakın.

## <a name="create-an-image-gallery"></a>Resim galerisi oluşturma 

Resim galerisi, görüntü paylaşımını etkinleştirmek için kullanılan birincil kaynaktır. Galeri adı için izin verilen karakterler büyük veya küçük harfler, basamaklar, noktalar ve dönemlerdir. Galeri adı tire içeremez.   Galeri adları aboneliğinizde benzersiz olmalıdır. 

az sig create kullanarak bir resim galerisi [oluşturun.](/cli/azure/sig#az-sig-create) Aşağıdaki örnek, *myGalleryRG'de* *myGallery* adlı bir galeri oluşturur.

```azurecli-interactive
az group create --name myGalleryRG --location WestCentralUS
az sig create --resource-group myGalleryRG --gallery-name myGallery
```

## <a name="create-an-image-definition"></a>Görüntü tanımı oluşturma

Görüntü tanımları görüntüler için mantıksal bir gruplandırma oluşturur. Bunlar, içlerinde oluşturulan görüntü sürümleri hakkındaki bilgileri yönetmek için kullanılır. Resim tanım adları büyük veya küçük harflerden, basamaklardan, noktalardan, tirelerden ve dönemlerden oluşur. Görüntü tanımı için belirtebileceğiniz değerler hakkında daha fazla bilgi için [Resim tanımlarına](https://docs.microsoft.com/azure/virtual-machines/linux/shared-image-galleries#image-definitions)bakın.

Az sig resim tanımı oluşturma 'nı kullanarak galeride ilk resim [tanımı oluşturun.](/cli/azure/sig/image-definition#az-sig-image-definition-create)

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


## <a name="create-an-image-version"></a>Resim sürümü oluşturma 

[Az resim galerisi create-image-version](/cli/azure/sig/image-version#az-sig-image-version-create)kullanarak gerektiği gibi görüntünün sürümlerini oluşturun. Görüntü sürümünü oluşturmak için taban çizgisi olarak kullanmak için yönetilen görüntünün kimliğini geçirmeniz gerekir. Kaynak grubundaki resimler hakkında bilgi almak için [az resim listesini](/cli/azure/image?view#az-image-list) kullanabilirsiniz. 

Görüntü sürümü için izin verilen karakterler sayılar ve dönemlerdir. Sayılar 32 bitlik bir tamsayı aralığında olmalıdır. Biçim: *MajorVersion*. *MinorVersion*. *Yama*.

Bu örnekte, görüntümüzün sürümü *1.0.0* ve biz *Batı Orta ABD* bölgesinde 2 çoğaltma oluşturmak için gidiyoruz, Güney Orta *ABD* bölgesinde 1 çoğaltma ve bölge yedekli depolama kullanarak Doğu *ABD 2* bölgesinde 1 çoğaltma.


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
> Başka bir görüntü sürümü oluşturmak için aynı yönetilen görüntüyü kullanabilmeniz için önce görüntü sürümünün tamamen oluşturulmasını ve çoğaltılmasını beklemeniz gerekir.
>
> Ayrıca, görüntü sürümünü oluştururken ekleyerek `--storage-account-type standard_zrs` tüm görüntü sürüm yinelemelerinizi Bölge [Yedekdeposunda](https://docs.microsoft.com/azure/storage/common/storage-redundancy-zrs) da depolayabilirsiniz.
>

## <a name="share-the-gallery"></a>Galeriyi paylaşın

Galeri düzeyindeki diğer kullanıcılarla paylaşmanızı öneririz. Galerinizin nesne kimliğini almak için [az sig show'u](/cli/azure/sig#az-sig-show)kullanın.

```azurecli-interactive
az sig show \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --query id
```

Kullanıcıya paylaşılan resim galerisine erişim sağlamak için nesne kimliğini, bir e-posta adresi ve [az rol ataması oluşturmanın](/cli/azure/role/assignment#az-role-assignment-create) yanı sıra kapsam olarak kullanın.

```azurecli-interactive
az role assignment create --role "Reader" --assignee <email address> --scope <gallery ID>
```


