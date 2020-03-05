---
title: Önceden oluşturulmuş etki alanı başvurusu-LUSıS
titleSuffix: Azure Cognitive Services
description: Önceden oluşturulmuş koleksiyonları hedefleri ve varlıkların gelen Language Understanding Intelligent Services (LUIS) önceden oluşturulmuş etki alanları için başvuru.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: 05ad340b3856291832ba0521c7da70ad55260384
ms.sourcegitcommit: d45fd299815ee29ce65fd68fd5e0ecf774546a47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2020
ms.locfileid: "78270607"
---
# <a name="prebuilt-domain-reference-for-your-luis-app"></a>LUSıS uygulamanız için önceden oluşturulmuş etki alanı başvurusu
Bu başvuru [önceden oluşturulan etki alanları](luis-how-to-use-prebuilt-domains.md)hakkında bilgi sağlar. Bu, önceden oluşturulmuş amaç ve diğer varlıkların sunduğu varlıklardır.

[Özel etki alanları](luis-how-to-start-new-app.md), aksine, amaç ve modeller olmadan başlar. Herhangi bir önceden oluşturulmuş etki alanı hedefleri ve varlıklar için özel bir model ekleyebilirsiniz.

## <a name="custom-domains-per-language"></a>Dil başına özel etki alanları

Aşağıdaki tabloda, şu anda desteklenen etki alanları özetlenmektedir. Ingilizce desteği genellikle diğerlerinden daha tamamdır.

| Varlık türü       | EN-US      | ZH-CN   | DE    | GS     | ES    | BT      | PT-BR |  JP  |      DILI |        NL |    TR |
|:-----------------:|:-------:|:-------:|:-----:|:------:|:-----:|:-------:| :-------:| :-------:| :-------:| :-------:|  :-------:|
| Takvim  | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
|İletişim  | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| Email     | ✓    | ✓       | ✓   | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| HomeAutomation          | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| Notlar     | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| Yerler   | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| RestaurantReservation  | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| ToDo     | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| Altyapı Hizmetleri      | ✓    | ✓        | ✓    | ✓      | ✓     | ✓       | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| Hava durumu        | ✓    | ✓        | ✓    | ✓      | ✓     | ✓       | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| Web    | ✓    | ✓        | ✓    | ✓      | ✓     | ✓       | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |

Önceden oluşturulmuş etki alanları ' de **desteklenmez** :

* Kanada Fransızcası
* Hintçe
* İspanyolca Meksika

## <a name="next-steps"></a>Sonraki adımlar

[Basit varlığı](reference-entity-simple.md)öğrenin.