---
title: Azure Kubernetes Hizmetine Bağlanın - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: PostgreSQL için Azure Kubernetes Hizmetini (AKS) Azure Veritabanı'na bağlama hakkında bilgi edinin - Tek Sunucu
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.date: 5/6/2019
ms.topic: conceptual
ms.openlocfilehash: 46aa411826dd3ea578a2d98b0fe631ab0a12ef4a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74769889"
---
# <a name="connecting-azure-kubernetes-service-and-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure Kubernetes Hizmetini ve Azure Veritabanını Bağlama - Tek Sunucu

Azure Kubernetes Hizmeti (AKS), Azure'da kullanabileceğiniz yönetilen bir Kubernetes kümesi sağlar. Aşağıda, bir uygulama oluşturmak için PostgreSQL için AKS ve Azure Veritabanı'nı birlikte kullanırken göz önünde bulundurulması gereken bazı seçenekler verilmiştir.


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

OSBA ile PostgreSQL sunucusu için bir Azure Veritabanı oluşturabilir ve Kubernetes'in ana dilini kullanarak AKS kümenize bağlayabilirsiniz. PostgreSQL için OSBA ve Azure Veritabanı'nı [OSBA GitHub sayfasında](https://github.com/Azure/open-service-broker-azure/blob/master/docs/modules/postgresql.md)birlikte nasıl kullanacağınız hakkında bilgi edinin. 


## <a name="connection-pooling"></a>Bağlantı havuzu
Bağlantı havuzlayıcı, veritabanına yeni bağlantılar oluşturma ve kapatma yla ilişkili maliyeti ve zamanı en aza indirir. Havuz, yeniden kullanılabilen bir bağlantı koleksiyonudur. 

PostgreSQL ile kullanabileceğiniz birden çok bağlantı havuzu vardır. Bunlardan biri [PgBouncer](https://pgbouncer.github.io/)olduğunu. Microsoft Konteyner Kayıt Defteri'nde, AKS'den PostgreSQL için Azure Veritabanı'na bağlantıları bir araya getirmek için bir kenar otomobilinde kullanılabilen hafif bir kapsayıcı pgbouncer saılmaktadır. Bu resme nasıl erişip kullanacağınızı öğrenmek için [docker hub sayfasını](https://hub.docker.com/r/microsoft/azureossdb-tools-pgbouncer/) ziyaret edin. 


## <a name="next-steps"></a>Sonraki adımlar
-  [Azure Kubernetes Hizmet kümesi oluşturma](../aks/kubernetes-walkthrough.md)
