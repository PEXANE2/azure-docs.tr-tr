---
title: Özellikler-LUSıS
description: Etiketlemek veya sınıflandırmak istediğiniz girişin nasıl tanınılacağı hakkında ipuçları sağlamak için bir dil modeline özellikler ekleyin.
ms.topic: conceptual
ms.date: 06/10/2020
ms.openlocfilehash: 823c51f0b58481e30ff54814dde03285ad094b9e
ms.sourcegitcommit: f01c2142af7e90679f4c6b60d03ea16b4abf1b97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84677600"
---
# <a name="machine-learning-ml-features"></a>Makine öğrenimi (ML) özellikleri

Machine Learning 'de bir **özellik**,   sistem engellerinizin ve öğrendiği verilerin bir ayırt edici nitelik veya özniteliğidir.

Machine Learning özellikleri, bir kavramı ayırt edecek şeyleri aramak için lu, önemli ipuçları verir. Bu kişiler, LUıN tarafından kullanılabilecek ancak zor kuralları olmayan bir ipuçlardır.  Bu ipuçları, verileri bulmak için etiketlerle birlikte kullanılır.

## <a name="what-is-a-feature"></a>Özellik nedir?

Bir özellik, bir işlev olarak açıklanabilir bir ayrım nitelik: f (x) = y. Özelliği, şöyle görüneceğimizi, örneğin ayrım nitelik için kullanılır. Şemanızı oluştururken, nitelik belirten örnek utnaterance hakkında ne bilirsiniz? Yanıtınız, özellik oluşturmak için en iyi kılavuzunuz.

## <a name="types-of-features"></a>Özellik türleri

 LUO, hem tümcecik listelerini hem de modelleri özellikler olarak destekler:
* Tümcecik listesi özelliği
* Özellik olarak model (amaç veya varlık)

Özellikler, şema tasarımınızın gerekli bir parçası olarak düşünülmelidir.

## <a name="how-you-find-features-in-your-example-utterances"></a>Örnek aradıklarınızın özelliklerini bulma

LUSıS, dil tabanlı bir uygulama olduğundan, Özellikler metin tabanlı olacaktır. Ayırmak istediğiniz nitelik belirten metni seçin. LUSıS için, metin tabanlı en küçük birim belirtecidir. İngilizce dil için, belirteç ve sayılardan oluşan boşluk veya noktalama işareti olmadan bir belirteç bitişik bir yayılım olur. Boşluk bir belirteç değil.

Boşluklar ve noktalama belirteçleri belirteç olmadığından, özellik olarak kullanabileceğiniz metin ipuçlarına odaklanın. Sözcük çeşitlemelerini dahil etmeyi unutmayın:
* çoğul formlar
* fiil zaman hali
* grubunun
* Yazım ve hatalı yazma

Metnin, bir ayrım nitelik, şunları yapmanız gerekir:
* Tam bir sözcük veya tümcecik ile Eşleştir-varlık veya amaca bir özellik olarak bir normal ifade varlığı veya bir liste varlığı eklemeyi düşünün
* Tarih, saat veya kişi adı gibi bilinen bir kavramı Eşleştir-varlık veya amaç için bir özellik olarak önceden oluşturulmuş bir varlık kullanın
* Zamana göre yeni örnekler öğrenin-varlık veya amaç için bir özellik olarak kavramın bazı örneklerinin bir tümcecik listesini kullanın

## <a name="combine-features"></a>Özellikleri Birleştir

Nitelik 'in nasıl açıklandığına ilişkin birkaç seçenek olduğundan, bu nitelik veya kavramı açıklamaya yardımcı olan birden fazla özellik kullanabilirsiniz. Ortak eşleştirme, bir tümcecik listesi özelliğini ve genellikle özellikler olarak kullanılan varlık türlerinden birini kullanmaktır: önceden oluşturulmuş varlık, normal ifade varlığı veya liste varlığı.

### <a name="ticket-booking-entity-example"></a>Bilet ayırma varlığı örneği

İlk örnek olarak, uçuş rezervasyonu hedefi ve bilet kayıt varlığı ile bir uçuş için bir uygulama düşünün.

Bilet kayıt varlığı, uçuş hedefi için makine tarafından öğrenilen bir varlıktır. Konumu ayıklamaya yardımcı olmak için şu iki özelliği kullanın:
* İlgili sözcüklerin,,,,,,,,, `plane` `flight` `reservation``ticket`
* `geographyV2`Varlığa özellik olarak önceden oluşturulmuş varlık

### <a name="pizza-entity-example"></a>Pizza varlık örneği

Başka bir örnek olarak, bir pizza sırası oluşturma amacı ve bir pizza varlığı içeren bir uygulamayı düşünün.

Pizza varlığı, pizza ayrıntıları için makine tarafından öğrenilen bir varlıktır. Ayrıntıları ayıklamaya yardımcı olmak için iki özelliği kullanın:
* İlgili sözcüklerin,,,,,,,,, `cheese` `crust` `pepperoni``pineapple`
* `number`Varlığa özellik olarak önceden oluşturulmuş varlık

## <a name="a-phrase-list-for-a-particular-concept"></a>Belirli bir kavram için tümcecik listesi

Tümcecik listesi, belirli bir kavramı kapsülleyen sözcüklerin veya deyimlerin listesidir ve belirteç düzeyinde büyük/küçük harfe duyarsız eşleşme olarak uygulanır.

Tümcecik listesi eklerken, özelliği şu şekilde ayarlayabilirsiniz:
* **[Genel](#global-features)**. Genel bir özellik tüm uygulama için geçerlidir.

### <a name="when-to-use-a-phrase-list"></a>Tümcecik listesi ne zaman kullanılır?

LUSıS uygulamanızın kavram için yeni öğeleri genelleştirabilmesi ve belirleyebilmesi gerektiğinde, bir tümcecik listesi kullanın. Tümcecik listeleri, hem amaçları hem de varlıkların anlaşılmasına yardımcı olan, etki alanına özgü sözlük gibidir.

### <a name="how-to-use-a-phrase-list"></a>Tümcecik listesi kullanma

Bir tümcecik listesi ile, LUSıS, bağlamı ve genelleştirir, ancak tam metin eşleşmesi değil, öğesine benzer öğeleri belirler.

Tümcecik listesi kullanma adımları:
* Makine öğrenimi varlığı ile başlama
    * Örnek konuşmalar ekleme
    * Makine öğrenimi varlığı ile etiketleme
* Tümcecik listesi ekleme
    * Benzer anlamlara sahip sözcükler ekleyin; olası her sözcük veya **tümceciği eklemeyin.** Bunun yerine, bir kerede birkaç sözcük veya tümce ekleyin, sonra yeniden eğitme ve yayımlama.
    * Önerilen sözcükleri gözden geçirin ve ekleyin

### <a name="a-typical-scenario-for-a-phrase-list"></a>Tümcecik listesi için tipik bir senaryo

Tümcecik listesi için tipik bir senaryo, belirli bir fikrle ilgili kelimeleri iyileştirmeniz olur.

Anlamlarını artırmak için bir tümcecik listesi gerektirebilecek kelimelerin bir örneği tıbbi koşullardır. Şartlar belirli fiziksel, kimyasal, aleutik ya da soyut anlamı olabilir. LUO, konu etki alanınız için tümcecik listesi olmadan bu koşulların önemli olduğunu bilmez.

Tıp şartlarını ayıklamak istiyorsanız:
* İlk olarak bu söyleyler dahilinde örnek bir örnek oluşturun ve tıbbi terimleri etiketleyin.
* Ardından, konu etki alanında yer alan koşullara örnek olarak bir tümcecik listesi oluşturun. Bu tümcecik listesi, etiketlediğiniz gerçek terimi ve aynı kavramı tanımlayan diğer terimleri içermelidir.
* Tümcecik listesini, tümcecik listesinde kullanılan kavramı çıkaran varlığa veya alt varlığa ekleyin. En yaygın senaryo, makine öğrenimi varlığının bir bileşenidir (alt öğesidir). Tümcecik listesinin tüm amaçlar veya varlıklar üzerinde uygulanması gerekiyorsa, tümcecik listesini genel ifade listesi olarak işaretleyin. `enabledForAllModels`Bayrak, bu model KAPSAMıNı API 'de denetler.

### <a name="token-matches-for-a-phrase-list"></a>Tümcecik listesi için belirteç eşleşmeleri

Tümcecik listesi, büyük/küçük harfe bakılmaksızın belirteç düzeyinde geçerlidir. Aşağıdaki grafikte, sözcüğü içeren bir tümcecik listesinin `Ann` Bu sırada aynı karakterlerin çeşitlemelerine nasıl uygulandığı gösterilmektedir.


| Belirteç çeşitlemesi`Ann` | Belirteç bulunduğunda tümcecik listesi eşleşiyor |
|--------------------------|---------------------------------------|
| N<br>n<br>           | Evet-belirteç`Ann`                  |
| Ann 'ın                    | Evet-belirteç`Ann`                  |
| Gamze                     | Belirteç yok`Anne`                  |


<a name="how-to-use-phrase-lists"></a>
<a name="how-to-use-a-phrase-lists"></a>
<a name="phrase-lists-help-identify-simple-exchangeable-entities"></a>

## <a name="a-model-as-a-feature-helps-another-model"></a>Özellik olarak bir model başka bir modele yardımcı olur

Başka bir modele (amaç veya varlık) bir özellik olarak model (amaç veya varlık) ekleyebilirsiniz. Bir özellik olarak mevcut bir amacı veya varlığı ekleyerek, etiketli örneklerle iyi tanımlanmış bir kavram ekliyoruz.

Bir model özellik olarak eklendiğinde, özelliği şu şekilde ayarlayabilirsiniz:
* **[Gerekli](#required-features)**. Modelin tahmin uç noktasından döndürülmesi için gerekli bir özellik bulunması gerekir.
* **[Genel](#global-features)**. Genel bir özellik tüm uygulama için geçerlidir.

### <a name="when-to-use-an-entity-as-a-feature-to-an-intent"></a>Bir varlık bir özellik olarak ne zaman kullanılır?

Bu varlığın algılanması amaç için önemli olduğunda amaca bir özellik olarak bir varlık ekleyin.

Örneğin, amaç bir uçuş kaydı için ise `BookFlight` ve varlık bilet bilgileri (örneğin, lisans sayısı, başlangıç ve hedef) ise, Bilet bilgileri varlığının bulunması, amaç tahmininde önemli bir ağırlık içermelidir `BookFlight` .

### <a name="when-to-use-an-entity-as-a-feature-to-another-entity"></a>Bir varlığın başka bir varlığa özellik olarak ne zaman kullanılacağı

Varlık (a), varlık (B) için önemli olduğunda bu varlığın (A) algılanabilmesi durumunda başka bir varlığa (B) bir özellik olarak eklenmelidir.

Örneğin, n sevkiyat adresi varlığı bir açık adres alt varlığı içeriyorsa, sokak adresi bulma alt varlık, sevkiyat adresi varlığı için tahmine göre önemli bir ağırlık ekler.

* Sevkiyat Adresi (makine tarafından öğrenilen varlık)
    * Cadde numarası (alt varlık)
    * Sokak adresi (alt varlık)
    * Şehir (alt varlık)
    * Eyalet veya Il (alt varlık)
    * Ülke/bölge (alt varlık)
    * Posta kodu (alt varlık)

## <a name="nested-subentities-with-features"></a>Özelliklerle iç içe geçmiş alt varlıklar

Bir makine öğrenilen alt varlık, üst varlık için bir kavram olduğunu belirtir, bu üst varlığın başka bir alt varlık veya en üst varlık olup olmadığı. Alt varlığın değeri, üst öğesi için bir özellik görevi görür.

Bir alt varlık bir özellik olarak hem bir özellik hem de bir model (başka bir varlık) olarak bir ifade listesi içerebilir.

Alt varlık bir tümcecik listesine sahip olduğunda, bu kavram sözlüğünü artırır ancak tahmine yönelik JSON yanıtına hiçbir bilgi eklemez.

Alt varlık başka bir varlık özelliğine sahip olduğunda, JSON yanıtı o varlığın ayıklanan verilerini içerir.

## <a name="required-features"></a>Gerekli özellikler

Modelin tahmin uç noktasından döndürülmesi için gerekli bir özellik bulunması gerekir. Gelen verilerinizin özelliği ile eşleşmesi gerektiğini bildiğiniz durumlarda gerekli bir özelliği kullanın.

Söylenişi metni gerekli özellik ile eşleşmiyorsa ayıklanmaz.

**Gerekli bir özellik makine tarafından öğrenilen bir varlık kullanır**:
* Normal ifade varlığı
* Liste varlığı
* Önceden oluşturulmuş varlık

Gerektiğinde işaretlemek için iyi özellikler nelerdir? Modelinize eminseniz, bu, özelliği gereken şekilde ayarlayın. Gerekli bir özellik, bulunamazsa hiçbir şey döndürmez.

Sevkiyat Adresi örneğine devam ediliyor:
* Sevkiyat Adresi (makine tarafından öğrenilen varlık)
    * Cadde numarası (alt varlık)
    * Sokak adresi (alt varlık)
    * Cadde adı (alt varlık)
    * Şehir (alt varlık)
    * Eyalet veya Il (alt varlık)
    * Ülke/bölge (alt varlık)
    * Posta kodu (alt varlık)

### <a name="required-feature-using-prebuilt-entities"></a>Önceden oluşturulmuş varlıklar kullanılarak gerekli özellik

Şehir, eyalet ve ülke/bölge genellikle kapalı bir liste kümesidir ve bu, zaman içinde çok değiştirmeyeceği anlamına gelir. Bu varlıklar ilgili önerilen özelliklere sahip olabilir ve bu özellikler gerekli olarak işaretlenebilir. Bu, tüm sevkiyat adresinin döndürülmediği anlamına gelir, gerekli özelliklere sahip varlıklar bulunamadı.

Şehir, eyalet veya ülke/bölge, deterance 'de, ancak bir konumda veya luın beklemediği bir konum ya da argo olduğunda ne olur? Bir kısmı çözmenize yardımcı olmak üzere bazı post işlemleri sağlamak istiyorsanız, LUSıS 'den düşük güvenilirlikli bir puan nedeniyle, özelliği gereken şekilde işaretlemeyin.

Sevkiyat adresi için gerekli bir özelliğin başka bir örneği, sokak numarasını gerekli [önceden oluşturulmuş](luis-reference-prebuilt-entities.md) bir sayı yapmak içindir. Bu, bir kullanıcının "1 Microsoft Way" veya "bir Microsoft Way" girmesini sağlar. Her ikisi de cadde numarası alt varlığı için bir sayıda "1" olarak çözümlenir.

### <a name="required-feature-using-list-entities"></a>Liste varlıklarını kullanan gerekli özellik

Bir [liste varlığı](reference-entity-list.md) , kendi eş anlamlılarıyla birlikte kurallı adların bir listesi olarak kullanılır. Gerekli bir özellik olarak, söylenişi kurallı adı ya da bir eş anlamlıyı içermiyorsa, varlık tahmin uç noktasının bir parçası olarak döndürülmez.

Sevkiyat Adresi örneğine devam etmek, şirketinizin yalnızca sınırlı bir ülke/bölge kümesine sevk edildiğini varsayalım. Müşterinizin ülkeye başvurmasına yönelik çeşitli yollar içeren bir liste varlığı oluşturabilirsiniz. LUSıS, utterance 'in metni içinde tam bir eşleşme bulamazsa, bu durumda varlık (liste varlığının gerekli özelliğine sahip olan) tahminde döndürülmez.

|Kurallı ad|Eş anlamlılar|
|--|--|
|Birleşik Devletler|ABD<br>U. S. A<br>ABD<br>ABD<br>0|

Bir sohbet bot gibi istemci uygulaması bir soru sorabilir ve müşterinin ülke/bölge seçiminin sınırlı ve _gerekli_olduğunu anlayabilmesi için, bir soru sorabilirsiniz.

### <a name="required-feature-using-regular-expression-entities"></a>Normal ifade varlıklarını kullanan gerekli özellik

Gerekli özellik olarak kullanılan bir [normal ifade varlığı](reference-entity-regular-expression.md) , zengin metin eşleştirme özellikleri sağlar.

Sevkiyat adresiyle devam etmek için ülke/bölge posta kodlarının sözdizimi kurallarını yakalayan bir normal ifade oluşturabilirsiniz.

## <a name="global-features"></a>Genel Özellikler

En yaygın kullanım, belirli bir modele bir özellik uygulamak olsa da, Özelliği uygulamanızın tamamına uygulamak için **genel bir özellik** olarak yapılandırabilirsiniz.

Genel bir özellik için en yaygın kullanım, uygulamaya başka bir dilden sözcükler gibi ek bir sözlük eklemektir. Müşterileriniz birincil bir dil kullanıyorsa, ancak aynı utde başka bir dil kullanabiliyor olması beklendiğinde, ikincil dilden sözcükler içeren bir özellik ekleyebilirsiniz.

Kullanıcı herhangi bir amaç veya varlık genelinde ikinci dili kullanması beklenen için, ifade listesi genel özellik olarak yapılandırılmış bir tümcecik listesine eklenmelidir.

## <a name="best-practices"></a>En iyi uygulamalar
[En iyi yöntemleri](luis-concept-best-practices.md)öğrenin.

## <a name="next-steps"></a>Sonraki adımlar

* Uygulama modellerinizi tahmin çalışma zamanında [genişletme](schema-change-prediction-runtime.md)
* LUSıS uygulamanıza özellikler ekleme hakkında daha fazla bilgi edinmek için bkz. [özellik ekleme](luis-how-to-add-features.md) .
