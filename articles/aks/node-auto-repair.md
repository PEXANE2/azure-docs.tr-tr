---
title: Azure Kubernetes hizmeti (AKS) düğümlerini otomatik olarak onarma
description: Düğüm otomatik onarma işlevselliği ve AKS 'in kopuk çalışan düğümlerini düzeltme hakkında bilgi edinin.
services: container-service
ms.topic: conceptual
ms.date: 08/24/2020
ms.openlocfilehash: 781a1ffebb40b0cce9f18699d308db90633e8626
ms.sourcegitcommit: de2750163a601aae0c28506ba32be067e0068c0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89490114"
---
# <a name="azure-kubernetes-service-aks-node-auto-repair"></a>Azure Kubernetes hizmeti (AKS) düğümü otomatik onarma

AKS, çalışan düğümlerinin sistem durumunu sürekli olarak denetler ve sağlıksız hale gelirse düğümlerin otomatik olarak onarımını gerçekleştirir. Bu belgede, otomatik düğüm onarma işlevinin hem Windows hem de Linux düğümlerinde nasıl davrandığı hakkında operatörler bildirilir. Azure VM platformu, AKS onarımına ek olarak, sorunlar yaşayan [sanal makinelerde de bakım gerçekleştirir][vm-updates] . AKS ve Azure VM 'Leri, kümelerdeki hizmet kesintilerini en aza indirmek için birlikte çalışır.

## <a name="how-aks-checks-for-unhealthy-nodes"></a>AKS, sağlıksız düğümleri denetler

AKS, bir düğümün sağlıksız olup olmadığını ve onarımı gerekip gerekmediğini belirleme kurallarını kullanır. AKS, otomatik onarımın gerekip gerekmediğini anlamak için aşağıdaki kuralları kullanır.

* Düğüm, 10 dakikalık bir zaman dilimi içinde ardışık denetimlerin durumunu **NotReady** olarak bildiriyor
* Düğüm, 10 dakika içinde bir durum raporlamaz

Kubectl ile düğümlerinizin sistem durumunu el ile kontrol edebilirsiniz.

```
kubectl get nodes
```

## <a name="how-automatic-repair-works"></a>Otomatik onarımın nasıl çalıştığı

> [!Note]
> AKS, Kullanıcı hesabı **aks-düzeltici**ile onarım işlemlerini başlatır.

Bir düğüm yukarıdaki kurallara göre uygun değilse ve 10 ardışık dakika boyunca sağlıksız kalırsa, aşağıdaki eylemler alınır.

1. Düğümü yeniden başlatın
1. Yeniden başlatma başarısız olursa, düğümü yeniden görüntü oluştur
1. Yeniden görüntü başarısız olursa yeni bir düğüm oluşturup yeniden görüntü oluşturun

Eylemlerden hiçbiri başarılı olmazsa, ek düzeltmeler AKS mühendislerine göre araştırılır. Bir sistem durumu denetimi sırasında birden çok düğüm sağlıksız ise, başka bir onarım başlamadan önce her düğüm tek tek onarılır.

## <a name="next-steps"></a>Sonraki adımlar

AKS kümesi iş yüklerinizde yüksek kullanılabilirliği artırmak için [kullanılabilirlik alanları][availability-zones] kullanın.

<!-- LINKS - External -->

<!-- LINKS - Internal -->
[availability-zones]: ./availability-zones.md
[vm-updates]: ../virtual-machines/maintenance-and-updates.md
