---
title: Bilişsel hizmetler metin analizi kaynak oluştur
titleSuffix: Azure Cognitive Services
description: Bilişsel hizmetler metin analizi kaynak oluşturmayı öğrenin.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: 7e8b4480911f00afa8524ef4b81d697bb8ee5bcc
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/15/2019
ms.locfileid: "67877470"
---
## <a name="create-a-cognitive-services-text-analytics-resource"></a>Bilişsel hizmetler metin analizi kaynak oluştur

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Seçin **kaynak Oluştur**ve ardından **yapay ZEKA + makine öğrenimi** > **metin analizi**.
   Veya Git [oluşturma metin analizi](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics).
1. Gerekli tüm ayarları girin:

    |Ayar|Value|
    |--|--|
    |Ad|(2-64 karakter) bir ad girin|
    |Subscription|Uygun aboneliği seçin|
    |Location|Yakın bir konum seçin|
    |Fiyatlandırma katmanı| Girin **S**, standart fiyatlandırma katmanı|
    |Resource group|Kullanılabilir kaynak grubu seçin|

1. Seçin **Oluştur** ve kaynak oluşturulmasını bekleyin. Tarayıcınız otomatik olarak yeni oluşturulan kaynak sayfasına yönlendirir.
1. Yapılandırılmış toplama `endpoint` ve API anahtarı:

    |Portal'daki kaynak sekmesi|Ayar|Value|
    |--|--|--|
    |**Genel bakış**|Uç Nokta|Uç nokta kopyalayın. Benzer şekilde görünür `https://northeurope.api.cognitive.microsoft.com/text/analytics/v2.0`|
    |**anahtarları**|API anahtarı|İki anahtarlardan birini kopyalayın. Boşluk veya tire ile 32 karakter alfasayısal bir dize olan: <`xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`>.|
