---
title: Dil desteği-Soru-Cevap Oluşturma
titleSuffix: Azure Cognitive Services
description: Bilgi tabanınız için Soru-Cevap Oluşturma tarafından desteklenen bir kültürün, doğal dillerin bir listesi. Aynı bilgi bankasındaki dilleri karıştırmayın.
services: cognitive-services
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: reference
ms.date: 09/24/2019
ms.openlocfilehash: c990b6980dea871679b0b301e293e4fb94748db7
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89650898"
---
# <a name="language-support-for-a-qna-maker-resource-and-knowledge-bases"></a>Soru-Cevap Oluşturma bir kaynak ve bilgi tabanı için dil desteği

Kaynak için ilk Bilgi Bankası oluşturduğunuzda hizmetin dili seçilir. Kaynaktaki tüm ek bilgi tabanları aynı dilde olmalıdır. 

Dil, Kullanıcı sorgularına yanıt olarak Soru-Cevap Oluşturma sonuçların uygunluğunu belirler. Soru-Cevap Oluşturma kaynağı ve bu kaynaktaki tüm bilgi tabanları, tek bir dili destekler. Tek dil, bir sorgunun en iyi yanıt sonuçlarını sağlamak için gereklidir.

## <a name="single-language-per-resource"></a>Kaynak başına tek dil

Aşağıdaki topluluklara bir göz atın:

* Bir Soru-Cevap Oluşturma Hizmeti ve tüm bilgi tabanları, yalnızca bir dili destekler.
* Hizmetin ilk bilgi tabanı oluşturulduğunda dil açıkça ayarlanır
* Dil, Bilgi Bankası oluşturulduğunda eklenen dosyalardan ve URL 'lerden belirlenir
* Hizmet, hizmette diğer bilgi tabanları için değiştirilemez
* Dil, bir sorguya en iyi yanıtı oluşturmak için Bilişsel Arama hizmeti (Ranker #1) ve Soru-Cevap Oluşturma Hizmeti (Ranker #2) tarafından kullanılır

## <a name="supporting-multiple-languages"></a>Birden çok dili destekleme

Birkaç dil içeren bir Bilgi Bankası sistemini desteklemeniz gerekiyorsa şunları yapabilirsiniz:

* Soruyu bilgi tabanınızı göndermeden önce bir soruyu tek bir dile çevirmek için [Translator hizmetini](../../translator/translator-info-overview.md) kullanın. Bu, tek bir dilin kalitesine ve diğer soruların ve yanıtların kalitesine odaklanabilmenizi sağlar.
* Her dil için bu kaynak içindeki bir Soru-Cevap Oluşturma kaynağı ve Bilgi Bankası oluşturun. Bu, ayrı alternatif soruları yönetmenize ve her bir dil için daha fazla nuılmış metin elde etmenizi sağlar. Bu, çok daha fazla esneklik sağlar ancak sorular veya yanıtlar tüm dillerde değiştiğinde çok daha yüksek bir bakım maliyeti gerektirir.

Soru-Cevap Oluşturma için [desteklenen dilleri](../overview/language-support.md) gözden geçirin.

### <a name="support-each-language-with-a-qna-maker-resource"></a>Soru-Cevap Oluşturma kaynağıyla her dili destekleme

* Her dil için bir Soru-Cevap Oluşturma kaynağı oluşturma
* Yalnızca bu dilin dosyalarını ve URL 'Lerini ekleyin
* Dili tanımlamak için kaynak için bir adlandırma kuralı kullanın. `qna-maker-fr`Fransızca belgelerin tüm Bilgi Bankası bilgileri için örnek bir örnektir


## <a name="languages-supported"></a>Desteklenen diller

Aşağıdaki liste, bir Soru-Cevap Oluşturma kaynağı için desteklenen dilleri içerir. 

|Dil|
|--|
|Arapça|
|Ermenice|
|Bangla|
|Baskça|
|Bulgarca|
|Katalanca|
|Chinese_Simplified|
|Chinese_Traditional|
|Hırvatça|
|Çekçe|
|Danca|
|Felemenkçe|
|İngilizce|
|Estonya Dili|
|Fince|
|Fransızca|
|Galiçya Dili|
|Almanca|
|Yunanca|
|Gucerat dili|
|İbranice|
|Hintçe|
|Macarca|
|İzlandaca|
|Endonezce|
|İrlandaca|
|İtalyanca|
|Japonca|
|Kannada dili|
|Korece|
|Letonca|
|Litvanca|
|Malayalam dili|
|Malayca|
|Norveççe|
|Lehçe|
|Portekizce|
|Pencap dili|
|Rumence|
|Rusça|
|Serbian_Cyrillic|
|Serbian_Latin|
|Slovakça|
|Slovence|
|İspanyolca|
|İsveççe|
|Tamil dili|
|Telugu dili|
|Tayca|
|Türkçe|
|Ukraynaca|
|Urduca|
|Vietnamca|

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

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dil seçimi](../how-to/language-knowledge-base.md)
