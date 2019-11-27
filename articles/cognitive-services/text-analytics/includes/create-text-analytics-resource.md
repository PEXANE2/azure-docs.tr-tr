---
title: Bilişsel hizmetler Metin Analizi kaynak oluşturma
titleSuffix: Azure Cognitive Services
description: Bilişsel hizmetler Metin Analizi kaynağı oluşturmayı öğrenin.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 11/21/2019
ms.author: dapine
ms.openlocfilehash: d3b65174f3d161e0b8780a890f297d9d1c812b44
ms.sourcegitcommit: f523c8a8557ade6c4db6be12d7a01e535ff32f32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74383423"
---
## <a name="create-a-cognitive-services-text-analytics-resource"></a>Bilişsel hizmetler Metin Analizi kaynak oluşturma

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. **Kaynak oluştur**' u seçin ve ardından **aı + Machine Learning** > **metin analizi**' na gidin.
   Ya da [metin analizi oluştur](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics)' a gidin.
1. Gerekli tüm ayarları girin:

    |Ayar|Value|
    |--|--|
    |Name|Bir ad girin (2-64 karakter).|
    |Abonelik|Uygun aboneliği seçin.|
    |Konum|Yakın bir konum seçin.|
    |Fiyatlandırma katmanı| Standart fiyatlandırma katmanı olan **S**'yi girin.|
    |Kaynak grubu|Kullanılabilir bir kaynak grubu seçin.|

1. **Oluştur**' u seçin ve kaynağın oluşturulmasını bekleyin. Tarayıcınız yeni oluşturulan kaynak sayfasına otomatik olarak yeniden yönlendirir.
1. Yapılandırılmış `endpoint` ve bir API anahtarını toplayın:

    |Portalda kaynak sekmesi|Ayar|Value|
    |--|--|--|
    |**Genel Bakış**|Uç Nokta|Uç noktayı kopyalayın. `https://northeurope.api.cognitive.microsoft.com/text/analytics/v2.0`benzer şekilde görünür.|
    |**Belirlenmesine**|API anahtarı|İki anahtardan birini kopyalayın. Boşluk veya tire içermeyen 32 karakterlik alfasayısal bir dizedir: <`xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`>.|
