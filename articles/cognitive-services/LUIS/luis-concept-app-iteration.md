---
title: Yinelemeli uygulama tasarımı - LUIS
titleSuffix: Azure Cognitive Services
description: LUIS, model değişiklikleri, sözcük örnekleri, yayımlama ve bitiş noktası sorgularından veri toplama yinelemeli bir döngüde en iyi yitirmeyle öğrenir.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/20/2019
ms.author: diberry
ms.openlocfilehash: c1c1b2df301634a435b610c395a1a58aa5573da3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74422595"
---
# <a name="iterative-app-design-for-luis"></a>LUIS için yinelemeli uygulama tasarımı

Bir Dil Anlama (LUIS) uygulaması öğrenir ve yineleme ile en verimli şekilde gerçekleştirir. Tipik bir yineleme döngüsü aşağıda veda edebilirsiniz:

* Yeni sürüm oluşturma
* LUIS uygulama şemasını edin. Buna aşağıdakiler dahildir:
    * Örnek sözlerle niyet
    * Varlıklar
    * Özellikler
* Tren, test ve yayımlama
    * Aktif öğrenme için tahmin bitiş noktasında test
* Uç nokta sorgularından veri toplama

![Yazma döngüsü](./media/luis-concept-app-iteration/iteration.png)

## <a name="building-a-luis-schema"></a>Luis şema oluşturma

Bir uygulamanın şeması, kullanıcının ne istediğini _(niyet_ veya _niyet)_ ve niyetin hangi bölümlerinin yanıtı belirlemeye yardımcı olmak için kullanılan ayrıntıları _(varlıklar_olarak adlandırılır) sağladığını tanımlar. 

Uygulama şeması, ilgili sözcük ve tümcecikleri belirlemek ve tipik sözcük sıralamasını belirlemek için uygulama etki adlarına özgü olmalıdır. 

Örnek konuşmalar, uygulamanın çalışma zamanında beklediği tanınan konuşma veya metin gibi kullanıcı girişlerini temsil eder. 

Şema niyet gerektirir ve varlıklar _olmalıdır._ 

### <a name="example-schema-of-intents"></a>Niyet örneği şeması

En yaygın şema, niyetle düzenlenmiş bir entrikadır. Bu tür şema, kullanıcının niyetini belirlemek için LUIS'i kullanır. 

LUIS kullanıcının niyetini belirlemesine yardımcı olacaksa, niyet şema türünde varlıklar olabilir. Örneğin, bir sevkiyat varlığı (bir niyetin tanımlayıcısı olarak) LUIS'in bir sevkiyat niyetini belirlemesine yardımcı olur. 

### <a name="example-schema-of-entities"></a>Varlıkların örnek şeması

Bir varlık şeması, kullanıcı sözcüklerinden elde edilen veriler olan varlıklara odaklanır. Örneğin, bir kullanıcı "Üç pizza sipariş etmek istiyorum" derse. Çıkarılacak iki varlık vardır: _üç_ ve _pizza._ Bunlar, bir emir vermek olan niyeti yerine getirmek için kullanılır. 

Bir varlık şeması için, söyleyiş niyeti istemci uygulaması için daha az önemlidir. 

Bir varlık şemasını düzenlemenin yaygın bir yöntemi, tüm örnek sözcüklerini **Yok** amacına eklemektir. 

### <a name="example-of-a-mixed-schema"></a>Karışık şema örneği

En güçlü ve olgun şema, çok çeşitli varlıklara ve özelliklere sahip bir niyet şemasıdır. Bu şema bir niyet veya varlık şeması olarak başlayabilir ve istemci uygulaması bu bilgi parçaları na ihtiyaç duyduğundan, her ikisinin de kavramlarını içerecek şekilde büyüyebilir. 

## <a name="add-example-utterances-to-intents"></a>Niyetlere örnek söz ekleme

LUIS her **niyet**birkaç örnek deyişler ihtiyacı var. Örnek, sözcüğün hangi amaç için olduğunu belirleyebilmek için kelime seçimi ve sözcük sırasının yeterli varyasyonuna ihtiyaç duyar. 

> [!CAUTION]
> Örnek sözlerini toplu olarak eklemeyin. 15 ila 30 özel ve değişen örneklerle başlayın. 

Her örnek söyleyiş, tasarlanmış ve **varlıklar**ile etiketlenmiş **ayıklamak için gerekli veri** olması gerekir. 

|Anahtar öğesi|Amaç|
|--|--|
|Amaç|Kullanıcı sözcüklerini tek bir niyet veya eylem olarak **sınıflandırın.** Örnekler `BookFlight` şunlardır ve `GetWeather`.|
|Varlık|Niyeti tamamlamak için gerekli olan söyleyiş verilerini **ayıklayın.** Örnekler arasında seyahat tarihi ve saati ve konumu yer almaktadır.|

Bir LUIS uygulaması, bir uygulamanın etki alanıyla alakalı olmayan sözcükleri yok saymak için, söyleyişi **Yok** amacına atayarak tasarlanabilir.

## <a name="test-and-train-your-app"></a>Uygulamanızı test edin ve eğitin

Her bir amaç için 15 ila 30 farklı örnek söz verdikten sonra, gerekli varlıklar etiketlendikten sonra, LUIS uygulamanızı test edip [eğitmeniz](luis-how-to-train.md) gerekir. 

## <a name="publish-to-a-prediction-endpoint"></a>Tahmin bitiş noktasına yayımlama

LUIS uygulaması, liste [tahmini bitiş noktası bölgelerinde](luis-reference-regions.md)sizin için kullanılabilir olacak şekilde yayımlanmalıdır.

## <a name="test-your-published-app"></a>Yayınlanan uygulamanızı test edin

Yayınlanan LUIS uygulamanızı HTTPS tahmin bitiş noktasından test edebilirsiniz. Tahmin bitiş noktasından test LUIS [gözden geçirilmesi](luis-how-to-review-endpoint-utterances.md)için düşük güven ile herhangi bir söyleyiş seçmenize olanak sağlar.  

## <a name="create-a-new-version-for-each-cycle"></a>Her döngü için yeni bir sürüm oluşturma

Her sürüm, LUIS uygulamasızamanında bir anlık görüntüdür. Uygulamada değişiklik yapmadan önce yeni bir sürüm oluşturun. Eski bir sürüme geri dönmek, önceki bir duruma olan niyet ve söyleyişlerini kaldırmaya çalışmaktan daha kolaydır.

Sürüm kimliği karakterlerden, basamaklardan veya '.' dan oluşur ve 10 karakterden uzun olamaz.

İlk sürüm (0.1) varsayılan etkin sürümüdür. 

### <a name="begin-by-cloning-an-existing-version"></a>Varolan bir sürümü klonlayarak başlayın

Her yeni sürüm için başlangıç noktası olarak kullanmak üzere varolan bir sürümü klonla. Bir sürümü klonladıktan sonra, yeni sürüm **etkin** sürüm olur. 

### <a name="publishing-slots"></a>Yayın yuvaları

Sahne ve/veya üretim yuvalarında yayınlayabilirsiniz. Her yuvanın farklı bir sürümü veya aynı sürümü olabilir. Bu, botlar veya diğer LUIS arama uygulamaları için kullanılabilen üretime yayımlanmadan önce değişiklikleri doğrulamak için yararlıdır. 

Eğitimli sürümler LUIS uygulamanızın [bitiş noktasında](luis-glossary.md#endpoint)otomatik olarak kullanılamaz. Luis uygulama bitiş noktanızda kullanılabilmesi için bir sürümü [yayımlamanız](luis-how-to-publish-app.md) veya yeniden yayımlamanız gerekir. **Aşamaaşama** ve **Üretim'de**yayınlayabilir ve size uygulamanın bitiş noktasında bulunan iki versiyonunu verebilirsiniz. Uygulamanın daha fazla sürümünün bir bitiş noktasında kullanılabilir olması gerekiyorsa, sürümü dışa aktarmalı ve yeni bir uygulamaya yeniden aktarmalısınız. Yeni uygulamanın farklı bir uygulama kimliği var.

### <a name="import-and-export-a-version"></a>Bir sürümü alma ve dışa aktarma

Bir sürüm uygulama düzeyinde içe aktarılabilir. Bu sürüm etkin sürüm olur ve uygulama `versionId` dosyasının özelliğinde sürüm kimliğini kullanır. Sürüm düzeyinde varolan bir uygulamaya da içe aktarabilirsiniz. Yeni sürüm etkin sürüm olur. 

Bir sürüm uygulama veya sürüm düzeyinde de dışa aktarılabilir. Tek fark, uygulama düzeyinde dışa aktarılan sürümün şu anda etkin sürüm olduğu, sürüm düzeyinde ise **[Ayarlar](luis-how-to-manage-versions.md)** sayfasında dışa aktarmak için herhangi bir sürümü seçebiliyor olmasıdır. 

Dışa aktarılan dosya **şunları** içermez:

* Makineden öğrenilen bilgiler, çünkü uygulama alındıktan sonra yeniden eğitilir
* Katılımcı bilgileri

LUIS uygulama şemanızı yedeklemek için [LUIS portalından](https://www.luis.ai/applications)bir sürüm dışa aktarın.

## <a name="manage-contributor-changes-with-versions-and-contributors"></a>Katılımcı değişikliklerini sürümler ve katkıda bulunanlarla yönetme

LUIS, Azure kaynak düzeyinde izinler sağlayarak bir uygulamaya katkıda bulunanlar kavramını kullanır. Hedeflenen işbirliği sağlamak için bu kavramı sürümle birleştirin. 

Uygulamanızdaki katılımcı değişikliklerini yönetmek için aşağıdaki teknikleri kullanın.

### <a name="manage-multiple-versions-inside-the-same-app"></a>Aynı uygulama içinde birden çok sürümü yönetme

Her yazar için bir temel sürümden [klonlama](luis-how-to-manage-versions.md#clone-a-version) ile başlayın. 

Her yazar uygulamanın kendi sürümünde değişiklik yapar. Yazar modelden memnun kaldığında, yeni sürümleri JSON dosyalarına dışa aktarın.  

Dışa aktarılan uygulamalar, .json veya .lu dosyaları, değişikliklerle karşılaştırılabilir. Yeni sürümün tek bir dosyasını oluşturmak için dosyaları birleştirin. Yeni `versionId` birleştirilmiş sürümü belirtmek için özelliği değiştirin. Bu sürümü orijinal uygulamaya aktarın. 

Bu yöntem, bir etkin sürümü, bir aşama sürümü ve bir yayımlanmış sürümü sağlar. Etkin sürümün sonuçlarını [etkileşimli test bölmesinde](luis-interactive-test.md)yayımlanmış bir sürümle (aşama veya üretim) karşılaştırabilirsiniz.

### <a name="manage-multiple-versions-as-apps"></a>Uygulama olarak birden çok sürümü yönetme

Temel sürümü [dışa](luis-how-to-manage-versions.md#export-version) aktarın. Her yazar sürümü içeri yedirir. Uygulamayı içeri yediren kişi sürümün sahibidir. Uygulamayı değiştirmeleri bittiğinde, sürümü dışa aktarın. 

Dışa aktarılan uygulamalar, değişiklikler için temel dışa aktarma yla karşılaştırılabilen JSON biçimli dosyalardır. Yeni sürümün tek bir JSON dosyası oluşturmak için dosyaları birleştirin. Yeni birleştirilmiş sürümü belirtmek için JSON'daki **versionId** özelliğini değiştirin. Bu sürümü orijinal uygulamaya aktarın.

[Ortak çalışanlardan](luis-how-to-collaborate.md)katkı yazma hakkında daha fazla bilgi edinin.

## <a name="review-endpoint-utterances-to-begin-the-new-iterative-cycle"></a>Yeni yinelemeli döngüye başlamak için uç nokta söyleyişlerini gözden geçirin

Bir yineleme döngüsü ile bitirdiğinizde, işlemi yineleyebilirsiniz. Luis düşük güven ile işaretlenmiş [tahmin uç nokta söyleyerek gözden geçirerek](luis-how-to-review-endpoint-utterances.md) başlayın. Hem doğru öngörülen niyet ve doğru ve tam varlık ayıklanmış için bu söyleyiyi kontrol edin. Değişiklikleri gözden geçirip kabul ettikten sonra, inceleme listesi boş olmalıdır.  

## <a name="next-steps"></a>Sonraki adımlar

[İşbirliği](luis-concept-keys.md)yle ilgili kavramları öğrenin.
