---
title: Kapsayıcı desteği
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/10/2020
ms.author: mbullwin
ms.openlocfilehash: d1add17c1c84d2a22d76aaa1f96aeca4db645ba7
ms.sourcegitcommit: 2ba6303e1ac24287762caea9cd1603848331dd7a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2020
ms.locfileid: "97506359"
---
## <a name="create-an-anomaly-detector-resource"></a>Anomali algılayıcısı kaynağı oluşturma

1. <a href="https://portal.azure.com" target="_blank">Azure Portal <span class="docon docon-navigate-external x-hidden-focus"></span> </a>oturum açın.
1. <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAnomalyDetector" target="_blank">Anomali algılayıcı <span class="docon docon-navigate-external x-hidden-focus"></span> kaynağı oluştur</a> ' u seçin.
1. Tüm gerekli ayarları girin:

    |Ayar|Değer|
    |--|--|
    |Ad|İstenen ad (2-64 karakter)|
    |Abonelik|Uygun aboneliği seçin|
    |Konum|Yakındaki ve kullanılabilir herhangi bir konumu seçin|
    |Fiyatlandırma Katmanı|`F0` -100 çağrı/saniye, aylık 20K Işlem. <br> Veya<br> `S0` -80 çağrı/saniye|
    |Kaynak Grubu|Kullanılabilir bir kaynak grubu seçin|

1. **Oluştur** ' a tıklayın ve kaynağın oluşturulmasını bekleyin. Oluşturulduktan sonra kaynak sayfasına gidin
1. Toplama yapılandırılmış `endpoint` ve bır API anahtarı:

    |Portalda anahtarlar ve uç nokta sekmesi|Ayar|Değer|
    |--|--|--|
    |**Genel Bakış**|Uç Nokta|Uç noktayı kopyalayın. Şuna benzer görünüyor ` https://<your-resource-name>.cognitiveservices.azure.com/`|
    |**Anahtarlar**|API Anahtarı|İki anahtardan oluşan 1 ' i kopyalayın. Boşluk veya tire içermeyen 32 alfasayısal karakter dizesidir `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx` .|



