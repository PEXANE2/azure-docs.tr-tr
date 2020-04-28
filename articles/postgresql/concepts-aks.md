---
title: Azure Kubernetes hizmetine bağlanma-PostgreSQL için Azure veritabanı-tek sunucu
description: Azure Kubernetes hizmeti 'ni (AKS) PostgreSQL için Azure veritabanı-tek sunucu ile bağlama hakkında bilgi edinin
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.date: 5/6/2019
ms.topic: conceptual
ms.openlocfilehash: 46aa411826dd3ea578a2d98b0fe631ab0a12ef4a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "74769889"
---
# <a name="connecting-azure-kubernetes-service-and-azure-database-for-postgresql---single-server"></a>Azure Kubernetes hizmetini ve PostgreSQL için Azure veritabanı 'nı bağlama-tek sunucu

Azure Kubernetes hizmeti (AKS), Azure 'da kullanabileceğiniz yönetilen bir Kubernetes kümesi sağlar. Aşağıda, bir uygulama oluşturmak için AKS ve PostgreSQL için Azure veritabanı kullanılırken dikkate alınması gereken bazı seçenekler verilmiştir.


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

OSBA ile, PostgreSQL için Azure veritabanı sunucusu oluşturabilir ve Kubernetes 'in yerel dilini kullanarak AKS kümenize bağlayabilirsiniz. Osba ve PostgreSQL için Azure veritabanı 'nı [osba GitHub sayfasında](https://github.com/Azure/open-service-broker-azure/blob/master/docs/modules/postgresql.md)birlikte kullanma hakkında bilgi edinin. 


## <a name="connection-pooling"></a>Bağlantı havuzu
Bir bağlantı havuzlayıcı, veritabanına yeni bağlantılar oluşturma ve kapatma ile ilişkili maliyet ve süreyi en aza indirir. Havuz yeniden kullanılabilen bir bağlantı koleksiyonudur. 

PostgreSQL ile kullanabileceğiniz birden çok bağlantı havuzleyiciler vardır. Bunlardan biri [Pgbouncer](https://pgbouncer.github.io/). Microsoft Container Registry, AKS 'ten PostgreSQL için Azure veritabanı arasındaki bağlantıları havuza almak üzere bir sepet içinde kullanılabilen hafif bir Kapsayıcılı PgBouncer sunuyoruz. Bu resme erişme ve bu görüntüyü kullanma hakkında bilgi edinmek için [Docker Hub sayfasını](https://hub.docker.com/r/microsoft/azureossdb-tools-pgbouncer/) ziyaret edin. 


## <a name="next-steps"></a>Sonraki adımlar
-  [Azure Kubernetes hizmet kümesi oluşturma](../aks/kubernetes-walkthrough.md)
