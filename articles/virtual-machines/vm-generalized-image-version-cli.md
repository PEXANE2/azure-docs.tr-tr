---
title: Azure CLı kullanarak Genelleştirilmiş görüntüden VM oluşturma
description: Azure CLı kullanarak Genelleştirilmiş görüntü sürümünden bir VM oluşturun.
author: cynthn
ms.service: virtual-machines
ms.subservice: imaging
ms.topic: how-to
ms.workload: infrastructure
ms.date: 05/04/2020
ms.author: cynthn
ms.openlocfilehash: 5e59872a4da0136232652008a2980601428eeab6
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/05/2020
ms.locfileid: "82796791"
---
# <a name="create-a-vm-from-a-generalized-image-version-using-the-cli"></a>CLı kullanarak Genelleştirilmiş görüntü sürümünden VM oluşturma

Paylaşılan görüntü galerisinde depolanan [Genelleştirilmiş görüntü sürümünden](https://docs.microsoft.com/azure/virtual-machines/linux/shared-image-galleries#generalized-and-specialized-images) bir VM oluşturun. Özel bir görüntü kullanarak bir VM oluşturmak istiyorsanız, bkz. [özel bir GÖRÜNTÜDEN VM oluşturma](vm-specialized-image-version-powershell.md). 


## <a name="get-the-image-id"></a>Görüntü KIMLIĞINI al

Tanımların adını ve KIMLIĞINI görmek için [az Sig Image-Definition List](/cli/azure/sig/image-definition#az-sig-image-definition-list) öğesini kullanarak bir galerideki görüntü tanımlarını listeleyin.

```azurecli-interactive 
resourceGroup=myGalleryRG
gallery=myGallery
az sig image-definition list --resource-group $resourceGroup --gallery-name $gallery --query "[].[name, id]" --output tsv
```

## <a name="create-the-vm"></a>Sanal makine oluşturma

[az vm create](/cli/azure/vm#az-vm-create) kullanarak VM oluşturun. Görüntünün en son sürümünü kullanmak için, görüntü tanımının KIMLIĞINE `--image` ayarlayın. 

Kaynak adlarını bu örnekte gereken şekilde değiştirin. 

```azurecli-interactive 
imgDef="/subscriptions/<subscription ID where the gallery is located>/resourceGroups/myGalleryRG/providers/Microsoft.Compute/galleries/myGallery/images/myImageDefinition"
vmResourceGroup=myResourceGroup
location=eastus
vmName=myVM
adminUsername=azureuser


az group create --name $vmResourceGroup --location $location

az vm create\
   --resource-group $vmResourceGroup \
   --name $vmName \
   --image $imgDef \
   --admin-username $adminUsername \
   --generate-ssh-keys
```

Ayrıca, `--image` parametresi için görüntü sürümü kimliğini kullanarak belirli bir sürümü kullanabilirsiniz. Örneğin, görüntü sürümü *1.0.0* türünü kullanmak için: `--image "/subscriptions/<subscription ID where the gallery is located>/resourceGroups/myGalleryRG/providers/Microsoft.Compute/galleries/myGallery/images/myImageDefinition/versions/1.0.0"`.

## <a name="next-steps"></a>Sonraki adımlar

[Azure görüntü Oluşturucu (Önizleme)](./linux/image-builder-overview.md) , görüntü sürümü oluşturmayı otomatikleştirmenize yardımcı olabilir, hatta [mevcut bir görüntü sürümünden yeni bir görüntü sürümünü](./linux/image-builder-gallery-update-image-version.md)güncelleştirmek ve oluşturmak için de kullanabilirsiniz. 
