---
title: Azure Arc etkin Kubernetes genel bakış
services: azure-arc
ms.service: azure-arc
ms.date: 05/19/2020
ms.topic: overview
author: mlearned
ms.author: mlearned
description: ''
keywords: Kubernetes, yay, Azure, kapsayıcılar
ms.openlocfilehash: 3b3b184ff7b18e05dfeb1052c0d6b4aa406a8787
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83666261"
---
# <a name="what-is-azure-arc-enabled-kubernetes-preview"></a>Azure Arc özellikli Kubernetes nedir (Önizleme)

Azure Arc etkin Kubernetes (Önizleme) ile Azure 'un içinde veya dışında Kubernetes kümelerini iliştirebilir ve yapılandırabilirsiniz. Azure yaya bir Kubernetes kümesi eklendiğinde, Azure portalında görünür, Azure Resource Manager KIMLIĞI ve yönetilen bir kimlik olur. Kümeler, kaynak grubunda bulunan standart Azure aboneliklerine iliştirilir ve diğer tüm Azure kaynakları gibi etiketleri alabilir. 


Bir Kubernetes kümesini Azure 'a bağlamak için bir küme yöneticisinin aracıları dağıtması gerekir. Bu aracılar adlı bir Kubernetes ad alanında çalışır `azure-arc` ve standart Kubernetes dağıtımlarıdır. Aracılar Azure ile bağlantı sağlanmasından, Azure Arc günlüklerini ve ölçümlerini toplamaya ve yapılandırma isteklerini izlemeye sorumludur.  
 
 > [!NOTE]
> Azure Arc etkin Kubernetes önizlemededir ve üretim iş yükleri için önerilmez. 


## <a name="supported-scenarios"></a>Desteklenen Senaryolar 

Azure Arc etkin Kubernetes aşağıdaki senaryoları destekler: 

* Envanter, gruplama ve etiketleme için Azure dışında çalışan Kubernetes 'i bağlama 

* Gilar tabanlı yapılandırma yönetimini kullanarak uygulama dağıtma ve yapılandırma uygulama 

* Kümelerinizi görüntülemek ve izlemek için kapsayıcılar için Azure Izleyicisini kullanın 

* Kubernetes için Azure Ilkesini kullanarak ilke uygulama 

 
## <a name="supported-regions"></a>Desteklenen bölgeler 

Azure Arc etkin Kubernetes Şu anda şu bölgelerde destekleniyor: 

* Doğu ABD 
* Batı Avrupa 


## <a name="next-steps"></a>Sonraki adımlar

* [Bir kümeyi bağlama](./connect-cluster.md)
