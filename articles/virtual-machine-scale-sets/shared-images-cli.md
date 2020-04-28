---
title: Azure 'da ölçek kümesi oluşturmak için paylaşılan VM görüntülerini kullanma
description: Azure 'da sanal makine ölçek kümeleri dağıtmak için kullanılacak paylaşılan VM görüntülerini oluşturmak üzere Azure CLı 'yı nasıl kullanacağınızı öğrenin.
author: axayjo
tags: azure-resource-manager
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: akjosh
ms.reviewer: cynthn
ms.custom: ''
ms.openlocfilehash: 83b3d5c904a65b28482acf8b685c939493c8c03b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76276276"
---
# <a name="create-and-use-shared-images-for-virtual-machine-scale-sets-with-the-azure-cli-20"></a>Azure CLı 2,0 ile sanal makine ölçek kümeleri için paylaşılan görüntüler oluşturma ve kullanma

Ölçek kümesi oluşturduğunuzda, sanal makine örnekleri dağıtılırken kullanılacak bir görüntü belirtirsiniz. [Paylaşılan görüntü galerileri](shared-image-galleries.md) , kuruluşunuz genelinde özel görüntü paylaşımını büyük ölçüde basitleştirir. Özel görüntüler market görüntüleri gibidir, ancak bunları kendiniz oluşturursunuz. Özel görüntüler, uygulamaları, uygulama yapılandırmalarını ve diğer işletim sistemi yapılandırmalarını önceden yükleme gibi yapılandırmaları önyüklemek için kullanılabilir. Paylaşılan görüntü Galerisi, özel VM görüntülerinizi kuruluşunuzdaki diğer kişilerle, bir AAD kiracısı içinde veya bölgeler arasında paylaşmanızı sağlar. Hangi görüntüleri paylaşmak istediğinizi, içinde hangi bölgelerin kullanılabilir olmasını istediğinizi ve bunları ile paylaşmak istediğinizi seçin. Paylaşılan görüntüleri mantıksal olarak gruplandırabilmeniz için birden çok Galeri oluşturabilirsiniz. Galeri, tam rol tabanlı erişim denetimi (RBAC) sağlayan en üst düzey bir kaynaktır. Görüntülerin sürümü oluşturulabilir ve her görüntü sürümünü farklı bir Azure bölgesi kümesine çoğaltmayı tercih edebilirsiniz. Galeri yalnızca yönetilen görüntülerle birlikte kullanılabilir. 

>[!NOTE]
> Bu makale, genelleştirilmiş yönetilen bir görüntü kullanma sürecinde size yol gösterir. Özelleştirilmiş bir sanal makine görüntüsünden ölçek kümesi oluşturulması desteklenmez.


[!INCLUDE [virtual-machines-common-shared-images-cli](../../includes/virtual-machines-common-shared-images-cli.md)]

## <a name="create-a-scale-set-from-the-custom-vm-image"></a>Özel bir sanal makine görüntüsünden ölçek kümesi oluşturma
İle [`az vmss create`](/cli/azure/vmss#az-vmss-create)bir ölçek kümesi oluşturun. *UbuntuLTS* veya *CentOS* gibi bir platform görüntüsü yerine, özel sanal makine görüntünüzün adını belirtin. Aşağıdaki örnek, önceki adımda yer alan *myImage* adlı özel görüntüyü kullanan *myScaleSet* adlı bir ölçek kümesi oluşturur:

```azurecli-interactive
az vmss create \
  -g myGalleryRG \
  -n myScaleSet \
  --image "/subscriptions/<subscription ID>/resourceGroups/myGalleryRG/providers/Microsoft.Compute/galleries/myGallery/images/myImageDefinition/versions/1.0.0" \
  --admin-username azureuser \
  --generate-ssh-keys
```

Tüm ölçek kümesi kaynaklarının ve VM'lerin oluşturulup yapılandırılması birkaç dakika sürer.


[!INCLUDE [virtual-machines-common-gallery-list-cli](../../includes/virtual-machines-common-gallery-list-cli.md)]


## <a name="clean-up-resources"></a>Kaynakları temizleme
Ölçek kümenizi ve ek kaynaklarınızı kaldırmak için [az group delete](/cli/azure/group) komutunu kullanarak kaynak grubunu ve bu kaynak grubunun tüm kaynaklarını silin. `--no-wait` parametresi işlemin tamamlanmasını beklemeden denetimi komut istemine döndürür. `--yes` parametresi kaynakları ek bir komut istemi olmadan silmek istediğinizi onaylar.

```azurecli-interactive
az group delete --name myResourceGroup --no-wait --yes
```


## <a name="next-steps"></a>Sonraki adımlar

Ayrıca, şablonlar kullanarak paylaşılan görüntü Galerisi kaynağı da oluşturabilirsiniz. Çeşitli Azure hızlı başlangıç şablonları mevcuttur: 

- [Paylaşılan görüntü galerisi oluşturma](https://azure.microsoft.com/resources/templates/101-sig-create/)
- [Paylaşılan görüntü galerisinde görüntü tanımı oluşturma](https://azure.microsoft.com/resources/templates/101-sig-image-definition-create/)
- [Paylaşılan görüntü galerisinde görüntü sürümü oluşturma](https://azure.microsoft.com/resources/templates/101-sig-image-version-create/)
- [Görüntü sürümünden VM oluşturma](https://azure.microsoft.com/resources/templates/101-vm-from-sig/)


Herhangi bir sorunla karşılaşırsanız [paylaşılan görüntü galerilerine sorun giderebilirsiniz](troubleshooting-shared-images.md).
