---
title: Azure CLı kullanarak özelleştirilmiş bir görüntü sürümünden VM oluşturma
description: Azure CLı kullanarak paylaşılan görüntü galerisinde özelleştirilmiş görüntü sürümünü kullanarak bir VM oluşturun.
author: cynthn
ms.service: virtual-machines
ms.subservice: imaging
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 04/23/2020
ms.author: cynthn
ms.reviewer: akjosh
ms.openlocfilehash: 1ccf03deee2a2f72c1eb2008e1acc5bf67d16447
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/05/2020
ms.locfileid: "82796778"
---
# <a name="create-a-vm-using-a-specialized-image-version-with-the-azure-cli"></a>Azure CLı ile özel görüntü sürümünü kullanarak VM oluşturma

Paylaşılan görüntü galerisinde depolanan [özelleştirilmiş görüntü sürümünden](https://docs.microsoft.com/azure/virtual-machines/linux/shared-image-galleries#generalized-and-specialized-images) bir VM oluşturun. Genelleştirilmiş görüntü sürümü kullanarak bir VM oluşturmak istiyorsanız, bkz. [Genelleştirilmiş görüntü sürümünden VM oluşturma](vm-generalized-image-version-cli.md).

Kaynak adlarını bu örnekte gereken şekilde değiştirin. 

Tanımların adını ve KIMLIĞINI görmek için [az Sig Image-Definition List](/cli/azure/sig/image-definition#az-sig-image-definition-list) öğesini kullanarak bir galerideki görüntü tanımlarını listeleyin.

```azurecli-interactive 
resourceGroup=myGalleryRG
gallery=myGallery
az sig image-definition list \
   --resource-group $resourceGroup \
   --gallery-name $gallery \
   --query "[].[name, id]" \
   --output tsv
```

Görüntünün özelleşmiş bir görüntü olduğunu göstermek için,--özelleşmiş parametresini kullanarak [az VM Create](/cli/azure/vm#az-vm-create) kullanarak VM oluşturun. 

Kullanılabilir görüntünün en son sürümünden VM `--image` oluşturmak için görüntü tanımı kimliği ' ni kullanın. Ayrıca, için `--image`görüntü sürüm kimliği sağlayarak VM 'yi belirli bir sürümden da oluşturabilirsiniz. 

Bu örnekte, *Myımagedefinition* görüntüsünün en son sürümünden bir VM oluşturacağız.

```azurecli
az group create --name myResourceGroup --location eastus
az vm create --resource-group myResourceGroup \
    --name myVM \
    --image "/subscriptions/<Subscription ID>/resourceGroups/myGalleryRG/providers/Microsoft.Compute/galleries/myGallery/images/myImageDefinition" \
    --specialized
```


## <a name="next-steps"></a>Sonraki adımlar
[Azure görüntü Oluşturucu (Önizleme)](./linux/image-builder-overview.md) , görüntü sürümü oluşturmayı otomatikleştirmenize yardımcı olabilir, hatta [mevcut bir görüntü sürümünden yeni bir görüntü sürümünü](./linux/image-builder-gallery-update-image-version.md)güncelleştirmek ve oluşturmak için de kullanabilirsiniz. 

Ayrıca, şablonlar kullanarak paylaşılan görüntü Galerisi kaynağı da oluşturabilirsiniz. Çeşitli Azure hızlı başlangıç şablonları mevcuttur: 

- [Paylaşılan görüntü galerisi oluşturma](https://azure.microsoft.com/resources/templates/101-sig-create/)
- [Paylaşılan görüntü galerisinde görüntü tanımı oluşturma](https://azure.microsoft.com/resources/templates/101-sig-image-definition-create/)
- [Paylaşılan görüntü galerisinde görüntü sürümü oluşturma](https://azure.microsoft.com/resources/templates/101-sig-image-version-create/)
- [Görüntü sürümünden VM oluşturma](https://azure.microsoft.com/resources/templates/101-vm-from-sig/)


