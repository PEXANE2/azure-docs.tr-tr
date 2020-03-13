---
title: Sözlük-LUSıS
titleSuffix: Azure Cognitive Services
description: Terimler sözlüğü açıklanmaktadır LUIS API'si hizmeti ile çalışırken karşılaşabileceğiniz.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 10/25/2019
ms.author: diberry
ms.openlocfilehash: 4f78b4c50f4cd65f3dc32c48cea81b705dc44de1
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79220957"
---
# <a name="language-understanding-glossary-of-common-vocabulary-and-concepts"></a>Dil ortak Sözlük ve kavramların sözlüğünü anlama
LUIS API'si hizmeti ile çalışırken, Language Understanding (LUIS), koşulları sözlüğü açıklar karşılaşabilirsiniz.

## <a name="active-version"></a>Etkin sürüm

Etkin LUIS sürüm model değişiklikleri alan sürümdür. [Luu](luis-reference-regions.md) portalında, etkin sürüm olmayan bir sürümde değişiklik yapmak istiyorsanız, önce bu sürümü etkin olarak ayarlamanız gerekir.

## <a name="authoring"></a>Özgün

Yazma, [LUIS](luis-reference-regions.md) portalını veya [yazma API 'Lerini](https://go.microsoft.com/fwlink/?linkid=2092087)kullanarak bir [LUIS uygulaması](#luis-app)oluşturma, yönetme ve dağıtma olanağıdır.

## <a name="authoring-key"></a>Yazma anahtarı

Daha önce "Programlama" anahtar adı. Uygulama yazmak için kullanılır. Üretim düzeyinde uç nokta sorgular için kullanılmaz. Daha fazla bilgi için bkz. [anahtar sınırları](luis-boundaries.md#key-limits).

## <a name="batch-test-json-file"></a>Batch test JSON dosyası

Toplu iş testi, geçerli bir LUO uygulamasının modelini tutarlı ve bilinen bir kullanıcı tarafından yapılan test kümesiyle doğrulama yeteneğidir. Batch testi, [JSON biçimli bir dosyada](luis-concept-batch-test.md#batch-file-format)tanımlanır.

Ayrıca bkz:
* [Kavramlar ](luis-concept-batch-test.md)
* [Nasıl yapılır](luis-how-to-batch-test.md)
* [Öğretici](luis-tutorial-batch-testing.md)

## <a name="collaborator"></a>Ortak çalışan

Ortak çalışan/katkıda bulunan, uygulamanın [sahibi](#owner) değildir, ancak hedefleri, varlıkları ve atları eklemek, düzenlemek ve silmek için aynı izinlere sahiptir.

## <a name="contributor"></a>Mcý

Katkıda bulunan, [ortak çalışan](#collaborator)ile aynı şeydir.

## <a name="descriptor"></a>CI

Bir tanımlayıcı, bir modele, [tümce listeleri](#phrase-list) ve [varlıklar](#entity)dahil olmak üzere eğitim zamanında uygulanan bir [özelliktir](#features) . 

## <a name="domain"></a>Alanını

LUSıS bağlamında, **etki alanı** bir bilgi alanıdır. Etki alanınız, Bilgi Bankası uygulama bölgesine özeldir. Bu, seyahat aracı uygulaması gibi genel bir alan olabilir. Bir seyahat aracı uygulaması belirli alanlara yalnızca belirli coğrafi konumlar, diller ve Hizmetleri gibi şirketinizin bilgi olabilir.

## <a name="endpoint"></a>Bkz

Lusıs [bitiş noktası](https://go.microsoft.com/fwlink/?linkid=2092356) URL 'si, luın [uygulaması](#luis-app) YAPıLDıKTAN ve yayımlandıktan sonra lusıs sorgularını gönderdiğiniz yerdir. Yayımlanmış uygulama ve bunun yanı sıra uygulama kimliği bölge uç nokta URL'sini içerir Uç noktasını uygulamanızın **[anahtarlar ve uç noktalar](luis-how-to-azure-subscription.md)** sayfasında bulabilir veya [uygulama bilgileri al](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c37) API 'sinden Endpoint URL 'sini alabilirsiniz.

## <a name="entity"></a>Varlığının

[Varlıklar](luis-concept-entity-types.md) , [Amaç](luis-concept-intent.md)ile ilgili [bilgileri anlatan önemli](luis-concept-utterance.md) sözcüklerdir ve bazen bunun için önemli olan öneme sahiptir. Veri türü LUIS, aslında bir varlıktır.

## <a name="f-measure"></a>F ölçümü

[Batch testinde](luis-interactive-test.md#batch-testing), testin doğruluğunu ölçer.

## <a name="false-negative"></a>Yanlış negatif (FN)

[Toplu iş testinde](luis-interactive-test.md#batch-testing), veri noktaları, uygulamanızın hedef amaç/varlık yokluğunu yanlış tahmin ettiği noktaları temsil eder.

## <a name="false-positive"></a>Yanlış pozitif (FP)

[Toplu iş testinde](luis-interactive-test.md#batch-testing), veri noktaları, uygulamanızın hedef amaç/varlık varlığını yanlış tahmin ettiği noktaları temsil eder.

## <a name="features"></a>Özelliklerinde

Machine Learning 'de, bir [özellik](luis-concept-feature.md) sistem engelinizdeki verilerin bir ayırt edici nitelik veya özniteliğidir.

## <a name="intent"></a>Varsayılandır

[Amaç](luis-concept-intent.md) , kullanıcının gerçekleştirmek istediği bir görevi veya eylemi temsil eder. Bir amaç veya hedefin bir uçuştaki kayıt, fatura ödeme veya haber makalesinin bulma gibi bir kullanıcının giriş, ifade olduğundan. LUIS hedefi tahmin tüm utterance üzerinde temel alır. Varlıklar, buna karşılık olarak bir utterance parçalarıdır.

## <a name="labeling"></a>Kapatma

Etiketleme veya işaretleme, bir [varlıkla](#entity) (veri türü) bir sözcüğün veya tümceciğin bir [amaca göre ilişkilendirilmesi](#utterance) işlemidir.

## <a name="luis-app"></a>LUSıS uygulaması

Bir LUSıS uygulaması, [amaçlar](#intent), [varlıklar](#entity)ve Etiketlendirme dahil olmak üzere doğal dil işleme için dil modellerinin [](#utterance)bir koleksiyonudur.

## <a name="owner"></a>İnde

Her uygulamanın, uygulamayı oluşturan kişi olan bir sahip vardır. Sahip, [ortak çalışanlar](#collaborator)ekleyebilir.

## <a name="pattern"></a>Lıklarını
Önceki desen özelliği [desenlerle](luis-concept-patterns.md)değiştirilmiştir. Desenler, daha az eğitim örnekleri sağlayarak tahmin doğruluğunu artırmak için kullanın.

## <a name="phrase-list"></a>Tümcecik listesi

[Tümcecik listesi](luis-concept-feature.md) , aynı sınıfa ait olan ve benzer şekilde (örneğin, şehirlerin veya ürünlerin adları) bir grup değer (sözcükler veya ifadeler) içerir. Değiştirilebilir bir listesi, eş anlamlı sözcükler kabul edilir.

## <a name="prebuilt-domains"></a>Önceden oluşturulmuş etki alanı

[Önceden oluşturulmuş bir etki alanı](luis-how-to-use-prebuilt-domains.md) , giriş Otomasyonu (homeautomation) veya restoran ayırmaları (Restoran antreservatıon) gibi belirli bir etki alanı için YAPıLANDıRıLMıŞ bir LUL uygulamasıdır. Hedefleri, konuşma ve varlıklar, bu etki alanı için yapılandırılır.

## <a name="prebuilt-entity"></a>Önceden oluşturulmuş varlık

[Önceden oluşturulmuş bir varlık](luis-prebuilt-entities.md) , sayı, URL ve e-posta gibi yaygın bilgi türleri IÇIN bir lusıs sağlar. Uygulamanız için önceden oluşturulmuş bir varlık eklemek seçin.

## <a name="precision"></a>Duyarlılık
[Toplu iş testinde](luis-interactive-test.md#batch-testing)duyarlık (pozitif tahmine dayalı değer olarak da bilinir), alınan bildirimler arasında ilgili söylerin kesiri olur.

## <a name="programmatic-key"></a>Programlı anahtar

[Yazma anahtarı](#authoring-key)olarak yeniden adlandırıldı.

## <a name="publish"></a>Yayınlamanız

Yayımlama, bir LUSıS etkin sürümünü hazırlama veya üretim [uç noktasında](#endpoint)kullanılabilir hale getirmek anlamına gelir.  

## <a name="quota"></a>Kotasının

LUSıS kotası, [Azure abonelik katmanının](https://aka.ms/luis-price-tier)kısıtlamasıdır. Her iki istek / saniye (HTTP durum 429) ve toplam istek (HTTP durum 403) ayda LUIS kota sınırlı olabilir.

## <a name="recall"></a>Çekmenin
[Toplu iş testinde](luis-interactive-test.md#batch-testing)geri çek (duyarlılık olarak da bilinir), luto Genelleştirmenin bir özelliğidir.

## <a name="semantic-dictionary"></a>Anlamsal sözlük
İfade listesi sayfası yanı sıra listesi varlık sayfası üzerinde bir anlam sözlük sağlanır. Anlam sözlük bir kelimelerin geçerli kapsamda önerileri sağlar.

## <a name="sentiment-analysis"></a>Yaklaşım Analizi
Yaklaşım analizi, [metin analizi](https://azure.microsoft.com/services/cognitive-services/text-analytics/)tarafından sağlanan sensülebilirlik değerlerinin pozitif veya negatif değerlerini sağlar.

## <a name="speech-priming"></a>Konuşma primi

Konuşma Hazırlama işlemi aracılığıyla LUIS modelinize primed, konuşma hizmeti sağlar.

## <a name="spelling-correction"></a>Yazım denetimi düzeltmesi

Bing yazım denetleyicisi önce tahmin uzunluğu yanlış yazılan sözcükleri düzeltmek etkinleştirin.

## <a name="starter-key"></a>Başlangıç anahtarı

İlk olarak lusıs kullanarak ilk kez başlatıldığında kullanılacak olan ücretsiz bir anahtar.

## <a name="structure"></a>Yapısı

Tanımlayıcılar (Özellikler) ve kısıtlamalar (normal ifade veya liste varlıkları) ile alt bileşenler sağlamak için makineye öğrenilen bir varlığa yapı ekleyin.

## <a name="subscription-key"></a>Abonelik anahtarı

Abonelik anahtarı, [Azure 'da oluşturduğunuz](luis-how-to-azure-subscription.md)Luo hizmeti ile ilişkili **tahmin uç noktası** anahtarıdır. Bu anahtar [yazma anahtarı](#programmatic-key)değil. Bir uç noktası anahtarı varsa, tüm uç nokta istekleri yazma anahtarı yerine kullanılmalıdır. Geçerli uç nokta anahtarınızı, [lusıs](luis-reference-regions.md) Web sitesindeki [ **anahtarlar ve uç noktalar** sayfasının](luis-how-to-azure-subscription.md) alt kısmında bulunan uç nokta URL 'si içinde görebilirsiniz. Bu, **abonelik anahtar** adı/değer çiftinin değeridir.

## <a name="test"></a>Sınamanız

Bir lusıs uygulamasını [Test](luis-interactive-test.md#test-your-app) etmek, luya 'ya bir söylenişi ve JSON sonuçlarını görüntüleme anlamına gelir.

## <a name="timezoneoffset"></a>Saat dilimi kayması

Uç nokta timezoneOffset içerir. Bu ekleyin veya datetimeV2 kaldırmak istediğiniz dakika sayısıdır önceden oluşturulmuş varlık. Utterance Örneğin, "ne zaman artık sağlıyor?", döndürülen datetimeV2 istemci isteği için geçerli zaman ise. Bir bot veya botunuzun ait kullanıcı ile aynı değil başka bir uygulama, istemci istek geliyorsa, bot ve kullanıcı uzaklığı geçmelidir.

Bkz. [önceden oluşturulmuş datetimeV2 varlığının saat dilimini değiştirme](luis-concept-data-alteration.md?#change-time-zone-of-prebuilt-datetimev2-entity).

## <a name="token"></a>Simgesinde
Bir belirteç varlık etiketli en küçük birimdir. Simgeleştirme, uygulamanın [kültürünü](luis-language-support.md#tokenization)temel alır.

## <a name="train"></a>Eğit

Eğitim süreci, son eğitiminden bu yana etkin sürümde yapılan tüm değişiklikler hakkında eğitme işlemidir.

## <a name="true-negative"></a>True negatif (TN)

[Toplu iş testinde](luis-interactive-test.md#batch-testing), veri noktaları, uygulamanızın hedef amaç/varlık yokluğunu doğru bir şekilde tahmin ettiği noktaları temsil eder.

## <a name="true-positive"></a>True pozitif (TP)

[Toplu iş testinde](luis-interactive-test.md#batch-testing), veri noktaları, uygulamanızın hedef hedefin/varlığın varlığını doğru bir şekilde tahmin ettiği noktaları temsil eder.

## <a name="utterance"></a>Söylenişi

Bir utterance, "Seattle sonraki Salı kitap 2 bilet" gibi doğal dil bir terimdir. Örnek konuşma ıntent'e eklenir.

## <a name="version"></a>Sürüm

Bir LUSıS [sürümü](luis-how-to-manage-versions.md) , bir lusıs uygulama kimliğiyle ve yayımlanan uç noktayla ilişkili belirli bir veri modelidir. Her LUIS uygulaması en az bir sürüm var.
