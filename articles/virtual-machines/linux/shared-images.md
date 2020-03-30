---
title: Azure CLI ile paylaşılan resim galerileri oluşturma
description: Bu makalede, Azure'da paylaşılan bir VM resmi oluşturmak için Azure CLI'yi nasıl kullanacağınızı öğrenirsiniz.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: axayjo
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/06/2019
ms.author: akjosh
ms.reviewer: cynthn
ms.custom: ''
ms.openlocfilehash: de1afa2367afcb78e8ca68e518acc93e33f61c43
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74034958"
---
# <a name="create-a-shared-image-gallery-with-the-azure-cli"></a>Azure CLI ile paylaşılan bir resim galerisi oluşturma

[Paylaşılan Resim Galerisi,](shared-image-galleries.md) kuruluşunuz genelinde özel görüntü paylaşımını kolaylaştırır. Özel görüntüler market görüntüleri gibidir, ancak bunları kendiniz oluşturursunuz. Özel görüntüler, uygulamaları, uygulama yapılandırmalarını ve diğer işletim sistemi yapılandırmalarını önceden yükleme gibi yapılandırmaları önyüklemek için kullanılabilir. 

Paylaşılan Resim Galerisi, özel VM görüntülerinizi kuruluşunuzdaki, bölgeler içinde veya diğer bölgeler arasında, bir AAD kiracısı içinde başkalarıyla paylaşmanıza olanak tanır. Hangi resimleri paylaşmak istediğinizi, bunları hangi bölgelerde kullanılabilir hale getirmek istediğinizi ve bunları kiminle paylaşmak istediğinizi seçin. Paylaşılan görüntüleri mantıksal olarak gruplandırmak için birden çok galeri oluşturabilirsiniz. 

Galeri, tam rol tabanlı erişim denetimi (RBAC) sağlayan üst düzey bir kaynaktır. Görüntüler sürülebilir ve her resim sürümünü farklı bir Azure bölgesi kümesinde çoğaltmayı seçebilirsiniz. Galeri yalnızca Yönetilen Görüntüler ile çalışır.

Paylaşılan Resim Galerisi özelliğinin birden çok kaynak türü vardır. Bu makalede bunları kullanacak veya inşa edeceğiz:

| Kaynak | Açıklama|
|----------|------------|
| **Yönetilen görüntü** | Bu, tek başına kullanılabilen veya resim galerisinde **görüntü sürümü** oluşturmak için kullanılabilen temel bir görüntüdür. Yönetilen görüntüler genelleştirilmiş VM'lerden oluşturulur. Yönetilen görüntü, birden çok VM yapmak için kullanılabilen ve şimdi paylaşılan görüntü sürümleri oluşturmak için kullanılabilen özel bir VHD türüdür. |
| **Resim galerisi** | Azure Marketi gibi, **resim galerisi** de görüntüleri yönetmek ve paylaşmak için bir depodur, ancak kimin erişimi olduğunu siz denetlersiniz. |
| **Resim tanımı** | Görüntüler bir galeri içinde tanımlanır ve görüntü ve dahili olarak kullanmak için gereksinimleri hakkında bilgi taşır. Bu, görüntünün Windows veya Linux olup olmadığını, sürüm notlarını ve minimum ve maksimum bellek gereksinimlerini içerir. Bu bir görüntü türünün tanımıdır. |
| **Görüntü sürümü** | **Resim sürümü,** galeri kullanırken VM oluşturmak için kullandığınız sürümdür. Ortamınız için gerektiği gibi görüntünün birden çok sürümüne sahip olabilirsiniz. Yönetilen bir görüntü gibi, VM oluşturmak için bir **görüntü sürümü** kullandığınızda, görüntü sürümü VM için yeni diskler oluşturmak için kullanılır. Görüntü sürümleri birden çok kez kullanılabilir. |



[!INCLUDE [virtual-machines-common-shared-images-cli](../../../includes/virtual-machines-common-shared-images-cli.md)]

## <a name="create-a-vm"></a>VM oluşturma

Az vm oluştur kullanarak en son görüntü sürümünden bir [VM oluşturun.](/cli/azure/vm#az-vm-create)

```azurecli-interactive 
az vm create\
   --resource-group myGalleryRG \
   --name myVM \
   --image "/subscriptions/subscription ID where the gallery is located>/resourceGroups/myGalleryRG/providers/Microsoft.Compute/galleries/myGallery/images/myImageDefinition" \
   --generate-ssh-keys
```

`--image` Parametre için resim sürüm kimliğini kullanarak belirli bir sürümü de kullanabilirsiniz. Örneğin, resim sürümü *1.0.0* türünü `--image "/subscriptions/<subscription ID where the gallery is located>/resourceGroups/myGalleryRG/providers/Microsoft.Compute/galleries/myGallery/images/myImageDefinition/versions/1.0.0"`kullanmak için: .

[!INCLUDE [virtual-machines-common-gallery-list-cli](../../../includes/virtual-machines-common-gallery-list-cli.md)]

[!INCLUDE [virtual-machines-common-shared-images-update-delete-cli](../../../includes/virtual-machines-common-shared-images-update-delete-cli.md)]

## <a name="next-steps"></a>Sonraki adımlar
[Azure Image Builder (önizleme)](image-builder-overview.md) görüntü sürümü oluşturma otomatikleştirmek yardımcı olabilir, hatta güncelleme kullanabilirsiniz ve [varolan bir resim sürümünden yeni bir resim sürümü oluşturmak](image-builder-gallery-update-image-version.md). 

Şablonları kullanarak Paylaşılan Resim Galerisi kaynaklarını da oluşturabilirsiniz. Kullanılabilir birkaç Azure Quickstart Şablonu vardır: 

- [Paylaşılan Resim Galerisi Oluşturma](https://azure.microsoft.com/resources/templates/101-sig-create/)
- [Paylaşılan Resim Galerisinde Resim Tanımı Oluşturma](https://azure.microsoft.com/resources/templates/101-sig-image-definition-create/)
- [Paylaşılan Resim Galerisinde Resim Sürümü Oluşturma](https://azure.microsoft.com/resources/templates/101-sig-image-version-create/)
- [Resim Sürümünden VM Oluşturma](https://azure.microsoft.com/resources/templates/101-vm-from-sig/)

Paylaşılan Resim Galerileri hakkında daha fazla bilgi için [Genel Bakış'a](shared-image-galleries.md)bakın. Sorunlarla karşılaştıysanız, [paylaşılan görüntü galerilerini sorun giderme](troubleshooting-shared-images.md)konusuna bakın.
