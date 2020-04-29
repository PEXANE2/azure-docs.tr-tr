---
title: Kapsayıcı desteği
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 7/5/2019
ms.author: aahi
ms.openlocfilehash: 0a330c2401412e1e3d5e2e49bf8121a681342808
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80878383"
---
## <a name="create-an-face-resource"></a>Yüz kaynağı oluşturma

1. [Azure Portal](https://portal.azure.com) oturum açın
1. [ **Yüz** kaynağı oluştur](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFace) ' a tıklayın
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
    |**Genel bakış**|Uç Nokta|Uç noktayı kopyalayın. Şuna benzer görünüyor`https://face.cognitiveservices.azure.com/face/v1.0`|
    |**Anahtarlar**|API Anahtarı|İki anahtardan oluşan 1 ' i kopyalayın. Boşluk veya tire içermeyen 32 alfasayısal karakter dizesidir `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.|
