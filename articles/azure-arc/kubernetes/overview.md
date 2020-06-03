---
title: Azure Arc etkin Kubernetes 'e genel bakış
services: azure-arc
ms.service: azure-arc
ms.date: 05/19/2020
ms.topic: overview
author: mlearned
ms.author: mlearned
description: Bu makalede, Azure Arc özellikli Kubernetes 'e genel bakış sunulmaktadır.
keywords: Kubernetes, yay, Azure, kapsayıcılar
ms.custom: references_regions
ms.openlocfilehash: 3960a0f0b6866a0a5d3476e841d29c6e5a89a238
ms.sourcegitcommit: 69156ae3c1e22cc570dda7f7234145c8226cc162
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84309943"
---
# <a name="what-is-azure-arc-enabled-kubernetes-preview"></a>Azure Arc özellikli Kubernetes önizlemesi nedir?

Azure Arc özellikli Kubernetes önizlemesi 'ni kullanarak Azure 'un içindeki veya dışındaki Kubernetes kümelerini iliştirebilir ve yapılandırabilirsiniz. Azure yaya bir Kubernetes kümesi eklendiğinde Azure portal görüntülenir. Azure Resource Manager KIMLIĞI ve yönetilen kimlik olur. Kümeler standart Azure aboneliklerine iliştirilir, bir kaynak grubunda bulunur ve diğer tüm Azure kaynakları gibi etiketleri alabilir. 

Bir Kubernetes kümesini Azure 'a bağlamak için, küme yöneticisinin aracıları dağıtması gerekir. Bu aracılar adlı bir Kubernetes ad alanında çalışır `azure-arc` ve standart Kubernetes dağıtımlarıdır. Aracılar Azure ile bağlantı sağlanmasından, Azure Arc günlüklerini ve ölçümlerini toplamaya ve yapılandırma isteklerini izlemeye sorumludur. 

Azure Arc özellikli Kubernetes, yoldaki verilerin güvenliğini sağlamak için sektör standardı SSL 'yi destekler. Ayrıca veriler, verilerin gizliliğini sağlamak için bir Azure Cosmos DB veritabanında şifreli olarak depolanır.
 
 > [!NOTE]
> Azure Arc etkin Kubernetes önizleme aşamasındadır. Bunu üretim iş yükleri için önermiyoruz. 


## <a name="supported-scenarios"></a>Desteklenen senaryolar 

Azure Arc etkin Kubernetes bu senaryoları destekler: 

* Envanter, gruplama ve etiketleme için Azure dışında çalışan Kubernetes 'i bağlayın.

* Gilar tabanlı yapılandırma yönetimini kullanarak uygulamaları dağıtın ve yapılandırma uygulayın. 

* Kümelerinizi görüntülemek ve izlemek için kapsayıcılar için Azure Izleyici 'yi kullanın. 

* Kubernetes için Azure Ilkesini kullanarak ilkeleri uygulayın. 

 
## <a name="supported-regions"></a>Desteklenen bölgeler 

Azure Arc özellikli Kubernetes Şu anda şu bölgelerde destekleniyor: 

* Doğu ABD 
* Batı Avrupa 

## <a name="next-steps"></a>Sonraki adımlar

* [Bir kümeyi bağlama](./connect-cluster.md)
