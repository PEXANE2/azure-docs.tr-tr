---
title: Bir Azure Kubernetes hizmeti küme kaynağı oluşturun
titleSuffix: Azure Cognitive Services
description: Azure Kubernetes Service (AKS) kaynak oluşturmayı öğrenin.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 7/3/2019
ms.author: dapine
ms.openlocfilehash: ab7ce8b4a538e6a286a00285069a22878c5d88d9
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/15/2019
ms.locfileid: "67877457"
---
## <a name="create-an-azure-kubernetes-service-cluster-resource"></a>Bir Azure Kubernetes hizmeti küme kaynağı oluşturun

1. Git [Azure Kubernetes hizmeti](https://ms.portal.azure.com/#create/microsoft.aks) seçip **Oluştur**.

1. Üzerinde **Temelleri** sekmesinde, aşağıdaki bilgileri girin:

    |Ayar|Değer|
    |--|--|
    |Subscription|Uygun bir abonelik seçin|
    |Resource group|Kullanılabilir kaynak grubu seçin|
    |Kubernetes küme adı|(Küçük) bir ad girin|
    |Bölge|Yakın bir konum seçin|
    |Kubernetes sürümü|1.12.8 (varsayılan)|
    |DNS adı ön eki|Otomatik olarak oluşturuldu ancak geçersiz kılabilirsiniz|
    |Düğüm boyutu|Standart DS2 v2'de:<br>`2 vCPUs`, `7 GB`|
    |Düğüm sayısı|Kaydırıcı varsayılan değerde bırakın|

1. Üzerinde **ölçek** sekmesinde **sanal düğümler** ve **sanal makine ölçek kümeleri (Önizleme)** varsayılan değerlere ayarlayın.
1. Üzerinde **kimlik doğrulaması** sekmesinde **hizmet sorumlusu** ve **etkinleştirme RBAC** varsayılan değerlere ayarlayın.
1. Üzerinde **ağ** sekmesinde, aşağıdaki seçimleri girin:

    |Ayar|Value|
    |--|--|
    |HTTP uygulaması yönlendirme|Hayır|
    |Ağ yapılandırması|Temel|

1. Üzerinde **izleme** sekmesinde, emin **kapsayıcı izlemeyi etkinleştir** ayarlanır **Evet**, bırakıp **Log Analytics çalışma alanı** olarak Varsayılan değer.
1. Üzerinde **etiketleri** sekmesinde, ad/değer çiftleri şimdilik boş bırakın.
1. Seçin **gözden geçir ve Oluştur**.
1. Doğrulama denetimini geçtikten seçin **Oluştur**.

> [!NOTE]
> Doğrulama başarısız olursa, bir "Hizmet sorumlusu" hatası nedeniyle olabilir. Geri gidin **kimlik doğrulaması** sekmesine ve ardından yeniden **gözden geçir + Oluştur**, nerede doğrulama çalıştırın ve ardından geçirin.
