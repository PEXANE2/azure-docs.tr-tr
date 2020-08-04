---
title: Azure Kubernetes hizmeti (AKS) düğümlerini otomatik olarak onarma
description: Düğüm otomatik onarma işlevselliği ve AKS 'in kopuk çalışan düğümlerini düzeltme hakkında bilgi edinin.
services: container-service
ms.topic: conceptual
ms.date: 06/02/2020
ms.openlocfilehash: 7fcb7b380f3694aaf34328019c3e09f5157c9e64
ms.sourcegitcommit: 8def3249f2c216d7b9d96b154eb096640221b6b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87542051"
---
# <a name="azure-kubernetes-service-aks-node-auto-repair"></a>Azure Kubernetes hizmeti (AKS) düğümü otomatik onarma

AKS, çalışan düğümlerinin sistem durumunu sürekli olarak denetler ve sağlıksız hale gelirse düğümlerin otomatik olarak onarımını gerçekleştirir. Bu belge, otomatik düğüm onarma işlevinin nasıl davranacağını gösteren işleçlere bildirir. Azure VM platformu, AKS onarımına ek olarak, sorunlar yaşayan [sanal makinelerde de bakım gerçekleştirir][vm-updates] . AKS ve Azure VM 'Leri, kümelerdeki hizmet kesintilerini en aza indirmek için birlikte çalışır.

## <a name="limitations"></a>Sınırlamalar

* Windows düğüm havuzları bugün desteklenmez.

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

Bir düğümün Yukarıdaki kurallara göre sağlıksız olduğu belirlenirse ve ardışık olarak 10 dakika boyunca sağlıksız kalırsa, AKS düğümü yeniden başlatır. İlk onarma işleminden sonra düğümler sağlıksız kalırsa, ek düzeltmeler AKS mühendisleri tarafından araştırılır.
  
Bir sistem durumu denetimi sırasında birden çok düğüm sağlıksız ise, başka bir onarım başlamadan önce her düğüm tek tek onarılır.

## <a name="next-steps"></a>Sonraki adımlar

AKS kümesi iş yüklerinizde yüksek kullanılabilirliği artırmak için [kullanılabilirlik alanları][availability-zones] kullanın.

<!-- LINKS - External -->

<!-- LINKS - Internal -->
[availability-zones]: ./availability-zones.md
[vm-updates]: ../virtual-machines/maintenance-and-updates.md
