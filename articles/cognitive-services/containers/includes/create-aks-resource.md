---
title: Azure Kubernetes Hizmet kümesi kaynağı oluşturma
titleSuffix: Azure Cognitive Services
description: Azure Kubernetes Hizmeti (AKS) kaynağını nasıl oluşturabilirsiniz öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: d01245a76c5210a1843307b543f98d4f1d59d2cc
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80877871"
---
## <a name="create-an-azure-kubernetes-service-cluster-resource"></a>Azure Kubernetes Hizmet kümesi kaynağı oluşturma

1. Azure [Kubernetes Hizmeti'ne](https://ms.portal.azure.com/#create/microsoft.aks)gidin ve **Oluştur'u**seçin.

1. Temel **Bilgiler** sekmesinde aşağıdaki bilgileri girin:

    |Ayar|Değer|
    |--|--|
    |Abonelik|Uygun bir abonelik seçin.|
    |Kaynak grubu|Kullanılabilir bir kaynak grubu seçin.|
    |Kubernetes küme adı|Bir ad girin (küçük harf).|
    |Bölge|Yakındaki bir konumu seçin.|
    |Kubernetes sürümü|Hangi değer **(varsayılan)** olarak işaretlenirse.|
    |DNS ad öneki|Otomatik olarak oluşturulur, ancak geçersiz kılınabilirsiniz.|
    |Düğüm boyutu|Standart DS2 v2:<br>`2 vCPUs`, `7 GB`|
    |Düğüm sayısı|Kaydırıcıyı varsayılan değerde bırakın.|

1. **Ölçek** sekmesinde, **Sanal düğümleri** ve **VM ölçek kümelerini** varsayılan değerlerine bırakın.
1. Kimlik **Doğrulama** sekmesinde, **Hizmet anasını** bırakın ve RBAC kümesini varsayılan değerlerine **ayarlayın.**
1. **Ağ** sekmesinde aşağıdaki seçimleri girin:

    |Ayar|Değer|
    |--|--|
    |HTTP uygulaması yönlendirme|Hayır|
    |Ağ yapılandırması|Temel|

1. **İzleme** sekmesinde, **kapsayıcı izlemeyi etkinleştir'in** **Evet**olarak ayarlandıklarından emin olun ve **Log Analytics çalışma alanını** varsayılan değer olarak bırakın.
1. **Etiketler** sekmesinde, ad/değer çiftlerini şimdilik boş bırakın.
1. **Gözden Geçir ve Oluştur'u**seçin.
1. Doğrulama geçtikten sonra **Oluştur'u**seçin.

> [!NOTE]
> Doğrulama başarısız olursa, bunun nedeni bir "Hizmet sorumlusu" hatası olabilir. **Kimlik Doğrulama** sekmesine geri dön ve ardından doğrulamanın çalışması ve sonra geçmesi gereken Gözden Geçir **+ oluştur'a**geri dön.
