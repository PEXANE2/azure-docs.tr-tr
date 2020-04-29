---
title: Kapsayıcı desteği
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 8dbe36693e551bd03022f4a00044b75b912c834a
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80875153"
---
## <a name="create-an-anomaly-detector-resource"></a>Anomali algılayıcısı kaynağı oluşturma

1. [Azure Portal](https://portal.azure.com) oturum açın
1. [ **Anomali algılayıcı** kaynağı oluştur](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAnomalyDetector) ' a tıklayın
1. Tüm gerekli ayarları girin:

    |Ayar|Değer|
    |--|--|
    |Adı|İstenen ad (2-64 karakter)|
    |Abonelik|Uygun aboneliği seçin|
    |Konum|Yakındaki ve kullanılabilir herhangi bir konumu seçin|
    |Fiyatlandırma Katmanı|`F0`-Minimum Fiyatlandırma Katmanı|
    |Kaynak Grubu|Kullanılabilir bir kaynak grubu seçin|
    |Önizleme onay kutusu (gerekli)|**Önizleme** bildiriminin okunup okunmayacağı|

1. **Oluştur** ' a tıklayın ve kaynağın oluşturulmasını bekleyin. Oluşturulduktan sonra kaynak sayfasına gidin
1. Toplama yapılandırılmış `endpoint` ve bir API anahtarı:

    |Portalda kaynak sekmesi|Ayar|Değer|
    |--|--|--|
    |**Genel bakış**|Uç Nokta|Uç noktayı kopyalayın. Şuna benzer görünüyor`https://westus2.api.cognitive.microsoft.com/`|
    |**Anahtarlar**|API Anahtarı|İki anahtardan oluşan 1 ' i kopyalayın. Boşluk veya tire içermeyen 32 alfasayısal karakter dizesidir `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.|



