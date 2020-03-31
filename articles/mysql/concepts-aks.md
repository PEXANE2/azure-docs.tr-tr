---
title: Azure Kubernetes Hizmetine Bağlanın - MySQL için Azure Veritabanı
description: Azure Kubernetes Hizmetini MySQL için Azure Veritabanı'na bağlama hakkında bilgi edinin
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 9e020d34b6cfb8117ccff1114cc938c966126b77
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79537287"
---
# <a name="connecting-azure-kubernetes-service-and-azure-database-for-mysql"></a>MySQL için Azure Kubernetes Hizmetini ve Azure Veritabanını Bağlama

Azure Kubernetes Hizmeti (AKS), Azure'da kullanabileceğiniz yönetilen bir Kubernetes kümesi sağlar. Aşağıda, mysql için AKS ve Azure Veritabanı'nı kullanarak bir uygulama oluştururken göz önünde bulundurulması gereken bazı seçenekler verilmiştir.


## <a name="accelerated-networking"></a>Hızlandırılmış ağ iletişimi
AKS kümenizde hızlandırılmış ağ etkinleştirilmiş temel VM'leri kullanın. VM'de hızlandırılmış ağ etkinleştirildiğinde, VM'de daha düşük gecikme gecikmesi, daha az jitr ve azalmış CPU kullanımı vardır. Hızlandırılmış ağ kullanımının nasıl çalıştığı, desteklenen işletim sistemi sürümleri ve [Linux](../virtual-network/create-vm-accelerated-networking-cli.md)için desteklenen VM örnekleri hakkında daha fazla bilgi edinin.

AKS, Kasım 2018'den itibaren desteklenen VM örneklerinde hızlandırılmış ağ iletişimini destekler. Hızlandırılmış ağ, varsayılan olarak bu VM'leri kullanan yeni AKS kümelerinde etkinleştirilir.

AKS kümenizin ağ iletişimini hızlandırıp hızlandırmadığını doğrulayabilirsiniz:
1. Azure portalına gidin ve AKS kümenizi seçin.
2. Özellikler sekmesini seçin.
3. **Altyapı Kaynak Grubu'nun**adını kopyalayın.
4. Altyapı kaynak grubunu bulmak ve açmak için portal arama çubuğunu kullanın.
5. Bu kaynak grubunda bir VM seçin.
6. VM'nin **Ağ** sekmesine gidin.
7. **Hızlandırılmış ağ'ın** 'Etkin' olup olmadığını doğrulayın.

Veya aşağıdaki iki komutu kullanarak Azure CLI aracılığıyla:
```azurecli
az aks show --resource-group myResourceGroup --name myAKSCluster --query "nodeResourceGroup"
```
Çıktı, AKS'nin ağ arabirimini içeren oluşturduğu oluşturulan kaynak grubu olacaktır. "nodeResourceGroup" adını alın ve bir sonraki komutta kullanın. **EnableAcceleratedNetworking** doğru veya yanlış olacaktır:
```azurecli
az network nic list --resource-group nodeResourceGroup -o table
```

## <a name="open-service-broker-for-azure"></a>Azure için Açık Hizmet Aracısı 
[Azure için Açık Servis Aracısı](https://github.com/Azure/open-service-broker-azure/blob/master/README.md) (OSBA), Azure hizmetlerini doğrudan Kubernetes veya Cloud Foundry'den sağlamanıza olanak tanır. Azure için [bir Açık Hizmet Aracısı API](https://www.openservicebrokerapi.org/) uygulamasıdır.

OSBA ile MySQL sunucusu için bir Azure Veritabanı oluşturabilir ve Kubernetes'in ana dilini kullanarak AKS kümenize bağlayabilirsiniz. OSBA [GitHub sayfasında](https://github.com/Azure/open-service-broker-azure/blob/master/docs/modules/mysql.md)MySQL için OSBA ve Azure Veritabanı'nı birlikte nasıl kullanacağınız hakkında bilgi edinin. 



## <a name="next-steps"></a>Sonraki adımlar
- [Azure Kubernetes Hizmet kümesi oluşturma](../aks/kubernetes-walkthrough.md)
- [MySQL için OSBA ve Azure Veritabanı'nı kullanarak WordPress'i bir Miğfer grafiğinden](../aks/integrate-azure.md) nasıl yükleyin
