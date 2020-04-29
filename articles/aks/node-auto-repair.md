---
title: Azure Kubernetes hizmeti (AKS) düğümlerini otomatik olarak onarma
description: Düğüm otomatik onarma işlevselliği ve AKS 'in kopuk çalışan düğümlerini düzeltme hakkında bilgi edinin.
services: container-service
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: 9bf9df69a0a6bfa4d9f4029278d2a146811980c8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80284849"
---
# <a name="azure-kubernetes-service-aks-node-auto-repair"></a>Azure Kubernetes hizmeti (AKS) düğümü otomatik onarma

AKS, çalışan düğümlerinin sistem durumunu sürekli olarak denetler ve sağlıksız hale gelirse düğümlerin otomatik olarak onarımını gerçekleştirir. Bu belgelerde, Azure Kubernetes hizmeti 'nin (AKS) çalışan düğümlerini nasıl izlediği ve sağlıksız çalışan düğümlerini nasıl onarmakta olduğu açıklanır.  Belgeler, düğüm onarma işlevinin davranışında AKS işleçlerini bilgilendirmektir. Ayrıca, Azure platformunun sorunlarla karşılaşan [sanal makinelerde bakım gerçekleştirdiğine][vm-updates] dikkat edin. AKS ve Azure, kümelerinizde hizmet kesintilerini en aza indirmek için birlikte çalışır.

> [!Important]
> Düğüm otomatik onarım işlevselliği Şu anda Windows Server düğüm havuzları için desteklenmiyor.

## <a name="how-aks-checks-for-unhealthy-nodes"></a>AKS, sağlıksız düğümleri denetler

> [!Note]
> AKS, Kullanıcı hesabı **aks-düzeltici**olan düğümlerde bir onarım eylemi gerçekleştirir.

AKS, bir düğümün sağlıksız durumda olup olmadığını ve onarılması gerekip gerekmediğini belirleme kurallarını kullanır. AKS, otomatik onarımın gerekip gerekmediğini anlamak için aşağıdaki kuralları kullanır.

* Düğüm, 10 dakikalık bir zaman dilimi içinde ardışık denetimlerin durumunu **NotReady** olarak bildiriyor
* Düğüm, 10 dakika içinde bir durum raporlamaz

Kubectl ile düğümlerinizin sistem durumunu el ile kontrol edebilirsiniz. 

```
kubectl get nodes
```

## <a name="how-automatic-repair-works"></a>Otomatik onarımın nasıl çalıştığı

> [!Note]
> AKS, Kullanıcı hesabı **aks-düzeltici**olan düğümlerde bir onarım eylemi gerçekleştirir.

Bu davranış, **sanal makine ölçek kümelerine**yöneliktir.  Otomatik onarma, bozuk bir düğümü onarmak için birkaç adım sürer.  Bir düğümün sağlıksız olduğu belirlenirse, AKS birkaç düzeltme adımını dener.  Adımlar şu sırayla gerçekleştirilir:

1. Kapsayıcı çalışma zamanı 10 dakika boyunca yanıt vermemeye başladıktan sonra, düğüm üzerinde başarısız çalışma zamanı hizmetleri yeniden başlatılır.
2. Düğüm 10 dakika içinde hazırsanız, düğüm yeniden başlatılır.
3. Düğüm 30 dakika içinde hazırsanız, düğüm yeniden görüntülenir.

> [!Note]
> Birden çok düğüm sağlıksız ise, bunlardan biri onarılır

## <a name="next-steps"></a>Sonraki adımlar

AKS kümesi iş yüklerinizde yüksek kullanılabilirliği artırmak için [kullanılabilirlik alanları][availability-zones] kullanın.

<!-- LINKS - External -->

<!-- LINKS - Internal -->
[availability-zones]: ./availability-zones.md
[vm-updates]: ../virtual-machines/maintenance-and-updates.md
