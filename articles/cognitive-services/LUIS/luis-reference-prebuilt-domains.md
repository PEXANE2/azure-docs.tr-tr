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
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: 6e701396191ec8bf66ece0e2858b9f32857e7b17
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68933597"
---
# <a name="prebuilt-domain-reference-for-your-luis-app"></a>LUSıS uygulamanız için önceden oluşturulmuş etki alanı başvurusu
Bu başvuru, hakkında bilgi sağlar. [önceden oluşturulmuş etki alanları](luis-how-to-use-prebuilt-domains.md), önceden oluşturulmuş koleksiyon hedefleri ve LUIS sunan varlıkların olduğu.

[Özel etki alanları](luis-how-to-start-new-app.md), aksine, hiçbir hedefleri ve modelleri başlayın. Herhangi bir önceden oluşturulmuş etki alanı hedefleri ve varlıklar için özel bir model ekleyebilirsiniz.

## <a name="supported-domains-across-cultures"></a>Kültürler genelinde desteklenen etki alanları

Desteklenen tek kültür İngilizce 'dir. 

<!--

The table below summarizes the currently supported domains. Support for English is usually more complete than others.


| Entity Type       | EN-US      | ZH-CN   | DE    | FR     | ES    | IT      | PT-BR |  JP  |      KO |        NL |    TR |
|:-----------------:|:-------:|:-------:|:-----:|:------:|:-----:|:-------:| :-------:| :-------:| :-------:| :-------:|  :-------:| 
| Calendar    | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | -      | -    | -    | -     | -  |
| Communication   | ✓    | -       | ✓    | ✓     | ✓     | ✓  | -  | -      | -    | -    | -     | -  |
| Email           | ✓    | ✓       | ✓   | ✓     | ✓     | ✓  | -  | -      | -    | -    | -     | -  |
| HomeAutomation           | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | -  | -      | -    | -    | -     | -  |
| Notes      | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | -  | -      | -    | -    | -     | -  |
| Places    | ✓    | -       | ✓    | ✓     | ✓     | ✓  | -  | -      | -    | -    | -     | -  |
| RestaurantReservation   | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | -  | -      | -    | -    | -     | -  |
| ToDo     | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | -  | -      | -    | -    | -     | -  |
| ToDo_IPA        | ✓    | ✓       | ✓    | ✓      | ✓     | ✓       | -  | -      | -    | -    | -     | -  |
| Utilities          | ✓    | ✓        | ✓    | ✓      | ✓     | ✓       | -  | -      | -    | -    | -     | -  |
| Weather        | ✓    | ✓        | ✓    | ✓      | ✓     | ✓       | -  | -      | -    | -    | -     | -  |
| Web    | ✓    | -        | ✓    | ✓      | ✓     | ✓       | -  | -      | -    | -    | -     | -  |
||||||||||||| 

-->

<br><br>

|Varlık türü|description|
|--|--|
|Takvim|Takvim, kişisel toplantılar ve randevular hakkında (Dünya fincanı zamanlamaları, Seattle olay takvimleri gibi) veya genel takvimler (Bugün günün ne olduğu, işçilik günü ne kadar sürer).|
|İletişim|Çağrı yapma, metin veya anlık ileti gönderme, kişileri bulma ve ekleme ve diğer iletişimle ilgili istekleri (genellikle giden) bulma istekleri. Kişi adı yalnızca sorgular Iletişim etki alanına ait değildir.|
|Email|E-posta, Iletişim etki alanının bir alt etki alanıdır. Genellikle e-posta yoluyla ileti gönderme ve alma isteklerini içerir.|
|HomeAutomation|HomeAutomation etki alanı, akıllı giriş cihazlarını denetleme ile ilgili amaçlar ve varlıklar sağlar. Genellikle ışıklar ve hava koşullayıcı ile ilgili denetim komutunu destekler, ancak diğer elektrik gereçlerine yönelik bazı Genelleştirme becerileri vardır.|
|Notlar|Not etki alanı, notları oluşturmak ve kullanıcılar için öğeleri yazmak üzere amaçlar ve varlıklar sağlar.|
|Yerler|Yerler arasında işletmeler, kurumlar, restoranlar, kamu alanları ve adresler bulunur. Etki alanı, konum bulma ve konum, çalışma saati ve mesafe gibi genel bir yerin bilgilerini ister.|
|RestaurantReservation|Restoran ayırma etki alanı, restoranlar için rezervasyonları işleme amaçlarını destekler.|
|ToDo|ToDo etki alanı, kullanıcıların Todo öğelerini eklemesi, işaretlemesi ve silmesi için görev listesi türleri sağlar.|
|Altyapı Hizmetleri|Yardımcı programlar etki alanı, tüm lu, önceden oluşturulmuş modellerdeki, fark senaryolarında ortak amaçlar ve söyler içeren genel bir etki alanıdır.|
|Hava durumu|Hava durumu etki alanı, hava durumu koşullarına ve BT danışmanlarını, hava durumu koşullarına göre konum ve saat ya da denetim saatine|
|Web|Web etki alanı, bir Web sitesi aramak için amaç ve varlıklar sağlar.|
