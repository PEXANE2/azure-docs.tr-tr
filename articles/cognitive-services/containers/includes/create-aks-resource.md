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
ms.openlocfilehash: d01245a76c5210a1843307b543f98d4f1d59d2cc
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80877871"
---
## <a name="create-an-azure-kubernetes-service-cluster-resource"></a>Azure Kubernetes hizmet kümesi kaynağı oluşturma

1. [Azure Kubernetes hizmetine](https://ms.portal.azure.com/#create/microsoft.aks)gidin ve **Oluştur**' u seçin.

1. **Temel bilgiler** sekmesinde, aşağıdaki bilgileri girin:

    |Ayar|Değer|
    |--|--|
    |Abonelik|Uygun bir abonelik seçin.|
    |Kaynak grubu|Kullanılabilir bir kaynak grubu seçin.|
    |Kubernetes küme adı|Bir ad girin (küçük harf).|
    |Bölge|Yakın bir konum seçin.|
    |Kubernetes sürümü|Herhangi bir değer **(varsayılan)** olarak işaretlenir.|
    |DNS adı ön eki|Otomatik olarak oluşturulur, ancak geçersiz kılabilirsiniz.|
    |Düğüm boyutu|Standart DS2 v2:<br>`2 vCPUs`, `7 GB`|
    |Düğüm sayısı|Kaydırıcıyı varsayılan değerde bırakın.|

1. **Ölçek** sekmesinde, **sanal düğümleri** ve **VM Ölçek kümelerini** varsayılan değerlerine ayarlayın.
1. **Kimlik doğrulama** sekmesinde **hizmet SORUMLUSUNU** bırakın ve RBAC kümesini varsayılan değerlerine **etkinleştirin** .
1. **Ağ** sekmesinde, aşağıdaki seçimleri girin:

    |Ayar|Değer|
    |--|--|
    |HTTP uygulaması yönlendirme|Hayır|
    |Ağ yapılandırması|Temel|

1. **İzleme** sekmesinde, **kapsayıcı izlemeyi etkinleştir** ' in **Evet**olarak ayarlandığından emin olun ve **Log Analytics çalışma alanını** varsayılan değer olarak bırakın.
1. **Etiketler** sekmesinde, ad/değer çiftlerini şimdilik boş bırakın.
1. **Gözden geçir ve Oluştur '** u seçin.
1. Doğrulama geçtikten sonra **Oluştur**' u seçin.

> [!NOTE]
> Doğrulama başarısız olursa, bunun nedeni "hizmet sorumlusu" hatası olabilir. **Kimlik doğrulama** sekmesine dönün ve ardından doğrulama ' nın ardından Pass ve sonra geçmesi gereken **İnceleme + oluştur**' a geri dönün.
