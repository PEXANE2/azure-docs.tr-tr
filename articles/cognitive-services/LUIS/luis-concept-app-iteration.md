---
title: Yinelemeli uygulama tasarımı-LUSıS
titleSuffix: Azure Cognitive Services
description: Lu, bir model değişikliği, söylenişi örnekleri, yayımlama ve uç nokta sorgularından veri toplamayı en iyi şekilde öğreniyor.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/06/2020
ms.author: diberry
ms.openlocfilehash: 560a7d9106b9eaef0f82766615253715deb9238a
ms.sourcegitcommit: bcb962e74ee5302d0b9242b1ee006f769a94cfb8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86057883"
---
# <a name="iterative-app-design-for-luis"></a>LUSıS için yinelemeli uygulama tasarımı

Language Understanding (LUSıS) uygulaması, yineleme ile en verimli şekilde öğrenir ve çalışır. İşte tipik bir yineleme çevrimi:

* Yeni sürüm oluştur
* LUSıS uygulama şemasını düzenleyin. Buna aşağıdakiler dahildir:
    * Örnek söyleyle amaçları
    * Varlıklar
    * Özellikler
* Eğitme, test ve yayımlama
    * Etkin öğrenme için tahmin uç noktasında test edin
* Uç nokta sorgularından veri toplama

![Yazma döngüsü](./media/luis-concept-app-iteration/iteration.png)

## <a name="building-a-luis-schema"></a>LUSıS şeması oluşturma

Bir uygulamanın şeması, kullanıcının ne sorabileceğini ( _Amaç_ veya _Amaç_ ) ve amacının hangi bölümlerinin yanıt belirlemek için kullanılan ayrıntıları ( _varlıklar_olarak adlandırılır) sağlar.

Uygulama şemasının, ilgili kelimeleri ve tümcecikleri ve tipik sözcük sıralamasını belirlemesi için uygulama etki alanlarına özel olması gerekir.

Örnek söyleyme, uygulamanın çalışma zamanında beklediği tanınan konuşma veya metin gibi kullanıcı girişlerini temsil eder.

Şema için _Amaç gerekir ve varlıklar olmalıdır_ .

### <a name="example-schema-of-intents"></a>Örnek bir amaç şeması

En yaygın şema, amaçlar ile düzenlenmiş bir amaç şemadır. Bu tür bir şema, bir kullanıcının amaç olduğunu belirlemede HALSıS kullanır.

Amaç şeması türü, LUSıS 'in kullanıcının amacını belirlemesine yardımcı olması halinde varlıklara sahip olabilir. Örneğin, bir gönderim varlığı (bir amaca yönelik _makine öğrenimi özelliği_ olarak), luın bir sevkiyat amacını belirlemesine yardımcı olur.

### <a name="example-schema-of-entities"></a>Varlıkların örnek şeması

Bir varlık şeması, Kullanıcı utslarından ayıklanan veriler olan varlıklara odaklanır. Örneğin, bir Kullanıcı söyledim, "üç Pizzas sıralamak istiyorum." Ayıklanacak iki varlık vardır: _üç_ ve _Pizzas_. Bunlar, bir sipariş oluşturmak için kullanılan amaç yerine getirmek için kullanılır.

Bir varlık şeması için, söylenişi 'in amacı, istemci uygulaması için daha az önemlidir.

Bir varlık şemasının düzenlenmesinin yaygın bir yöntemi, tüm örnek söyleylerini **none** amacına eklemektir.

### <a name="example-of-a-mixed-schema"></a>Karışık şema örneği

En güçlü ve olgun şeması, eksiksiz bir varlık ve özellik aralığına sahip bir amaç şemadır. Bu şema, bir amaç veya varlık şeması olarak başlayabilir ve istemci uygulaması bu bilgi parçasına ihtiyaç duydukça her ikisinin kavramlarını dahil etmek için büyüyebilir.

## <a name="add-example-utterances-to-intents"></a>Amaçlar için örnek ekleme

LUO 'NUN her bir **Amaç**için birkaç örnek elde etmek gerekir. Örnek söyleyler, hangi Amaçın hangi amaca yönelik olduğunu belirleyebilmek için yeterli sözcük seçimi ve sözcük sırası çeşitlemesine gerek duyar.

> [!CAUTION]
> Toplu olarak örnek eklemeyin. 15 ila 30 ' a ve değişen örneklerle başlayın.

Her örnek söylenişi 'in, **varlıklar**ile tasarlanıp etiketlenmesi **için gerekli verilerin** olması gerekir.

|Anahtar öğesi|Amaç|
|--|--|
|Amaç|Kullanıcı araslarını tek bir amaç veya eylem halinde **sınıflandırın** . Örnekler `BookFlight` ve içerir `GetWeather` .|
|Varlık|Bir amaç için gereken materance 'tan veri **ayıklayın** . Örneğin, seyahat tarihi ve saati ve konumunu içerir.|

Bir LUSıS uygulaması, bir uygulamanın etki alanıyla ilgili olmayan, **yok** etme amacına atanarak, bir uygulamanın etki alanıyla ilgisi olmayan utbotları yok sayacak şekilde tasarlanabilir.

## <a name="test-and-train-your-app"></a>Uygulamanızı test edin ve eğitme

Her bir amaca göre 15 ila 30 farklı örneğe sahip olduktan sonra, etiketli gerekli varlıklar sayesinde LUSıS uygulamanızı test etmeniz ve [eğmeniz](luis-how-to-train.md) gerekir.

## <a name="publish-to-a-prediction-endpoint"></a>Tahmin uç noktasında Yayımlama

LUO uygulamasının, liste [Tahmini uç nokta bölgelerinde](luis-reference-regions.md)kullanılabilir olması için yayımlanması gerekir.

## <a name="test-your-published-app"></a>Yayınlanan uygulamanızı test etme

Yayınlanan LUSıS uygulamanızı HTTPS tahmin uç noktasından test edebilirsiniz. Tahmin uç noktasından test etmek, LUSıS 'nin [Gözden geçirme](luis-how-to-review-endpoint-utterances.md)için düşük güvenilirliğe sahip herhangi bir noktayı seçmesine olanak sağlar.

## <a name="create-a-new-version-for-each-cycle"></a>Her bir döngüyle ilgili yeni bir sürüm oluşturun

Her sürüm, LUSıS uygulamasının zaman içindeki bir anlık görüntüdür. Uygulamada değişiklik yapmadan önce yeni bir sürüm oluşturun. Daha eski bir sürüme geri dönmek daha kolaydır. Bu, önceki bir duruma göre amaçları ve kullanımları kaldırmaya çalışır.

Sürüm KIMLIĞI, karakterler, rakamlar veya '. ' karakterlerinden oluşur ve 10 karakterden uzun olamaz.

İlk sürüm (0,1) varsayılan etkin sürümdür.

### <a name="begin-by-cloning-an-existing-version"></a>Mevcut bir sürümü klonlayarak başlayın

Her yeni sürüm için başlangıç noktası olarak kullanmak üzere var olan bir sürümü kopyalayın. Bir sürümü klonladıktan sonra, yeni sürüm **etkin** sürüm olur.

### <a name="publishing-slots"></a>Yayımlama Yuvaları

Aşama ve/veya üretim yuvalarında yayımlayabilirsiniz. Her yuva farklı bir sürüme veya aynı sürüme sahip olabilir. Bu, robotların veya başka bir LUıN uygulama çağıran uygulamalar tarafından kullanılabilen üretime yayımlamadan önce değişiklikleri doğrulamak için yararlıdır.

Eğitilen sürümler, LUSıS uygulamanızın [uç noktasında](luis-glossary.md#endpoint)otomatik olarak kullanılamaz. LUSıS uygulama uç noktanıza kullanılabilmesi için bir sürümü [yayımlamanız](luis-how-to-publish-app.md) veya yeniden yayımlamanız gerekir. **Hazırlama** ve **üretime**yayınlayabilirsiniz ve bu sayede, uç noktada uygulamanın iki sürümünü kullanabilirsiniz. Bir uç noktada uygulamanın daha fazla sürümünün kullanılabilir olması gerekiyorsa, sürümü dışarı aktarıp yeni bir uygulamaya yeniden içeri aktarmanız gerekir. Yeni uygulamanın farklı bir uygulama KIMLIĞI vardır.

### <a name="import-a-version"></a>Bir sürümü içeri aktar

Bir sürüm, yeni olarak **içeri aktarılabilir** :
* Uygulama, yeni bir uygulama KIMLIĞIYLE
* Mevcut bir uygulamanın sürümü

Bu sürüm etkin sürüm olur ve `versionId` uygulama dosyasının özelliğindeki sürüm kimliğini kullanır.

### <a name="export-a-version"></a>Bir sürümü dışarı aktarma

Bir sürüm, LUO portalından uygulama düzeyinde ya da sürüm düzeyinde **aktarılabilir** :

* Uygulama düzeyi- **uygulamalarım** sayfasında uygulama ' yı seçin ve ardından **dışarı aktar** ' ı seçin
* Sürüm düzeyi- **uygulamalarım** sayfasında uygulama bağlantısı ' nı seçin, **Yönet**' i seçin, **sürümler** ' i seçin

Tek fark, uygulama düzeyinde dışarı aktarılmış sürüm, sürüm düzeyinde şu anda etkin olan sürümdür, **[Ayarlar](luis-how-to-manage-versions.md)** sayfasında dışarı aktarılacak herhangi bir sürümü seçebilirsiniz.

İçe **aktarılmış dosya şunları** içermez:

* makine öğrenimi bilgileri, uygulama alındıktan sonra geri çekme yaptığından
* Katkıda bulunan bilgileri

LUSıS uygulama şemanızı yedeklemek için, bir sürümü [lusıs portalından](https://www.luis.ai/applications)dışarı aktarın.

## <a name="manage-contributor-changes-with-versions-and-contributors"></a>Sürümler ve katkıda bulunanlar ile katkıda bulunan değişiklikleri yönetin

LUSıS, Azure Kaynak düzeyi izinleri sunarak bir uygulamaya katkıda bulunanlar kavramını kullanır. Hedeflenen işbirliği sağlamak için bu kavramı sürümü oluşturma ile birleştirin.

Uygulamanıza katkıda bulunan değişiklikleri yönetmek için aşağıdaki teknikleri kullanın.

### <a name="manage-multiple-versions-inside-the-same-app"></a>Aynı uygulama içinde birden çok sürümü yönetme

Her yazar için bir temel sürümden [kopyalamaya](luis-how-to-manage-versions.md#clone-a-version) başlayın.

Her yazar uygulamanın kendi sürümünde değişiklik yapar. Yazar modelle karşılanmıyorsa, yeni sürümleri JSON dosyalarına dışarı aktarın.

Aktarılmış uygulamalar `.json` veya `.lu` dosyalar, değişiklikler için karşılaştırılabilir. Yeni sürümden tek bir dosya oluşturmak için dosyaları birleştirin. `versionId`Özelliği yeni birleştirilmiş sürümü işaret etmek üzere değiştirin. Bu sürümü özgün uygulamaya aktarın.

Bu yöntem, bir etkin sürüme, bir aşama sürümüne ve bir yayımlanmış sürüme sahip etmenize olanak tanır. Etkin sürüm ile ilgili sonuçları, [etkileşimli test bölmesindeki](luis-interactive-test.md)yayımlanmış bir sürümle (aşama veya üretim) karşılaştırabilirsiniz.

### <a name="manage-multiple-versions-as-apps"></a>Birden çok sürümü uygulama olarak yönetme

Temel sürümü [dışarı aktarın](luis-how-to-manage-versions.md#export-version) . Her yazar sürümü içeri aktarır. Uygulamayı içeri aktaran kişi, sürümün sahibidir. Uygulamayı değiştirme bittiğinde sürümü dışarı aktarın.

Dışarı aktarılmış uygulamalar, değişiklikler için temel dışarı aktarmaya kıyasla JSON biçimli dosyalardır. Yeni sürümden tek bir JSON dosyası oluşturmak için dosyaları birleştirin. JSON içindeki **VersionId** özelliğini yeni birleştirilmiş sürümü işaret etmek üzere değiştirin. Bu sürümü özgün uygulamaya aktarın.

[Ortak çalışanlarla](luis-how-to-collaborate.md)ilgili katkıları yazma hakkında daha fazla bilgi edinin.

## <a name="review-endpoint-utterances-to-begin-the-new-iterative-cycle"></a>Yeni yinelemeli döngüyü başlatmak için uç nokta utslerini gözden geçirin

Bir yineleme döngüsüyle işiniz bittiğinde, işlemi tekrarlayabilirsiniz. [İnceleme tahmini uç noktası utterations](luis-how-to-review-endpoint-utterances.md) lu, düşük güvenilirlikle işaretlendi. Bu söyleyeni, hem doğru tahmini amaç hem de doğru ve tamamen ayıklanan varlık için denetleyin. Değişiklikleri gözden geçirdikten ve kabul ettikten sonra, gözden geçirme listesinin boş olması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

[İşbirliği](luis-how-to-azure-subscription.md)hakkında kavramları öğrenin.
