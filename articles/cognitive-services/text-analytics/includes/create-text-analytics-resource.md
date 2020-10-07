---
title: Bilişsel Hizmetler Metin Analizi kaynağını oluşturma
titleSuffix: Azure Cognitive Services
description: Bilişsel hizmetler Metin Analizi kaynağı oluşturmayı öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: daafea59b96cc8da6b78f0733c9f54e0e4d3a8c2
ms.sourcegitcommit: ef69245ca06aa16775d4232b790b142b53a0c248
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/06/2020
ms.locfileid: "91779152"
---
## <a name="create-a-cognitive-services-text-analytics-resource"></a>Bilişsel Hizmetler Metin Analizi kaynağını oluşturma

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. **Kaynak oluştur**’u seçin ve ardından **Yapay Zeka + Makine Öğrenmesi** > **Metin Analizi**’ne gidin.
   Ya da [metin analizi oluştur](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics)' a gidin.
1. Tüm gerekli ayarları girin:

    |Ayar|Değer|
    |--|--|
    |Ad|Bir ad girin (2-64 karakter arası).|
    |Abonelik|Uygun aboneliği seçin.|
    |Konum|Yakındaki bir konumu seçin.|
    |Fiyatlandırma katmanı| Standart fiyatlandırma katmanı için **S** girin.|
    |Kaynak grubu|Kullanılabilir kaynak grubunu seçin.|

1. **Oluştur**’u seçin ve kaynağın oluşturulmasını bekleyin. Tarayıcınız otomatik olarak yeni oluşturulan kaynak sayfasına yönlendirilir.
1. Yapılandırılmış `endpoint` ve bır API anahtarını toplayın:

    |Portalda kaynak sekmesi|Ayar|Değer|
    |--|--|--|
    |**Genel Bakış**|Uç Noktası|Uç noktayı kopyalayın. Şuna benzer şekilde görünür `https://northeurope.api.cognitive.microsoft.com/text/analytics/v2.0` .|
    |**Anahtarlar**|API Anahtarı|İki anahtardan birini kopyalayın. Boşluk veya tire içermeyen 32 karakterlik alfasayısal bir dizedir: <`xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`>.|
