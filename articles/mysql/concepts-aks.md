---
title: Azure Kubernetes hizmetine bağlanma-MySQL için Azure veritabanı
description: Azure Kubernetes hizmetini MySQL için Azure veritabanı ile bağlama hakkında bilgi edinin
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 07/14/2020
ms.openlocfilehash: 712bf702ba355ec3b2ca6184c33da8489f8a178e
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86519873"
---
# <a name="connecting-azure-kubernetes-service-and-azure-database-for-mysql"></a>Azure Kubernetes hizmetini ve MySQL için Azure veritabanı 'nı bağlama

Azure Kubernetes hizmeti (AKS), Azure 'da kullanabileceğiniz yönetilen bir Kubernetes kümesi sağlar. Aşağıda, bir uygulama oluşturmak için AKS ve MySQL için Azure veritabanı kullanılırken dikkate alınması gereken bazı seçenekler verilmiştir.


## <a name="accelerated-networking"></a>Hızlandırılmış ağ iletişimi
AKS kümenizde hızlandırılmış ağ özellikli, temel alınan VM 'Leri kullanın. Bir VM 'de hızlandırılmış ağ etkinleştirildiğinde, VM 'de daha düşük gecikme süresi, azaltılmış değişim ve CPU kullanımı azalır. Hızlandırılmış ağ oluşturma, desteklenen işletim sistemi sürümleri ve [Linux](../virtual-network/create-vm-accelerated-networking-cli.md)IÇIN desteklenen VM örnekleri hakkında daha fazla bilgi edinin.

AKS, Kasım 2018 ' den bu desteklenen sanal makine örneklerinde hızlandırılmış ağı destekler. Hızlandırılmış ağ, bu VM 'Leri kullanan yeni AKS kümelerinde varsayılan olarak etkindir.

AKS kümenizin hızlandırılmış ağa sahip olup olmadığını doğrulayabilirsiniz:
1. Azure portal gidin ve AKS kümenizi seçin.
2. Özellikler sekmesini seçin.
3. **Altyapı kaynak grubunun**adını kopyalayın.
4. Altyapı kaynak grubunu bulmak ve açmak için Portal arama çubuğunu kullanın.
5. Bu kaynak grubunda bir VM seçin.
6. VM 'nin **ağ** sekmesine gidin.
7. **Hızlandırılmış ağın** ' etkin ' olup olmadığını onaylayın.

Ya da aşağıdaki iki komutu kullanarak Azure CLı aracılığıyla:
```azurecli
az aks show --resource-group myResourceGroup --name myAKSCluster --query "nodeResourceGroup"
```
Çıktı, ağ arabirimini içeren AKS oluşturduğu kaynak grubu olacaktır. "NodeResourceGroup" adını alıp sonraki komutta kullanın. **Enableivme ağı** doğru veya yanlış olacaktır:
```azurecli
az network nic list --resource-group nodeResourceGroup -o table
```


## <a name="next-steps"></a>Sonraki adımlar
- [Azure Kubernetes Service kümesi oluşturma](../aks/kubernetes-walkthrough.md)
- [OSBA ve MySQL Için Azure veritabanı 'nı kullanarak bir hele grafiğinden WordPress yüklemeyi](../aks/integrate-azure.md) öğrenin
