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
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80878383"
---
## <a name="create-an-face-resource"></a>Yüz kaynağı oluşturma

1. [Azure portalında](https://portal.azure.com) oturum açma
1. [ **Yüz** Oluştur](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFace) kaynağını tıklatın
1. Gerekli tüm ayarları girin:

    |Ayar|Değer|
    |--|--|
    |Adı|İstenilen ad (2-64 karakter)|
    |Abonelik|Uygun aboneliği seçin|
    |Konum|Yakındaki ve kullanılabilir herhangi bir konumu seçin|
    |Fiyatlandırma Katmanı|`F0`- minimum fiyatlandırma katmanı|
    |Kaynak Grubu|Kullanılabilir bir kaynak grubu seçme|

1. **Oluştur'u** tıklatın ve kaynağın oluşturulmasını bekleyin. Oluşturulduktan sonra kaynak sayfasına gidin
1. Yapılandırılmış `endpoint` ve api anahtarı toplamak:

    |Portaldaki Kaynak Sekmesi|Ayar|Değer|
    |--|--|--|
    |**Genel bakış**|Uç Nokta|Bitiş noktasını kopyalayın. Bu benzer görünüyor`https://face.cognitiveservices.azure.com/face/v1.0`|
    |**Anahtarlar**|API Anahtarı|İki anahtardan 1'ini kopyalayın. Boşluksuz veya tiresiz 32 alfanümerik karakterli bir `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`dizedir.|
