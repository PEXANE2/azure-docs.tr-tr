---
title: Kapsayıcı desteği
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 7/5/2019
ms.author: trbye
ms.openlocfilehash: 8abd520a7bb80f1f9a2d8ebcbe46b90fe59a96ec
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81422147"
---
## <a name="create-a-speech-resource"></a>Konuşma kaynağı oluşturma

1. [Azure Portal](https://portal.azure.com) oturum açın
1. [ **Konuşma** kaynağı oluştur](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices) ' a tıklayın
1. Tüm gerekli ayarları girin:

    |Ayar|Değer|
    |--|--|
    |Adı|İstenen ad (2-64 karakter)|
    |Abonelik|Uygun aboneliği seçin|
    |Konum|Yakındaki ve kullanılabilir herhangi bir konumu seçin|
    |Fiyatlandırma Katmanı|`F0`-Minimum Fiyatlandırma Katmanı|
    |Kaynak Grubu|Kullanılabilir bir kaynak grubu seçin|

1. **Oluştur** ' a tıklayın ve kaynağın oluşturulmasını bekleyin. Oluşturulduktan sonra kaynak sayfasına gidin
1. Toplama yapılandırılmış `endpoint` ve bir API anahtarı:

    |Portalda kaynak sekmesi|Ayar|Değer|
    |--|--|--|
    |**Genel bakış**|Uç Nokta|Uç noktayı kopyalayın. Şuna benzer görünüyor`https://speech.cognitiveservices.azure.com/sts/v1.0/issuetoken`|
    |**Anahtarlar**|API Anahtarı|İki anahtardan oluşan 1 ' i kopyalayın. Boşluk veya tire içermeyen 32 alfasayısal karakter dizesidir `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.|
