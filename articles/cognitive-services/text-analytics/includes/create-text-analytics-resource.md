---
title: Bilişsel Hizmetler Metin Analizi kaynağı oluşturma
titleSuffix: Azure Cognitive Services
description: Bilişsel Hizmetler Metin Analizi kaynağını nasıl oluşturabileceğinizi öğrenin.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 11/21/2019
ms.author: dapine
ms.openlocfilehash: d3b65174f3d161e0b8780a890f297d9d1c812b44
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74383423"
---
## <a name="create-a-cognitive-services-text-analytics-resource"></a>Bilişsel Hizmetler Metin Analizi kaynağı oluşturma

1. [Azure portalında](https://portal.azure.com)oturum açın.
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
