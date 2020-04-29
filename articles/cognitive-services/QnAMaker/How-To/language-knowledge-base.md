---
title: Dil kavramları-Soru-Cevap Oluşturma
titleSuffix: Azure Cognitive Services
description: Soru-Cevap Oluşturma birçok dilde Bilgi Bankası içeriğini destekler. Ancak, her bir Soru-Cevap Oluşturma Hizmeti tek bir dil için ayrılmalıdır. Oluşturulan ilk bilgi tabanı, belirli bir Soru-Cevap Oluşturma hizmetini hedefleyerek bu hizmetin dilini ayarlar.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: diberry
ms.openlocfilehash: 38701e8bbef1c5d78eca2242105e81fe7261c0f6
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "79220638"
---
# <a name="language-support-of-knowledge-base-content-for-qna-maker"></a>Soru-Cevap Oluşturma için Bilgi Bankası içeriğinin dil desteği

Kaynak için ilk Bilgi Bankası oluşturduğunuzda hizmetin dili seçilir. Kaynaktaki tüm ek bilgi tabanları aynı dilde olmalıdır.

Dil, Kullanıcı sorgularına yanıt olarak Soru-Cevap Oluşturma sonuçların uygunluğunu belirler.

## <a name="one-language-for-all-knowledge-bases-in-resource"></a>Kaynaktaki tüm bilgi tabanları için bir dil

Soru-Cevap Oluşturma, birinci Bilgi Bankası oluşturma sırasında QnA hizmetinizin dilini seçmenizi sağlar. Bir Soru-Cevap Oluşturma kaynaktaki tüm bilgi tabanları için, hepsi aynı dilde olmalıdır. Bu dil değiştirilemez.

Tek bir kaynakta farklı dillerde bilgi tabanları oluşturmak, Kullanıcı sorgularına yanıt olarak Soru-Cevap Oluşturma sonuçların uygunluğunu olumsuz şekilde etkiler.

[Desteklenen dillerin](../overview/language-support.md#languages-supported) listesini ve dillerin [eşleştirmeyi ve uygunluğu](#query-matching-and-relevance)nasıl etkilediğini gözden geçirin.

## <a name="select-language-when-creating-first-knowledge-base"></a>İlk Bilgi Bankası oluşturulurken dili seçin

Dil seçimi, bir kaynakta ilk bilgi tabanının oluşturulması için adımların bir parçasıdır.

![İlk bilgi tabanı için dil seçme 'nin Soru-Cevap Oluşturma portalı ekran görüntüsü](../media/language-support/select-language-when-creating-knowledge-base.png)

## <a name="query-matching-and-relevance"></a>Sorgu eşleştirme ve ilgisi
Soru-Cevap Oluşturma, sonuçları sağlamak için [Azure bilişsel arama dil Çözümleyicileri](https://docs.microsoft.com/rest/api/searchservice/language-support) 'ne bağlıdır.

Azure bilişsel arama özellikleri desteklenen diller için eşit olsa da, soru-cevap oluşturma Azure Arama sonuçlarının üzerinde yer alan ek bir derecelendiricisini vardır. Bu derecelendiricisini modelinde, aşağıdaki dillerde bazı özel semantik ve Word tabanlı özellikleri kullanırız.

|Ek derecelendiricisini ile diller|
|--|
|Çince|
|Çekçe|
|Felemenkçe|
|İngilizce|
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

Bu ek derecelendirme, Soru-Cevap Oluşturma Ranker 'ın iç çalışmadır.

## <a name="verify-language"></a>Dili doğrula

Soru-Cevap Oluşturma kaynağınızın dilini Soru-Cevap Oluşturma hizmet ayarları sayfasından doğrulayabilirsiniz.

![Hizmet Ayarları sayfasının Soru-Cevap Oluşturma portalı ekran görüntüsü](../media/language-support/language-knowledge-base.png)


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi bankasını geçirme](../Tutorials/migrate-knowledge-base.md)
