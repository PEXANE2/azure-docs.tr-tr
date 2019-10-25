---
title: Kapsayıcı desteği
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 10/23/2019
ms.author: dapine
ms.openlocfilehash: ecb172aec37fdab21bed8373ac7484dc52a54829
ms.sourcegitcommit: 8e271271cd8c1434b4254862ef96f52a5a9567fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72822049"
---
## <a name="create-a-luis-resource"></a>LUSıS kaynağı oluşturma

1. [Azure Portal](https://portal.azure.com) oturum açın
1. [Oluştur **Language Understanding** ](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUISAllInOne) tıklayın
1. Tüm gerekli ayarları girin:

    |Ayar|Değer|
    |--|--|
    |Adı|İstenen ad (2-64 karakter)|
    |Abonelik|Uygun aboneliği seçin|
    |Konum|Yakındaki ve kullanılabilir herhangi bir konumu seçin|
    |Fiyatlandırma Katmanı|`F0`-minimum Fiyatlandırma Katmanı|
    |Kaynak Grubu|Kullanılabilir bir kaynak grubu seçin|

1. **Oluştur** ' a tıklayın ve kaynağın oluşturulmasını bekleyin. Oluşturulduktan sonra kaynak sayfasına gidin
1. Yapılandırılmış `endpoint` ve bir API anahtarı toplayın:

    |Portalda kaynak sekmesi|Ayar|Değer|
    |--|--|--|
    |**Genel Bakış**|Uç nokta|Uç noktayı kopyalayın. Şuna benzer `https://luis.cognitiveservices.azure.com/luis/v2.0`|
    |**Belirlenmesine**|API anahtarı|İki anahtardan oluşan 1 ' i kopyalayın. Boşluk veya kesik çizgi olmayan 32 alfasayısal karakter dizesidir `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.|
