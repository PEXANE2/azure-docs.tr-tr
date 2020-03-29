---
title: Dil kavramları - QnA Maker
titleSuffix: Azure Cognitive Services
description: QnA Maker birçok dilde bilgi tabanı içeriğini destekler. Ancak, her QnA Maker hizmeti tek bir dil için rezerve edilmelidir. Belirli bir QnA Maker hizmetini hedefleyen oluşturulan ilk bilgi tabanı, bu hizmetin dilini ayarlar.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: diberry
ms.openlocfilehash: 38701e8bbef1c5d78eca2242105e81fe7261c0f6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79220638"
---
# <a name="language-support-of-knowledge-base-content-for-qna-maker"></a>QnA Maker için bilgi tabanı içeriğinin dil desteği

Kaynaktaki ilk bilgi tabanını oluşturduğunuzda hizmetin dili seçilir. Kaynaktaki tüm ek bilgi tabanları aynı dilde olmalıdır.

Dil, QnA Maker'ın kullanıcı sorgularına yanıt olarak sağladığı sonuçların alaka düzeyini belirler.

## <a name="one-language-for-all-knowledge-bases-in-resource"></a>Kaynaktaki tüm bilgi tabanları için tek dil

QnA Maker, ilk bilgi tabanını oluştururken QnA hizmetinizin dilini seçmenize olanak tanır. Bir QnA Maker kaynağındaki tüm bilgi tabanları için, hepsi aynı dilde olmalıdır. Bu dil değiştirilemez.

Tek bir kaynakta farklı dillerde bilgi tabanları oluşturmak, QnA Maker'ın kullanıcı sorgularına yanıt olarak sağladığı sonuçların alaka düzeyini olumsuz etkiler.

Desteklenen dillerin listesini ve dillerin [eşlemeyi ve alaka düzeyini](#query-matching-and-relevance)nasıl etkilediğini gözden [geçirin.](../overview/language-support.md#languages-supported)

## <a name="select-language-when-creating-first-knowledge-base"></a>İlk bilgi tabanını oluştururken dili seçin

Dil seçimi, bir kaynaktaki ilk bilgi tabanını oluşturma adımlarının bir parçasıdır.

![QnA Maker portal ilk bilgi tabanı için dil seçimi ekran görüntüsü](../media/language-support/select-language-when-creating-knowledge-base.png)

## <a name="query-matching-and-relevance"></a>Sorgu eşleştirme ve alaka düzeyi
QnA Maker, sonuç sağlamak için [Azure Bilişsel Arama dil çözümleyicilerine](https://docs.microsoft.com/rest/api/searchservice/language-support) bağlıdır.

Azure Bilişsel Arama özellikleri desteklenen diller için eşit olsa da, QnA Maker'ın Azure arama sonuçlarının üzerinde yer alan ek bir sıralaması vardır. Bu sıralama modelinde, aşağıdaki dillerde bazı özel anlamsal ve kelime tabanlı özellikler kullanırız.

|Ek sıralamacıiçeren diller|
|--|
|Çince|
|Çekçe|
|Felemenkçe|
|Türkçe|
|Fransızca|
|Almanca|
|Macarca|
|İtalyanca|
|Japonca|
|Korece|
|Lehçe|
|Portekizce|
|İspanyolca|
|İsveççe|

Bu ek sıralama QnA Maker'S ranker bir iç çalışmadır.

## <a name="verify-language"></a>Dili doğrula

QnA Maker'daki hizmet ayarları sayfasından QnA Maker kaynağınızın dilini doğrulayabilirsiniz.

![Hizmet ayarları sayfasının QnA Maker portal ekran görüntüsü](../media/language-support/language-knowledge-base.png)


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi bankasını geçirme](../Tutorials/migrate-knowledge-base.md)
