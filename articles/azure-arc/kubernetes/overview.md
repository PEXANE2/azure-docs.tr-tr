---
title: Azure Arc etkin Kubernetes 'e genel bakış
services: azure-arc
ms.service: azure-arc
ms.date: 02/15/2021
ms.topic: overview
author: mlearned
ms.author: mlearned
description: Bu makalede, Azure Arc etkin Kubernetes 'e genel bakış sunulmaktadır.
keywords: Kubernetes, yay, Azure, kapsayıcılar
ms.custom: references_regions
ms.openlocfilehash: 54b439d73ed5dfb0709d33dea1f588b8bdf4489f
ms.sourcegitcommit: de98cb7b98eaab1b92aa6a378436d9d513494404
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100560265"
---
# <a name="what-is-azure-arc-enabled-kubernetes-preview"></a>Azure Arc özellikli Kubernetes Önizlemesi nedir?

Azure Arc etkin Kubernetes önizlemesi 'ni kullanarak Azure 'un içindeki veya dışındaki Kubernetes kümelerini iliştirebilir ve yapılandırabilirsiniz. Azure yaya bir Kubernetes kümesi eklendiğinde Azure portal görüntülenir. Azure Resource Manager KIMLIĞI ve yönetilen kimlik olur. Kümeler standart Azure aboneliklerine iliştirilir, bir kaynak grubunda bulunur ve diğer tüm Azure kaynakları gibi etiketleri alabilir. 

Bir Kubernetes kümesini Azure 'a bağlamak için, küme yöneticisinin aracıları dağıtması gerekir. Bu aracılar adlı bir Kubernetes ad alanında çalışır `azure-arc` ve standart Kubernetes dağıtımlarıdır. Aracılar Azure ile bağlantı sağlanmasından, Azure Arc günlüklerini ve ölçümlerini toplamaya ve yapılandırma isteklerini izlemeye sorumludur. 

Azure Arc etkin Kubernetes, transit verileri güvenli hale getirmek için sektör standardı SSL 'yi destekler. Ayrıca veriler, verilerin gizliliğini sağlamak için bir Azure Cosmos DB veritabanında şifreli olarak depolanır.
 
> [!NOTE]
> Azure Arc etkin Kubernetes önizleme aşamasındadır. Bunu üretim iş yükleri için önermiyoruz.

## <a name="supported-kubernetes-distributions"></a>Desteklenen Kubernetes dağıtımları

Azure Arc etkin Kubernetes, Azure 'da AKS-Engine, Azure Stack hub 'da AKS-Engine, GKE, EKS ve VMware vSphere kümesi gibi tüm bulut Yerel Bilgi Işlem altyapısı (CNCF) sertifikalı Kubernetes kümesiyle birlikte çalışarak.

Azure Arc etkin Kubernetes özellikleri, aşağıdaki dağıtımlara göre yay ekibi tarafından test edilmiştir:
* RedHat OpenShift 4,3
* Ranhi RKE 1.0.8
* Kurallı Charmed Kubernetes 1,18
* AKS Engine
* Azure Stack hub 'da AKS altyapısı
* Azure Stack HIN üzerinde AKS
* Küme API sağlayıcısı Azure

## <a name="supported-scenarios"></a>Desteklenen senaryolar 

Azure Arc etkin Kubernetes bu senaryoları destekler: 

* Envanter, gruplama ve etiketleme için Azure dışında çalışan Kubernetes 'i bağlayın.

* Gilar tabanlı yapılandırma yönetimini kullanarak uygulamaları dağıtın ve yapılandırma uygulayın. 

* Kümelerinizi görüntülemek ve izlemek için kapsayıcılar için Azure Izleyici 'yi kullanın. 

* Kubernetes için Azure Ilkesini kullanarak ilkeleri uygulayın. 

[!INCLUDE [azure-lighthouse-supported-service](../../../includes/azure-lighthouse-supported-service.md)]

## <a name="supported-regions"></a>Desteklenen bölgeler 

Şu bölgelerde Azure Arc etkin Kubernetes Şu anda destekleniyor: 

* Doğu ABD 
* West Europe

## <a name="next-steps"></a>Sonraki adımlar

* [Bir kümeyi bağlama](./connect-cluster.md)
