---
title: Azure Arc etkin Kubernetes 'e genel bakış
services: azure-arc
ms.service: azure-arc
ms.date: 02/17/2021
ms.topic: overview
author: mlearned
ms.author: mlearned
description: Bu makalede, Azure Arc etkin Kubernetes 'e genel bakış sunulmaktadır.
keywords: Kubernetes, yay, Azure, kapsayıcılar
ms.custom: references_regions
ms.openlocfilehash: 3d96c8c8764db89501da6fb9c498f0a3d20461af
ms.sourcegitcommit: 227b9a1c120cd01f7a39479f20f883e75d86f062
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2021
ms.locfileid: "100652539"
---
# <a name="what-is-azure-arc-enabled-kubernetes"></a>Azure Arc özellikli Kubernetes nedir?

Azure Arc etkin Kubernetes sayesinde, Azure 'un içinde veya dışında bulunan Kubernetes kümelerini iliştirebilir ve yapılandırabilirsiniz. Bir Kubernetes kümesini Azure yaya bağladığınızda, şu şekilde olur:
* Azure Resource Manager KIMLIĞI ve yönetilen kimlik ile Azure portal görüntülenir. 
* Standart Azure aboneliklerine eklenmelidir.
* Bir kaynak grubuna yerleştirilmelidir.
* Diğer tüm Azure kaynakları gibi etiketleri alın. 

Bir Kubernetes kümesini Azure 'a bağlamak için, küme yöneticisinin aracıları dağıtması gerekir. Bu aracılar:
* `azure-arc`Kubernetes ad alanında standart Kubernetes dağıtımları olarak çalıştırın.
* Azure ile bağlantıyı işleyin.
* Azure Arc günlüklerini ve ölçümlerini toplayın.
* Yapılandırma isteklerini izleyin. 

Azure Arc etkin Kubernetes, transit verileri güvenli hale getirmek için sektör standardı SSL 'yi destekler. Veri gizliliği sağlamak için bu veriler şifreli ve geri kalan bir Azure Cosmos DB veritabanında depolanır.
 
## <a name="supported-kubernetes-distributions"></a>Desteklenen Kubernetes dağıtımları

Azure Arc etkin Kubernetes, tüm bulut Yerel Bilgi Işlem altyapısı (CNCF) sertifikalı Kubernetes kümesi ile birlikte çalışarak:
* Azure 'da AKS-Engine
* Azure Stack hub 'da AKS-Engine
* GKE
* EKS
* VMware vSphere

Azure Arc etkin Kubernetes özellikleri, aşağıdaki dağıtımların yay ekibi tarafından test edilmiştir:
* RedHat OpenShift 4,3
* Ranhi RKE 1.0.8
* Kurallı Charmed Kubernetes 1,18
* AKS Engine
* Azure Stack hub 'da AKS altyapısı
* Azure Stack HIN üzerinde AKS
* Küme API sağlayıcısı Azure

## <a name="supported-scenarios"></a>Desteklenen senaryolar 

Azure Arc etkin Kubernetes aşağıdaki senaryoları destekler: 

* Envanter, gruplama ve etiketleme için Azure dışında çalışan Kubernetes 'i bağlayın.

* Gilar tabanlı yapılandırma yönetimini kullanarak uygulamaları dağıtın ve yapılandırma uygulayın. 

* Kapsayıcılar için Azure Izleyici 'yi kullanarak kümelerinizi görüntüleyin ve izleyin. 

* Kubernetes için Azure Ilkesini kullanarak ilkeleri uygulayın. 

[!INCLUDE [azure-lighthouse-supported-service](../../../includes/azure-lighthouse-supported-service.md)]

## <a name="supported-regions"></a>Desteklenen bölgeler 

Şu bölgelerde Azure Arc etkin Kubernetes Şu anda destekleniyor: 

* Doğu ABD 
* West Europe

## <a name="next-steps"></a>Sonraki adımlar

* [Bir kümeyi bağlama](./connect-cluster.md)
