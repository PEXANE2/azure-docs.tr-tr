---
title: Azure Kubernetes Service (AKS) düğümlerini otomatik olarak onarMa
description: Düğüm otomatik onarım işlevi ve AKS'nin bozuk alt düğümleri nasıl düzelttikleri hakkında bilgi edinin.
services: container-service
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: 9bf9df69a0a6bfa4d9f4029278d2a146811980c8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80284849"
---
# <a name="azure-kubernetes-service-aks-node-auto-repair"></a>Azure Kubernetes Service (AKS) düğüm otomatik onarım

AKS, işçi düğümlerinin sağlık durumunu sürekli olarak kontrol eder ve sağlıksız hale geldiklerinde düğümlerin otomatik olarak onarılmasını gerçekleştirir. Bu belge, Azure Kubernetes Service'in (AKS) işçi düğümlerini nasıl izlediğini ve sağlıksız işçi düğümlerini nasıl onardığını açıklar.  Belgeler, düğüm onarım işlevinin davranışı hakkında AKS operatörleribilgilendirmektir. Azure platformunun, sorunlarla karşılaşan [Sanal Makinelerde bakım gerçekleştirdiğini][vm-updates] de unutmayın. AKS ve Azure, kümelerinizdeki hizmet kesintilerini en aza indirmek için birlikte çalışır.

> [!Important]
> Düğüm otomatik onarım işlevi şu anda Windows Server düğüm havuzları için desteklenmez.

## <a name="how-aks-checks-for-unhealthy-nodes"></a>AKS sağlıksız düğümleri nasıl denetler?

> [!Note]
> AKS kullanıcı hesabı **aks-düzeltici**ile düğümler üzerinde onarım eylemi alır.

AKS, düğümün sağlıksız bir durum olup olmadığını ve onarılması gerekip gerekmeden belirlemek için kurallar kullanır. AKS, otomatik onarım gerekip gerekip gerekmeden gerek ipolmadığını belirlemek için aşağıdaki kuralları kullanır.

* Düğüm, 10 dakikalık bir zaman dilimi içinde ardışık çeklerde **NotReady** durumunu bildirir
* Düğüm 10 dakika içinde bir durum bildirmez

Kubectl ile düğümlerinizin sağlık durumunu el ile kontrol edebilirsiniz. 

```
kubectl get nodes
```

## <a name="how-automatic-repair-works"></a>Otomatik onarım nasıl çalışır?

> [!Note]
> AKS kullanıcı hesabı **aks-düzeltici**ile düğümler üzerinde onarım eylemi alır.

Bu davranış **Sanal Makine Ölçek Kümeleri**içindir.  Otomatik onarım, bozuk bir düğümü onarmak için birkaç adım atar.  Bir düğümün sağlıksız olduğu tespit edilirse, AKS birkaç düzeltme adımı dener.  Adımlar bu sırada gerçekleştirilir:

1. Kapsayıcı çalışma süresi 10 dakika boyunca yanıt vermiyor sonra, başarısız çalışma zamanı hizmetleri düğüm üzerinde yeniden başlatılır.
2. Düğüm 10 dakika içinde hazır değilse, düğüm yeniden başlatılır.
3. Düğüm 30 dakika içinde hazır değilse, düğüm yeniden görüntülenir.

> [!Note]
> Birden çok düğüm sağlıksızsa, tek tek onarılır

## <a name="next-steps"></a>Sonraki adımlar

AKS küme iş yükleri ile yüksek kullanılabilirliği artırmak için [Kullanılabilirlik Bölgeleri][availability-zones] kullanın.

<!-- LINKS - External -->

<!-- LINKS - Internal -->
[availability-zones]: ./availability-zones.md
[vm-updates]: ../virtual-machines/maintenance-and-updates.md
