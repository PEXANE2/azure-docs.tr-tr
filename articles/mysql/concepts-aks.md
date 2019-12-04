---
title: Azure Kubernetes hizmetine bağlanma-MySQL için Azure veritabanı
description: Azure Kubernetes hizmetini MySQL için Azure veritabanı ile bağlama hakkında bilgi edinin
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: 71b266231b7ed3012e5ea7f65fe9487eeb5fb358
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74773620"
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

## <a name="open-service-broker-for-azure"></a>Azure için Açık Hizmet Aracısı 
[Azure Için açık hizmet Aracısı](https://github.com/Azure/open-service-broker-azure/blob/master/README.md) (osba), Azure hizmetlerini doğrudan Kubernetes veya Cloud Foundry üzerinden sağlamanıza olanak tanır. Azure için [açık bir HIZMET ARACıSı API](https://www.openservicebrokerapi.org/) uygulamasıdır.

OSBA ile, MySQL için Azure veritabanı sunucusu oluşturabilir ve Kubernetes 'in yerel dilini kullanarak AKS kümenize bağlayabilirsiniz. Osba [GitHub sayfasında](https://github.com/Azure/open-service-broker-azure/blob/master/docs/modules/mysql.md), MySQL Için Azure veritabanı 'nı kullanma hakkında bilgi edinin. 



## <a name="next-steps"></a>Sonraki adımlar
- [Azure Kubernetes hizmet kümesi oluşturma](../aks/kubernetes-walkthrough.md)
- [OSBA ve MySQL Için Azure veritabanı 'nı kullanarak bir hele grafiğinden WordPress yüklemeyi](../aks/integrate-azure.md) öğrenin
