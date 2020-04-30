---
title: Sözlük-LUSıS
titleSuffix: Azure Cognitive Services
description: Sözlük, LUSıS API hizmetiyle çalışırken karşılaşabileceğiniz terimleri açıklar.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 10/25/2019
ms.author: diberry
ms.openlocfilehash: f764b0d42e08e68b45d49a5aae8542f05707ccde
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82099368"
---
# <a name="language-understanding-glossary-of-common-vocabulary-and-concepts"></a>Dil ortak Sözlük ve kavramların sözlüğünü anlama
Language Understanding (LUSıS) sözlüğü, LUSıS API hizmetiyle çalışırken karşılaşabileceğiniz terimleri açıklar.

## <a name="active-version"></a><a name="active-version"></a>Etkin sürüm

Etkin LUO sürümü modelde herhangi bir değişiklik alan sürümdür. [Luu](luis-reference-regions.md) portalında, etkin sürüm olmayan bir sürümde değişiklik yapmak istiyorsanız, önce bu sürümü etkin olarak ayarlamanız gerekir.

## <a name="authoring"></a><a name="authoring"></a>Yazma

Yazma, [LUIS](luis-reference-regions.md) portalını veya [yazma API 'Lerini](https://go.microsoft.com/fwlink/?linkid=2092087)kullanarak bir [LUIS uygulaması](#luis-app)oluşturma, yönetme ve dağıtma olanağıdır.

## <a name="authoring-key"></a><a name="authoring-key"></a>Yazma anahtarı

Daha önce "programlı" anahtar adı. Uygulamayı yazmak için kullanılır. Üretim düzeyi uç nokta sorguları için kullanılmaz. Daha fazla bilgi için bkz. [anahtar sınırları](luis-limits.md#key-limits).

## <a name="batch-test-json-file"></a><a name="batch-test-json-file"></a>Batch test JSON dosyası

Toplu iş testi, geçerli bir LUO uygulamasının modelini tutarlı ve bilinen bir kullanıcı tarafından yapılan test kümesiyle doğrulama yeteneğidir. Batch testi, [JSON biçimli bir dosyada](luis-concept-batch-test.md#batch-file-format)tanımlanır.

Ayrıca bkz:
* [Kavramlar](luis-concept-batch-test.md)
* [Nasıl yapılır](luis-how-to-batch-test.md)
* [Eğitmen](luis-tutorial-batch-testing.md)

## <a name="collaborator"></a><a name="collaborator"></a>İşbirlikçi

Ortak çalışan/katkıda bulunan, uygulamanın [sahibi](#owner) değildir, ancak hedefleri, varlıkları ve atları eklemek, düzenlemek ve silmek için aynı izinlere sahiptir.

## <a name="contributor"></a><a name="contributor"></a>Katılımcı

Katkıda bulunan, [ortak çalışan](#collaborator)ile aynı şeydir.

## <a name="descriptor"></a><a name="descriptor"></a>CI

Bir tanımlayıcı, bir modele, [tümce listeleri](#phrase-list) ve [varlıklar](#entity)dahil olmak üzere eğitim zamanında uygulanan bir [özelliktir](#features) .

## <a name="domain"></a><a name="domain"></a>Domain

LUSıS bağlamında, **etki alanı** bir bilgi alanıdır. Etki alanınız, bilgi sahibi olduğunuz uygulama alanına özgüdür. Bu, seyahat Aracısı uygulaması gibi genel bir alan olabilir. Seyahat Aracısı uygulaması, yalnızca şirketiniz için belirli coğrafi konumlar, diller ve hizmetler gibi bilgi alanlarında da özel olabilir.

## <a name="endpoint"></a><a name="endpoint"></a>Uç Nokta

Lusıs [bitiş noktası](https://go.microsoft.com/fwlink/?linkid=2092356) URL 'si, luın [uygulaması](#luis-app) YAPıLDıKTAN ve yayımlandıktan sonra lusıs sorgularını gönderdiğiniz yerdir. Uç nokta URL 'SI, yayımlanan uygulamanın yanı sıra uygulama KIMLIĞI bilgisini içerir. Uç noktasını uygulamanızın **[anahtarlar ve uç noktalar](luis-how-to-azure-subscription.md)** sayfasında bulabilir veya [uygulama bilgileri al](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c37) API 'sinden Endpoint URL 'sini alabilirsiniz.

## <a name="entity"></a><a name="entity"></a>Varlık

[Varlıklar](luis-concept-entity-types.md) , [Amaç](luis-concept-intent.md)ile ilgili [bilgileri anlatan önemli](luis-concept-utterance.md) sözcüklerdir ve bazen bunun için önemli olan öneme sahiptir. Bir varlık, LUSıS içindeki bir veri türüdür.

## <a name="f-measure"></a><a name="f-measure"></a>F ölçümü

[Batch testinde](luis-interactive-test.md#batch-testing), testin doğruluğunu ölçer.

## <a name="false-negative-fn"></a><a name="false-negative"></a>Yanlış negatif (FN)

[Toplu iş testinde](luis-interactive-test.md#batch-testing), veri noktaları, uygulamanızın hedef amaç/varlık yokluğunu yanlış tahmin ettiği noktaları temsil eder.

## <a name="false-positive-fp"></a><a name="false-positive"></a>Yanlış pozitif (FP)

[Toplu iş testinde](luis-interactive-test.md#batch-testing), veri noktaları, uygulamanızın hedef amaç/varlık varlığını yanlış tahmin ettiği noktaları temsil eder.

## <a name="features"></a><a name="features"></a>Özellikler

Machine Learning 'de, bir [özellik](luis-concept-feature.md) sistem engelinizdeki verilerin bir ayırt edici nitelik veya özniteliğidir.

## <a name="intent"></a><a name="intent"></a>Varsayılandır

[Amaç](luis-concept-intent.md) , kullanıcının gerçekleştirmek istediği bir görevi veya eylemi temsil eder. Bu, bir kullanıcının girişinde ifade edilen, bir uçuş veya bir fatura ödeme veya bir haberleri bulma gibi bir amaç veya hedeftir. LUSıS 'de, amaç tahmini tüm söylik temel alınarak hesaplanır. Varlıklar, karşılaştırmaya göre, utterance 'in parçalarından oluşur.

## <a name="labeling"></a><a name="labeling"></a>Kapatma

Etiketleme veya işaretleme, bir [varlıkla](#entity) (veri türü) bir sözcüğün veya tümceciğin bir [amaca göre ilişkilendirilmesi](#utterance) işlemidir.

## <a name="luis-app"></a><a name="luis-app"></a>LUSıS uygulaması

Bir LUSıS uygulaması, [amaçlar](#intent), [varlıklar](#entity)ve Etiketlendirme dahil olmak üzere doğal dil işleme için dil modellerinin [utterances](#utterance)bir koleksiyonudur.

## <a name="owner"></a><a name="owner"></a>Sahip

Her uygulamanın, uygulamayı oluşturan kişi olan bir sahibi vardır. Sahip, [ortak çalışanlar](#collaborator)ekleyebilir.

## <a name="patterns"></a><a name="pattern"></a>Desenler
Önceki desen özelliği [desenlerle](luis-concept-patterns.md)değiştirilmiştir. Daha az eğitim örneği sağlayarak tahmin doğruluğunu artırmak için desenler kullanın.

## <a name="phrase-list"></a><a name="phrase-list"></a>Tümcecik listesi

[Tümcecik listesi](luis-concept-feature.md) , aynı sınıfa ait olan ve benzer şekilde (örneğin, şehirlerin veya ürünlerin adları) bir grup değer (sözcükler veya ifadeler) içerir. Değiştirilebilir bir liste eş anlamlı olarak değerlendirilir.

## <a name="prebuilt-domain"></a><a name="prebuilt-domains"></a>Önceden oluşturulmuş etki alanı

[Önceden oluşturulmuş bir etki alanı](luis-how-to-use-prebuilt-domains.md) , giriş Otomasyonu (homeautomation) veya restoran ayırmaları (Restoran antreservatıon) gibi belirli bir etki alanı için YAPıLANDıRıLMıŞ bir LUL uygulamasıdır. Bu etki alanı için amaçlar, utbotslar ve varlıklar yapılandırılır.

## <a name="prebuilt-entity"></a><a name="prebuilt-entity"></a>Önceden oluşturulmuş varlık

[Önceden oluşturulmuş bir varlık](luis-prebuilt-entities.md) , sayı, URL ve e-posta gibi yaygın bilgi türleri IÇIN bir lusıs sağlar. Uygulamanıza önceden oluşturulmuş bir varlık eklemeyi tercih edersiniz.

## <a name="precision"></a><a name="precision"></a>Duyarlık
[Toplu iş testinde](luis-interactive-test.md#batch-testing)duyarlık (pozitif tahmine dayalı değer olarak da bilinir), alınan bildirimler arasında ilgili söylerin kesiri olur.

## <a name="programmatic-key"></a><a name="programmatic-key"></a>Programlı anahtar

[Yazma anahtarı](#authoring-key)olarak yeniden adlandırıldı.

## <a name="publish"></a><a name="publish"></a>Yayımlama

Yayımlama, bir LUSıS etkin sürümünü hazırlama veya üretim [uç noktasında](#endpoint)kullanılabilir hale getirmek anlamına gelir.

## <a name="quota"></a><a name="quota"></a>Kotasının

LUSıS kotası, [Azure abonelik katmanının](https://aka.ms/luis-price-tier)kısıtlamasıdır. LUIN kotası, her saniyedeki istek (HTTP durumu 429) ve bir ayda toplam istek (HTTP durumu 403) ile sınırlandırılabilir.

## <a name="recall"></a><a name="recall"></a>Çekmenin
[Toplu iş testinde](luis-interactive-test.md#batch-testing)geri çek (duyarlılık olarak da bilinir), luto Genelleştirmenin bir özelliğidir.

## <a name="semantic-dictionary"></a><a name="semantic-dictionary"></a>Anlamsal sözlük
Bir anlamsal sözlük, liste varlığı sayfasında ve tümcecik listesi sayfasından sağlanır. Anlamsal sözlük, geçerli kapsamı temel alan sözcüklerin önerilerini sağlar.

## <a name="sentiment-analysis"></a><a name="sentiment-analysis"></a>Yaklaşım Analizi
Yaklaşım analizi, [metin analizi](https://azure.microsoft.com/services/cognitive-services/text-analytics/)tarafından sağlanan sensülebilirlik değerlerinin pozitif veya negatif değerlerini sağlar.

## <a name="speech-priming"></a><a name="speech-priming"></a>Konuşma primi

Konuşma işbirliği, konuşma hizmetinizin LUSıS modelinize dahil olmasını sağlar.

## <a name="spelling-correction"></a><a name="spelling-correction"></a>Yazım denetimi düzeltmesi

Bing yazım denetleyicisini, tahminlerdeki yanlış yazılan sözcükleri tahmine göre giderecek şekilde etkinleştirin.

## <a name="starter-key"></a><a name="starter-key"></a>Başlangıç anahtarı

İlk olarak lusıs kullanarak ilk kez başlatıldığında kullanılacak olan ücretsiz bir anahtar.

## <a name="structure"></a><a name="structure"></a>Yapı

Tanımlayıcılar (Özellikler) ve kısıtlamalar (normal ifade veya liste varlıkları) ile alt bileşenler sağlamak için makineye öğrenilen bir varlığa yapı ekleyin.

## <a name="subscription-key"></a><a name="subscription-key"></a>Abonelik anahtarı

Abonelik anahtarı, [Azure 'da oluşturduğunuz](luis-how-to-azure-subscription.md)Luo hizmeti ile ilişkili **tahmin uç noktası** anahtarıdır. Bu anahtar [yazma anahtarı](#programmatic-key)değil. Bir uç nokta anahtarınız varsa, yazma anahtarı yerine herhangi bir uç nokta isteği için kullanılmalıdır. Geçerli uç nokta anahtarınızı, [lusıs](luis-reference-regions.md) Web sitesindeki [ **anahtarlar ve uç noktalar** sayfasının](luis-how-to-azure-subscription.md) alt kısmında bulunan uç nokta URL 'si içinde görebilirsiniz. Bu, **abonelik anahtar** adı/değer çiftinin değeridir.

## <a name="test"></a><a name="test"></a>Test etme

Bir lusıs uygulamasını [Test](luis-interactive-test.md#test-your-app) etmek, luya 'ya bir söylenişi ve JSON sonuçlarını görüntüleme anlamına gelir.

## <a name="timezone-offset"></a><a name="timezoneoffset"></a>Saat dilimi kayması

Uç nokta Timezonesapmayı içerir. Bu sayı, datetimeV2 önceden oluşturulmuş varlığa eklemek veya kaldırmak istediğiniz dakika cinsinden sayıdır. Örneğin, söylenişi "Şimdi ne zaman gelir?" ise, döndürülen datetimeV2 istemci isteği için geçerli süredir. İstemci isteğiniz, bot 'un kullanıcısı ile aynı olmayan bir bot veya başka bir uygulamadan geliyorsa, bot ve Kullanıcı arasındaki sapmayı geçirmeniz gerekir.

Bkz. [önceden oluşturulmuş datetimeV2 varlığının saat dilimini değiştirme](luis-concept-data-alteration.md?#change-time-zone-of-prebuilt-datetimev2-entity).

## <a name="token"></a><a name="token"></a>Simgesinde
Belirteç, bir varlıkta etiketlenebilir en küçük birimdir. Simgeleştirme, uygulamanın [kültürünü](luis-language-support.md#tokenization)temel alır.

## <a name="train"></a><a name="train"></a>Eğitim

Eğitim süreci, son eğitiminden bu yana etkin sürümde yapılan tüm değişiklikler hakkında eğitme işlemidir.

## <a name="true-negative-tn"></a><a name="true-negative"></a>True negatif (TN)

[Toplu iş testinde](luis-interactive-test.md#batch-testing), veri noktaları, uygulamanızın hedef amaç/varlık yokluğunu doğru bir şekilde tahmin ettiği noktaları temsil eder.

## <a name="true-positive-tp"></a><a name="true-positive"></a>True pozitif (TP)

[Toplu iş testinde](luis-interactive-test.md#batch-testing), veri noktaları, uygulamanızın hedef hedefin/varlığın varlığını doğru bir şekilde tahmin ettiği noktaları temsil eder.

## <a name="utterance"></a><a name="utterance"></a>İfade

Söylenişi, "yeni bir sonraki Salı için kitap 2 bileti" gibi doğal bir dil tümceciktir. Hedefe örnek olarak eklenmiştir.

## <a name="version"></a><a name="version"></a>Sürüm

Bir LUSıS [sürümü](luis-how-to-manage-versions.md) , bir lusıs uygulama kimliğiyle ve yayımlanan uç noktayla ilişkili belirli bir veri modelidir. Her LUSıS uygulamasının en az bir sürümü vardır.
