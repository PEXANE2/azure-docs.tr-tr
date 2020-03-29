---
title: Önceden oluşturulmuş etki alanı başvurusu - LUIS
titleSuffix: Azure Cognitive Services
description: Dil Anlama Akıllı Hizmetler (LUIS) niyet ve varlıkların önceden oluşturulmuş koleksiyonları olan önceden oluşturulmuş etki alanları için başvuru.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78270607"
---
# <a name="prebuilt-domain-reference-for-your-luis-app"></a>LUIS uygulamanız için önceden oluşturulmuş etki alanı başvurusu
Bu başvuru, LUIS'in sunduğu niyet ve varlıkların önceden oluşturulmuş koleksiyonları olan [önceden oluşturulmuş etki alanları](luis-how-to-use-prebuilt-domains.md)hakkında bilgi sağlar.

[Özel etki alanları,](luis-how-to-start-new-app.md)aksine, hiçbir niyet ve model ile başlar. Önceden oluşturulmuş etki alanı niyetlerini ve varlıklarını özel bir modele ekleyebilirsiniz.

## <a name="custom-domains-per-language"></a>Dil başına özel etki alanları

Aşağıdaki tabloda şu anda desteklenen etki alanları özetlenmiştir. İngilizce desteği genellikle diğerlerinden daha eksiksizdir.

| Varlık Türü       | EN-BİZ      | ZH-CN   | DE    | GS     | ES    | BT      | PT-BR |  JP  |      KO |        NL |    TR |
|:-----------------:|:-------:|:-------:|:-----:|:------:|:-----:|:-------:| :-------:| :-------:| :-------:| :-------:|  :-------:|
| Takvim  | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
|İletişim  | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| Email     | ✓    | ✓       | ✓   | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| AnasayfaOtomasyon          | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| Notlar     | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| Yerler   | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| RestoranRezervasyon  | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| Todo     | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| Yardımcı Programlar      | ✓    | ✓        | ✓    | ✓      | ✓     | ✓       | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| Hava durumu        | ✓    | ✓        | ✓    | ✓      | ✓     | ✓       | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| Web    | ✓    | ✓        | ✓    | ✓      | ✓     | ✓       | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |

Önceden oluşturulmuş etki alanları şu şekilde **desteklenmez:**

* Fransız Kanada
* Hintçe
* İspanyol Meksika

## <a name="next-steps"></a>Sonraki adımlar

Basit [varlığı](reference-entity-simple.md)öğrenin.