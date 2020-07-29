---
title: Azure Kubernetes hizmet kümesi kaynağı oluşturma
titleSuffix: Azure Cognitive Services
description: Azure Kubernetes hizmeti (AKS) kaynağı oluşturmayı öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: e7f5b6f3685a94b5497784360f8f12b22fb95012
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87298722"
---
## <a name="create-an-azure-kubernetes-service-cluster-resource"></a>Azure Kubernetes hizmet kümesi kaynağı oluşturma

1. [Azure Kubernetes hizmetine](https://ms.portal.azure.com/#create/microsoft.aks)gidin ve **Oluştur**' u seçin.

1. **Temel bilgiler** sekmesinde, aşağıdaki bilgileri girin:

    |Ayar|Değer|
    |--|--|
    |Abonelik|Uygun bir abonelik seçin.|
    |Kaynak grubu|Kullanılabilir kaynak grubunu seçin.|
    |Kubernetes küme adı|Bir ad girin (küçük harf).|
    |Bölge|Yakındaki bir konumu seçin.|
    |Kubernetes sürümü|Herhangi bir değer **(varsayılan)** olarak işaretlenir.|
    |DNS adı ön eki|Otomatik olarak oluşturulur, ancak geçersiz kılabilirsiniz.|
    |Düğüm boyutu|Standart DS2 v2:<br>`2 vCPUs`, `7 GB`|
    |Düğüm sayısı|Kaydırıcıyı varsayılan değerde bırakın.|

1. **Düğüm havuzları** sekmesinde, **sanal düğümleri** ve **VM Ölçek kümelerini** varsayılan değerlerine ayarlayın.
1. **Kimlik doğrulama** sekmesinde **hizmet SORUMLUSUNU** bırakın ve RBAC kümesini varsayılan değerlerine **etkinleştirin** .
1. **Ağ** sekmesinde, aşağıdaki seçimleri girin:

    |Ayar|Değer|
    |--|--|
    |HTTP uygulaması yönlendirme|Hayır|
    |Ağ yapılandırması|Temel|

1. **Tümleştirmeler** sekmesinde, **kapsayıcı izlemenin** **etkin**olarak ayarlandığından emin olun ve **Log Analytics çalışma alanını** varsayılan değer olarak bırakın.
1. **Etiketler** sekmesinde, ad/değer çiftlerini şimdilik boş bırakın.
1. **Gözden geçir ve Oluştur '** u seçin.
1. Doğrulama geçtikten sonra **Oluştur**' u seçin.

> [!NOTE]
> Doğrulama başarısız olursa, bunun nedeni "hizmet sorumlusu" hatası olabilir. **Kimlik doğrulama** sekmesine dönün ve ardından doğrulama ' nın ardından Pass ve sonra geçmesi gereken **İnceleme + oluştur**' a geri dönün.
