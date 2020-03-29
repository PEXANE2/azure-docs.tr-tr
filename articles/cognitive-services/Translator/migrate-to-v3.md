---
title: V3'e geçir - Çevirmen Metin API'si
titleSuffix: Azure Cognitive Services
description: Bu makalede, Azure Bilişsel Hizmetler Çevirmeni Metin API'sinin V2'sinden V3'e geçiş inmenize yardımcı olacak adımlar sağlanmaktadır.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: swmachan
ms.openlocfilehash: eb43d549d3e0cd449c865d533fc8701c4c3912fd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73837321"
---
# <a name="translator-text-api-v2-to-v3-migration"></a>Çevirmen Metin API V2 V3 Geçiş

> [!NOTE]
> V2 30 Nisan 2018 tarihinde amortismana kaldırıldı. Yalnızca V3'te kullanılabilen yeni işlevlerden yararlanmak için lütfen uygulamalarınızı V3'e geçirin.
> 
> Microsoft Translator Hub 17 Mayıs 2019 tarihinde kullanımdan kaldırılacaktır. [Önemli geçiş bilgilerini ve tarihlerini görüntüleyin.](https://www.microsoft.com/translator/business/hub/)  

Microsoft Translator ekibi, Translator Text API'nin Sürüm 3'ü (V3) yayımladı. Bu sürüm, Microsoft Translator Hizmeti'ne veri göndermek ve almak için yeni özellikler, amortismana alınan yöntemler ve yeni bir biçim içerir. Bu belge, V3 kullanmak için uygulamaları değiştirmek için bilgi sağlar. 

Bu belgenin sonunda daha fazla bilgi edinmek için yararlı bağlantılar içerir.

## <a name="summary-of-features"></a>Özelliklerin özeti

* Hiçbir İzleme Yok - V3 No-Trace Azure portalındaki tüm fiyatlandırma katmanları için geçerlidir. Bu özellik, V3 API'sine gönderilen hiçbir metnin Microsoft tarafından kaydedilmeyeceğini zedilen anlamına gelir.
* JSON - XML JSON ile değiştirilir. Hizmete gönderilen ve hizmetten alınan tüm veriler JSON formatındadır.
* Tek bir istekte birden çok hedef dil - Çeviri yöntemi, tek bir istekte çeviri için birden çok 'to' dili kabul eder. Örneğin, tek bir istek İngilizce'den ve Almanca, İspanyolca ve Japonca'ya veya başka bir dil grubuna 'dan' olabilir.
* İki dilli sözlük - API'ye iki dilli sözlük yöntemi eklendi. Bu yöntem 'arama' ve 'örnekler' içerir.
* Transliterate - API'ye bir transliterate yöntemi eklenmiştir. Bu yöntem, sözcükleri ve tümceleri tek bir komut dosyasında dönüştürür (ÖRN. Arapça) başka bir komut dosyasına (Örn. Latince).
* Diller - Yeni bir 'diller' yöntemi, JSON formatında, 'çeviri', 'sözlük' ve 'transliterate' yöntemleri ile kullanılmak üzere dil bilgilerini sunar.
* Yeni Çeviriye - V2 API'de bulunan bazı özellikleri ayrı yöntemlerle desteklemek için 'çevirme' yöntemine yeni özellikler eklendi. Bir örnek TranslateArray olduğunu.
* Konuşma yöntemi - Metinden konuşmaya işlevselliği artık Microsoft Translator API'sinde desteklenmemektedir. Metinden konuşmaya işlevselliği [Microsoft Konuşma Hizmeti'nde](https://docs.microsoft.com/azure/cognitive-services/speech-service/text-to-speech)kullanılabilir.

V2 ve V3 yöntemlerinin aşağıdaki listesi V2 ile birlikte gelen işlevselliği sağlayacak V3 yöntemlerini ve API'leri tanımlar.

| V2 API Yöntemi   | V3 API Uyumluluğu |
|:----------- |:-------------|
| `Translate`     | [Çevirme](reference/v3-0-translate.md)          |
| `TranslateArray`      | [Çevirme](reference/v3-0-translate.md)        |
| `GetLanguageNames`      | [Diller](reference/v3-0-languages.md)         |
| `GetLanguagesForTranslate`     | [Diller](reference/v3-0-languages.md)       |
| `GetLanguagesForSpeak`      | [Microsoft Konuşma Hizmeti](https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support#text-to-speech)         |
| `Speak`     | [Microsoft Konuşma Hizmeti](https://docs.microsoft.com/azure/cognitive-services/speech-service/text-to-speech)          |
| `Detect`     | [Algılama](reference/v3-0-detect.md)         |
| `DetectArray`     | [Algılama](reference/v3-0-detect.md)         |
| `AddTranslation`     | Özellik artık desteklenmiş       |
| `AddTranslationArray`    | Özellik artık desteklenmiş          |
| `BreakSentences`      | [BreakSentence](reference/v3-0-break-sentence.md)       |
| `GetTranslations`      | Özellik artık desteklenmiş         |
| `GetTranslationsArray`      | Özellik artık desteklenmiş         |

## <a name="move-to-json-format"></a>JSON biçimine taşıyın

Microsoft Translator Text Translation V2, XML formatında verileri kabul etti ve döndürdü. V3'te API kullanılarak gönderilen ve alınan tüm veriler JSON formatındadır. XML artık V3'te kabul edilmeyecek veya iade edilmeyecek.

Bu değişiklik, V2 Metin Çevirisi API'si için yazılmış bir uygulamanın çeşitli yönlerini etkileyecektir. Örnek olarak: Diller API metin çevirisi, çeviri ve iki sözlük yöntemleri için dil bilgilerini döndürür. Tüm yöntemler için tüm dil bilgilerini tek bir aramada isteyebilir veya tek tek isteyebilirsiniz.

Diller yöntemi kimlik doğrulaması gerektirmez; Aşağıdaki linke tıklayarak JSON V3 için tüm dil bilgilerini görebilirsiniz:

[https://api.cognitive.microsofttranslator.com/languages?api-version=3.0&scope=translation,sözlük,çeviri](https://api.cognitive.microsofttranslator.com/languages?api-version=3.0&scope=translation,dictionary,transliteration)

## <a name="authentication-key"></a>Kimlik doğrulama anahtarı

V2 için kullandığınız kimlik doğrulama anahtarı V3 için kabul edilecektir. Yeni bir abonelik almanız gerekmez. Bir yıllık geçiş döneminde uygulamalarınızda V2 ve V3'ü karıştırarak, V2-XML'den V3-JSON'a geçiş yaparken yeni sürümler yayınlamanızı kolaylaştırabileceksiniz.

## <a name="pricing-model"></a>Fiyatlandırma Modeli

Microsoft Translator V3, V2'nin fiyatlandırıldığu şekilde fiyatlandırılır; boşluklar da dahil olmak üzere karakter başına. V3'teki yeni özellikler, hangi karakterlerin faturalandırma için sayıldığında bazı değişiklikler yapar.

| V3 Yöntemi   | Faturalandırılmaya Sayılan Karakterler |
|:----------- |:-------------|
| `Languages`     | Hiçbir karakter gönderilmedi, hiçbiri sayılmadı, ücret yok.          |
| `Translate`     | Sayım, çeviri için kaç karakter gönderildiğine ve karakterlerin kaç dile çevrildiği temel alınarak yapılır. Gönderilen 50 karakter ve istenen 5 dilde 50x5 olacaktır.           |
| `Transliterate`     | Transliterasyon için gönderilen karakter sayısı sayılır.         |
| `Dictionary lookup & example`     | Sözlük araması için gönderilen karakter sayısı ve örnekler sayılır.         |
| `BreakSentence`     | Ücret yok.       |
| `Detect`     | Ücret yok.      |

## <a name="v3-end-points"></a>V3 Bitiş Noktaları

Genel

* api.cognitive.microsofttranslator.com

## <a name="v3-api-text-translations-methods"></a>V3 API metin çeviriyöntemleri

[`Languages`](reference/v3-0-languages.md)

[`Translate`](reference/v3-0-translate.md)

[`Transliterate`](reference/v3-0-transliterate.md)

[`BreakSentence`](reference/v3-0-break-sentence.md)

[`Detect`](reference/v3-0-detect.md)

[`Dictionary/lookup`](reference/v3-0-dictionary-lookup.md)

[`Dictionary/example`](reference/v3-0-dictionary-examples.md)

## <a name="compatibility-and-customization"></a>Uyumluluk ve özelleştirme

> [!NOTE]
> 
> Microsoft Translator Hub 17 Mayıs 2019 tarihinde kullanımdan kaldırılacaktır. [Önemli geçiş bilgilerini ve tarihlerini görüntüleyin.](https://www.microsoft.com/translator/business/hub/)   

Microsoft Translator V3 varsayılan olarak sinir seli makinesi çevirisi kullanır. Bu nedenle, Microsoft Translator Hub ile kullanılamaz. Translator Hub yalnızca eski istatistiksel makine çevirisini destekler. Nöral çeviri için özelleştirme artık Özel Çevirmen kullanılarak kullanılabilir. [Sinir seli çevirilerini özelleştirme hakkında daha fazla bilgi edinin](custom-translator/overview.md)

V3 metin API ile nöral çeviri standart kategorilerin kullanımını desteklemez (SMT, konuşma, teknoloji, generalnn).

| |Uç Nokta|    GDPR İşlemci Uyumluluğu|  Çevirmen Hub'ı Kullanma| Özel Çevirmen Kullanın (Önizleme)|
|:-----|:-----|:-----|:-----|:-----|
|Çevirmen Metin API Sürüm 2| api.microsofttranslator.com|    Hayır  |Evet    |Hayır|
|Çevirmen Metin API Sürüm 3| api.cognitive.microsofttranslator.com|  Evet|    Hayır| Evet|

**Çevirmen Metin API Sürüm 3**
* Genel olarak kullanılabilir ve tam olarak desteklenir.
* GDPR işlemci olarak uyumludur ve tüm ISO 20001 ve 20018 belgelendirme gereksinimlerini ve SOC 3 belgelendirme gerekliliklerini karşılar. 
* Yeni Translator NMT özelleştirme özelliği olan Custom Translator (Preview) ile özelleştirdiğiniz nöral ağ çeviri sistemlerini çağırmanızı sağlar. 
* Microsoft Translator Hub kullanılarak oluşturulan özel çeviri sistemlerine erişim sağlamaz.

Api.cognitive.microsofttranslator.com bitiş noktasını kullanıyorsanız, Çevirmen Metin API'sinin Sürüm 3'ü kullanıyorsunuz.

**Çevirmen Metin API Sürüm 2**
* Tüm ISO 20001,20018 ve SOC 3 belgelendirme gerekliliklerini karşılamaz. 
* Translator özelleştirme özelliği yle özelleştirdiğiniz nöral ağ çeviri sistemlerini çağırmanıza izin vermez.
* Microsoft Translator Hub kullanılarak oluşturulan özel çeviri sistemlerine erişim sağlar.
* Çevirmen Metin API'sinin Sürüm 2'sini kullanıyorsunuz, api.microsofttranslator.com bitiş noktasını kullanıyorsanız.

Translator API'nin hiçbir sürümü çevirilerinizin kaydını oluşturmaz. Çevirileriniz hiçbir zaman kimseyle paylaşılmaz. [Çevirmen No-Trace](https://www.aka.ms/NoTrace) web sayfası hakkında daha fazla bilgi.

## <a name="links"></a>Bağlantılar

* [Microsoft Gizlilik İlkesi](https://privacy.microsoft.com/privacystatement)
* [Microsoft Azure Yasal Bilgiler](https://azure.microsoft.com/support/legal)
* [Online Hizmet Koşulları](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [V3.0 Belgelerini Görüntüle](reference/v3-0-reference.md)
