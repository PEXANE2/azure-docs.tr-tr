---
title: Bilişsel hizmetler Metin Analizi kaynak oluşturma
titleSuffix: Azure Cognitive Services
description: Bilişsel hizmetler Metin Analizi kaynağı oluşturmayı öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 6cd653909e26dc5e0484ca289a1d2ab47e20457f
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80876473"
---
## <a name="create-a-cognitive-services-text-analytics-resource"></a>Bilişsel hizmetler Metin Analizi kaynak oluşturma

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. **Kaynak oluştur**' u seçin ve ardından **AI + Machine Learning** > **metin analizi**' a gidin.
   Ya da [metin analizi oluştur](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics)' a gidin.
1. Gerekli tüm ayarları girin:

    |Ayar|Değer|
    |--|--|
    |Adı|Bir ad girin (2-64 karakter).|
    |Abonelik|Uygun aboneliği seçin.|
    |Konum|Yakın bir konum seçin.|
    |Fiyatlandırma katmanı| Standart fiyatlandırma katmanı olan **S**'yi girin.|
    |Kaynak grubu|Kullanılabilir bir kaynak grubu seçin.|

1. **Oluştur**' u seçin ve kaynağın oluşturulmasını bekleyin. Tarayıcınız yeni oluşturulan kaynak sayfasına otomatik olarak yeniden yönlendirir.
1. Yapılandırılmış `endpoint` ve bir API anahtarını toplayın:

    |Portalda kaynak sekmesi|Ayar|Değer|
    |--|--|--|
    |**Genel bakış**|Uç Nokta|Uç noktayı kopyalayın. Şuna benzer şekilde `https://northeurope.api.cognitive.microsoft.com/text/analytics/v2.0`görünür.|
    |**Anahtarlar**|API Anahtarı|İki anahtardan birini kopyalayın. Boşluk veya tire içermeyen 32 karakterlik alfasayısal bir dizedir: <`xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`>.|
