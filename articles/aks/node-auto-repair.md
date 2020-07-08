---
title: Azure Kubernetes hizmeti (AKS) düğümlerini otomatik olarak onarma
description: Düğüm otomatik onarma işlevselliği ve AKS 'in kopuk çalışan düğümlerini düzeltme hakkında bilgi edinin.
services: container-service
ms.topic: conceptual
ms.date: 06/02/2020
ms.openlocfilehash: 91384461567634faabaaa1dd588d6e7ec6ece60e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84735634"
---
# <a name="azure-kubernetes-service-aks-node-auto-repair"></a>Azure Kubernetes hizmeti (AKS) düğümü otomatik onarma

AKS, çalışan düğümlerinin sistem durumunu sürekli olarak denetler ve sağlıksız hale gelirse düğümlerin otomatik olarak onarımını gerçekleştirir. Bu belge, otomatik düğüm onarma işlevinin nasıl davranacağını gösteren işleçlere bildirir. Azure VM platformu, AKS onarımına ek olarak, sorunlar yaşayan [sanal makinelerde de bakım gerçekleştirir][vm-updates] . AKS ve Azure VM 'Leri, kümelerdeki hizmet kesintilerini en aza indirmek için birlikte çalışır.

> [!Important]
> Düğüm otomatik onarım işlevselliği Şu anda Windows Server düğüm havuzları için desteklenmiyor.

## <a name="how-aks-checks-for-unhealthy-nodes"></a>AKS, sağlıksız düğümleri denetler

> [!Note]
> AKS, Kullanıcı hesabı **aks-düzeltici**olan düğümlerde bir onarım eylemi gerçekleştirir.

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

Otomatik onarma, **sanal makine ölçek kümelerinin**bir VM kümesi türüne sahip kümeler için varsayılan olarak desteklenir. Bir düğümün Yukarıdaki kurallara göre sağlıksız olduğu belirlenirse, AKS, ardışık olmayan 10 dakika sonra düğümü yeniden başlatır. İlk onarma işleminden sonra düğümler sağlıksız kalırsa, ek düzeltmeler AKS mühendisleri tarafından araştırılır.
  
Bir sistem durumu denetimi sırasında birden çok düğüm sağlıksız ise, başka bir onarım başlamadan önce her düğüm tek tek onarılır.

## <a name="next-steps"></a>Sonraki adımlar

AKS kümesi iş yüklerinizde yüksek kullanılabilirliği artırmak için [kullanılabilirlik alanları][availability-zones] kullanın.

<!-- LINKS - External -->

<!-- LINKS - Internal -->
[availability-zones]: ./availability-zones.md
[vm-updates]: ../virtual-machines/maintenance-and-updates.md
