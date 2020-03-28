---
title: V3 tahmini bitiş noktası nasıl elde edilir?
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 10/14/2019
ms.author: diberry
ms.openlocfilehash: 23e7f88b23ac7af7470183877eeb1be89e79779c
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80287756"
---
1. LUIS portalında, **Tahmin Kaynakları** sekmesindeki **Azure Kaynakları** sayfasında (sol menü) **Yönet** bölümünde (sağ üst menü) Yönet bölümünde, sayfanın altındaki **Sorgu örneğini** kopyalayın.

    URL'yi yeni bir tarayıcı sekmesine yapıştırın.

    URL'de uygulama kimliğiniz, anahtarınız ve yuva adınız vardır. V3 tahmini bitiş noktası URL'si aşağıdaki gibi görünür:

    `https://REGION.api.cognitive.microsoft.com/luis/prediction/v3.0/apps/APP-ID/slots/SLOT-NAME/predict?subscription-key=YOUR-KEY&<optional-name-value-pairs>&query=YOUR_QUERY_HERE`

