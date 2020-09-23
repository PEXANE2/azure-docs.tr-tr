---
title: Kapsayıcı desteği
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/10/2020
ms.author: aahi
ms.openlocfilehash: 8f93e0ffe74ade79059fc845788faf36ab5b4f13
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "91025363"
---
## <a name="create-an-anomaly-detector-resource"></a>Anomali algılayıcısı kaynağı oluşturma

1. [Azure Portal](https://portal.azure.com) oturum açın
1. [ **Anomali algılayıcı** kaynağı oluştur](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAnomalyDetector) ' a tıklayın
1. Tüm gerekli ayarları girin:

    |Ayar|Değer|
    |--|--|
    |Ad|İstenen ad (2-64 karakter)|
    |Abonelik|Uygun aboneliği seçin|
    |Konum|Yakındaki ve kullanılabilir herhangi bir konumu seçin|
    |Fiyatlandırma Katmanı|`F0` -Minimum Fiyatlandırma Katmanı|
    |Kaynak Grubu|Kullanılabilir bir kaynak grubu seçin|

1. **Oluştur** ' a tıklayın ve kaynağın oluşturulmasını bekleyin. Oluşturulduktan sonra kaynak sayfasına gidin
1. Toplama yapılandırılmış `endpoint` ve bır API anahtarı:

    |Portalda kaynak sekmesi|Ayar|Değer|
    |--|--|--|
    |**Genel bakış**|Uç Noktası|Uç noktayı kopyalayın. Şuna benzer görünüyor `https://westus2.api.cognitive.microsoft.com/`|
    |**Anahtarlar**|API Anahtarı|İki anahtardan oluşan 1 ' i kopyalayın. Boşluk veya tire içermeyen 32 alfasayısal karakter dizesidir `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx` .|



