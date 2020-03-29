---
title: Sözlük - LUIS
titleSuffix: Azure Cognitive Services
description: Sözlük, LUIS API Hizmeti ile çalışırken karşılaşabileceğiniz terimleri açıklar.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 10/25/2019
ms.author: diberry
ms.openlocfilehash: 4f78b4c50f4cd65f3dc32c48cea81b705dc44de1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79220957"
---
# <a name="language-understanding-glossary-of-common-vocabulary-and-concepts"></a>Ortak kelime ve kavramlar dil anlama sözlüğü
Dil Bilgisi (LUIS) sözlüğü, LUIS API Hizmeti ile çalışırken karşılaşabileceğiniz terimleri açıklar.

## <a name="active-version"></a><a name="active-version"></a>Etkin sürüm

Etkin LUIS sürümü, modelde değişiklik alan sürümdür. [LUIS](luis-reference-regions.md) portalında, etkin olmayan bir sürümde değişiklik yapmak istiyorsanız, önce bu sürümü etkin olarak ayarlamanız gerekir.

## <a name="authoring"></a><a name="authoring"></a>Yazma

Yazarlık, [LUIS](luis-reference-regions.md) portalını veya [yazaraplarını](https://go.microsoft.com/fwlink/?linkid=2092087)kullanarak bir [LUIS uygulaması](#luis-app)oluşturma, yönetme ve dağıtma yeteneğidir.

## <a name="authoring-key"></a><a name="authoring-key"></a>Anahtar Yazma

Daha önce "Programlı" anahtar olarak adlandırılır. Uygulamayı yazarlık için kullanılır. Üretim düzeyinde uç nokta sorguları için kullanılmaz. Daha fazla bilgi için [Anahtar sınırlarına](luis-boundaries.md#key-limits)bakın.

## <a name="batch-test-json-file"></a><a name="batch-test-json-file"></a>Toplu test JSON dosyası

Toplu işlem, geçerli bir LUIS uygulamasının modelini tutarlı ve bilinen bir test grubuyla doğrulayabilme yeteneğidir. Toplu işlem, [JSON biçimlendirilmiş](luis-concept-batch-test.md#batch-file-format)bir dosyada tanımlanır.

Ayrıca bkz:
* [Kavramlar](luis-concept-batch-test.md)
* [Nasıl yapılır](luis-how-to-batch-test.md)
* [Öğretici](luis-tutorial-batch-testing.md)

## <a name="collaborator"></a><a name="collaborator"></a>İşbirlikçi

Ortak çalışan/katkıda bulunan, uygulamanın [sahibi](#owner) değildir, ancak amaçları, varlıkları, söyleyiyi eklemek, düzenlemek ve silmek için aynı izinlere sahiptir.

## <a name="contributor"></a><a name="contributor"></a>Katılımcı

Katkıda bulunan kişi, [ortak çalışanla](#collaborator)aynı şeydir.

## <a name="descriptor"></a><a name="descriptor"></a>Tanımlayıcısı

Tanımlayıcı, [ifade listeleri](#phrase-list) ve varlıklar da dahil olmak üzere, eğitim zamanında bir modele uygulanan bir [özelliktir.](#entity) [feature](#features) 

## <a name="domain"></a><a name="domain"></a>Domain

LUIS bağlamında, etki **alanı** bilgi alanıdır. Etki alanınız uygulama bilgi alanınızın bilgisine özgüdür. Bu seyahat acentesi uygulaması gibi genel bir alan olabilir. Bir seyahat acentesi uygulaması, şirketinizin belirli coğrafi konumları, dilleri ve hizmetleri gibi yalnızca bilgi alanlarına özel olabilir.

## <a name="endpoint"></a><a name="endpoint"></a>Uç Nokta

[LUIS bitiş noktası](https://go.microsoft.com/fwlink/?linkid=2092356) URL'si, LUIS [uygulaması](#luis-app) yazarve yayınlandıktan sonra LUIS sorgularını gönderdiğiniz yerdir. Bitiş noktası URL'si, yayınlanan uygulamanın bölgesini ve uygulama kimliğini içerir. Uygulamanızın **[Keys ve uç noktaları](luis-how-to-azure-subscription.md)** sayfasında bitiş noktasını bulabilir veya Uygulama Bilgileri API'sini [Al'dan](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c37) bitiş noktası URL'sini alabilirsiniz.

## <a name="entity"></a><a name="entity"></a>Varlık

[Varlıklar,](luis-concept-entity-types.md) [niyetle](luis-concept-intent.md)ilgili bilgileri açıklayan ve bazen de bunun için gerekli olan [sözlerdeki](luis-concept-utterance.md) önemli sözcüklerdir. Bir varlık aslında LUIS bir veri türüdür.

## <a name="f-measure"></a><a name="f-measure"></a>F-ölçüsü

[Toplu testte,](luis-interactive-test.md#batch-testing)testin doğruluğunun bir ölçüsüdür.

## <a name="false-negative-fn"></a><a name="false-negative"></a>Yanlış negatif (FN)

[Toplu işlemde,](luis-interactive-test.md#batch-testing)veri noktaları uygulamanızın hedef amaç/varlığın yokluğunu yanlış tahmin ettiği sözcükleri temsil eder.

## <a name="false-positive-fp"></a><a name="false-positive"></a>Yanlış pozitif (FP)

[Toplu işlemde,](luis-interactive-test.md#batch-testing)veri noktaları uygulamanızın hedef niyetin/varlığın varlığını yanlış tahmin ettiği sözcükleri temsil eder.

## <a name="features"></a><a name="features"></a>Özellik

Makine öğreniminde bir [özellik,](luis-concept-feature.md) sisteminizin gözlemlediği ayırt edici bir özellik veya veri niteliğidir.

## <a name="intent"></a><a name="intent"></a>Niyet

[Bir niyet,](luis-concept-intent.md) kullanıcının gerçekleştirmek istediği bir görevi veya eylemi temsil eder. Uçuş rezervasyonu, fatura ödeme veya haber makalesi bulma gibi kullanıcının girişinde ifade edilen bir amaç veya amaçtır. LUIS'te, niyet tahmini tüm söyleyiş üzerine kuruludur. Varlıklar, karşılaştırma, bir söyleyiş parçalarıdır.

## <a name="labeling"></a><a name="labeling"></a>Etiketleme

Etiketleme veya işaretleme, bir sözcüğü veya tümceciği bir [varlığın](#entity) (veri türü) bir [niyetin söyleyişiyle](#utterance) ilişkilendirme işlemidir.

## <a name="luis-app"></a><a name="luis-app"></a>LUIS uygulaması

BIR LUIS [uygulaması, doğal](#intent)dil işleme amaçlı dil modelleri topluluğudur, [tüzel kişilikler](#entity), ve etiketli [söyleyerek](#utterance).

## <a name="owner"></a><a name="owner"></a>Sahibi

Her uygulamanın, uygulamayı oluşturan kişi olan bir sahibi vardır. Sahibi ortak [çalışanlar](#collaborator)ekleyebilirsiniz.

## <a name="patterns"></a><a name="pattern"></a>Desenler
Önceki Desen özelliği [Desenler](luis-concept-patterns.md)ile değiştirilir. Daha az eğitim örneği sağlayarak tahmin doğruluğunu artırmak için desenleri kullanın.

## <a name="phrase-list"></a><a name="phrase-list"></a>Tümcecik listesi

[Tümcecik listesi,](luis-concept-feature.md) aynı sınıfa ait olan ve benzer şekilde ele alınması gereken (örneğin, şehirlerin veya ürünlerin adları) bir değer grubu (sözcük veya tümcecikler) içerir. Değiştirilebilir bir liste eşanlamlı olarak kabul edilir.

## <a name="prebuilt-domain"></a><a name="prebuilt-domains"></a>Önceden oluşturulmuş etki alanı

[Önceden oluşturulmuş bir etki alanı,](luis-how-to-use-prebuilt-domains.md) ev otomasyonu (HomeAutomation) veya restoran rezervasyonları (RestaurantReservation) gibi belirli bir etki alanı için yapılandırılan bir LUIS uygulamasıdır. Niyetler, söyleyinmeler ve varlıklar bu etki alanı için yapılandırılır.

## <a name="prebuilt-entity"></a><a name="prebuilt-entity"></a>Önceden oluşturulmuş varlık

Önceden oluşturulmuş bir [varlık,](luis-prebuilt-entities.md) LUIS'in sayı, URL ve e-posta gibi yaygın bilgi türlerini sağladığı bir varlıktır. Uygulamanıza önceden oluşturulmuş bir varlık eklemeyi tercih esiniz.

## <a name="precision"></a><a name="precision"></a>Duyarlık
[Toplu testte,](luis-interactive-test.md#batch-testing)kesinlik (pozitif tahmin değeri olarak da adlandırılır) alınan söyleyişler arasında ilgili söyleyişlerin bir kısmıdır.

## <a name="programmatic-key"></a><a name="programmatic-key"></a>Programlı anahtar

Anahtar [yazma](#authoring-key)olarak yeniden adlandırıldı.

## <a name="publish"></a><a name="publish"></a>Yayımlamak

Yayımlama, luis etkin sürümünü evreleme veya üretim [bitiş noktasında](#endpoint)kullanılabilir hale getirmek anlamına gelir.  

## <a name="quota"></a><a name="quota"></a>Kota

LUIS kotası Azure [abonelik katmanının](https://aka.ms/luis-price-tier)sınırlamasıdır. LUIS kotası hem saniyedeki istekler (HTTP Status 429) hem de bir ay daki toplam istekle sınırlandırılabilir (HTTP Status 403).

## <a name="recall"></a><a name="recall"></a>Hatırla
[Toplu işlemde,](luis-interactive-test.md#batch-testing)geri çağırma (duyarlılık olarak da bilinir), LUIS'in genelleme yeteneğidir.

## <a name="semantic-dictionary"></a><a name="semantic-dictionary"></a>Anlamsal sözlük
Sözcük listesi sayfasının yanı sıra Liste varlık sayfasında da anlamsal bir sözlük sağlanır. Anlamsal sözlük, geçerli kapsama dayalı sözcüklerin önerilerini sağlar.

## <a name="sentiment-analysis"></a><a name="sentiment-analysis"></a>Duygusallık Analizi
Duyarlılık [analizi, Text Analytics](https://azure.microsoft.com/services/cognitive-services/text-analytics/)tarafından sağlanan söyleyişlerin olumlu veya olumsuz değerlerini sağlar.

## <a name="speech-priming"></a><a name="speech-priming"></a>Konuşma astarı

Konuşma astarı, konuşma hizmetinizin LUIS modeliyle birlikte hazır olmasını sağlar.

## <a name="spelling-correction"></a><a name="spelling-correction"></a>Yazım düzeltmesi

Bing yazım denetleyicisinin tahminden önce sözcüklerdeki yanlış yazılmış sözcükleri düzeltmesini etkinleştirin.

## <a name="starter-key"></a><a name="starter-key"></a>Başlangıç tuşu

LUIS'i kullanmaya ilk başladığınızda kullanabileceğiniz ücretsiz bir anahtar.

## <a name="structure"></a><a name="structure"></a>Yapı

Alt bileşenlere tanımlayıcılar (özellikler) ve kısıtlamalar (normal ifade veya liste varlıkları) sağlamak için makineden öğrenilen bir varlığa yapı ekleyin.

## <a name="subscription-key"></a><a name="subscription-key"></a>Abonelik anahtarı

Abonelik anahtarı, [Azure'da oluşturduğunuz](luis-how-to-azure-subscription.md)LUIS hizmetiyle ilişkili **tahmin bitiş noktası** anahtarıdır. Bu anahtar [yazma anahtarı](#programmatic-key)değildir. Bir bitiş noktası anahtarınız varsa, yazarlık anahtarı yerine tüm uç nokta istekleri için kullanılmalıdır. Geçerli uç nokta [anahtarınızı, LUIS](luis-reference-regions.md) web sitesinde [ **KiKeys ve uç noktalar** sayfasının](luis-how-to-azure-subscription.md) alt kısmında ki bitiş noktası URL'sinde görebilirsiniz. **Abonelik anahtarı** adı/değer çiftinin değeridir.

## <a name="test"></a><a name="test"></a>Test

LUIS uygulamasını [test](luis-interactive-test.md#test-your-app) etmek, LUIS'e bir söz söylemek ve JSON sonuçlarını görüntülemek anlamına gelir.

## <a name="timezone-offset"></a><a name="timezoneoffset"></a>Saat dilimi mahsup

Bitiş noktası saat dilimiOft içerir. Bu, datetimeV2 önceden oluşturulmuş varlık eklemek veya kaldırmak istediğiniz dakika sayısıdır. Örneğin, söyleyiş "şimdi saat nedir?", döndürülen datetimeV2 istemci isteği için geçerli saattir. İstemci isteğiniz, botunuzun kullanıcısıyla aynı olmayan bir botveya başka bir uygulamadan geliyorsa, bot ile kullanıcı arasındaki ofsette geçiş yapmalısınız.

Bkz. [Önceden oluşturulmuş datetimeV2 varlığının saat dilimini değiştir.](luis-concept-data-alteration.md?#change-time-zone-of-prebuilt-datetimev2-entity)

## <a name="token"></a><a name="token"></a>Belirte -ci
Belirteç, bir varlıkta etiketlenebilecek en küçük birimdir. Belirteçleştirme, uygulamanın [kültürüne](luis-language-support.md#tokenization)dayanır.

## <a name="train"></a><a name="train"></a>Eğitim

Eğitim, LUIS'e son eğitimden bu yana aktif sürümde yapılan değişiklikler hakkında eğitim verme sürecidir.

## <a name="true-negative-tn"></a><a name="true-negative"></a>Gerçek negatif (TN)

[Toplu işlemde,](luis-interactive-test.md#batch-testing)veri noktaları uygulamanızın hedef hedef amaç/varlığın yokluğunu doğru şekilde tahmin ettiği sözcükleri temsil eder.

## <a name="true-positive-tp"></a><a name="true-positive"></a>Gerçek pozitif (TP)

[Toplu işlemde,](luis-interactive-test.md#batch-testing)veri noktaları uygulamanızın hedef niyetin/varlığın varlığını doğru şekilde tahmin ettiği sözcükleri temsil eder.

## <a name="utterance"></a><a name="utterance"></a>İfade

Bir söyleyiş gibi doğal bir dil ifade "kitap 2 Seattle önümüzdeki Salı bilet". Örnek ifadeler niyete eklenir.

## <a name="version"></a><a name="version"></a>Sürüm

LUIS [sürümü,](luis-how-to-manage-versions.md) LUIS uygulama kimliği ve yayınlanan bitiş noktasıyla ilişkili belirli bir veri modelidir. Her LUIS uygulamasının en az bir sürümü vardır.
