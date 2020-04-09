---
title: Bilişsel Hizmetler Metin Analizi kaynağı oluşturma
titleSuffix: Azure Cognitive Services
description: Bilişsel Hizmetler Metin Analizi kaynağını nasıl oluşturabileceğinizi öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 6cd653909e26dc5e0484ca289a1d2ab47e20457f
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80876473"
---
## <a name="create-a-cognitive-services-text-analytics-resource"></a>Bilişsel Hizmetler Metin Analizi kaynağı oluşturma

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. **Kaynak Oluştur'u**seçin ve ardından **Yapay AI + Machine Learning** > **Text Analytics**adresine gidin.
   Veya Metin [Analizi Oluştur'a](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics)gidin.
1. Gerekli tüm ayarları girin:

    |Ayar|Değer|
    |--|--|
    |Adı|Bir ad girin (2-64 karakter).|
    |Abonelik|Uygun aboneliği seçin.|
    |Konum|Yakındaki bir konumu seçin.|
    |Fiyatlandırma katmanı| Standart fiyatlandırma katmanı **olan S'yi**girin.|
    |Kaynak grubu|Kullanılabilir bir kaynak grubu seçin.|

1. **Oluştur'u**seçin ve kaynağın oluşturulmasını bekleyin. Tarayıcınız otomatik olarak yeni oluşturulan kaynak sayfasına yönlendirir.
1. Yapılandırılan `endpoint` ve api anahtarını toplayın:

    |Portaldaki kaynak sekmesi|Ayar|Değer|
    |--|--|--|
    |**Genel bakış**|Uç Nokta|Bitiş noktasını kopyalayın. Bu benzer görünüyor `https://northeurope.api.cognitive.microsoft.com/text/analytics/v2.0`.|
    |**Anahtarlar**|API Anahtarı|İki anahtardan birini kopyala. Boşluk suz veya tiresiz 32 karakterlik alfasayısal bir dize: <`xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`>.|
