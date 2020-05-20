---
title: Özellikler-LUSıS
description: Etiketlemek veya sınıflandırmak istediğiniz girişin nasıl tanınılacağı hakkında ipuçları sağlamak için bir dil modeline özellikler ekleyin.
ms.topic: conceptual
ms.date: 05/14/2020
ms.openlocfilehash: e0fd4470c9e1c2a56562b3783010ff1ef87ff466
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83682152"
---
# <a name="machine-learning-ml-features"></a>Makine öğrenimi (ML) özellikleri

Machine Learning 'de, bir **özellik**   sistem engelinizdeki verilerin bir ayırt edici nitelik veya özniteliğidir.

Machine Learning özellikleri, bir kavramı ayırt edecek şeyleri aramak için lu, önemli ipuçları verir. Bu kişiler, LUıN tarafından kullanılabilecek ancak zor kuralları olmayan bir ipuçlardır.  Bu ipuçları, verileri bulmak için etiketlerle birlikte kullanılır.

 LUSıS her iki ifade listesini destekler ve diğer varlıkları özellikler olarak kullanmaktır:
* Tümcecik listesi özelliği
* Özellik olarak model (amaç veya varlık)

Özellikler, şema tasarımınızın gerekli bir parçası olarak düşünülmelidir.

## <a name="a-phrase-list-for-a-particular-concept"></a>Belirli bir kavram için tümcecik listesi

Tümcecik listesi, belirli bir kavramı kapsülleyen sözcüklerin veya tümceciklerin listesidir.

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
    * Ülke (alt varlık)
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
    * Ülke (alt varlık)
    * Posta kodu (alt varlık)

### <a name="required-feature-using-prebuilt-entities"></a>Önceden oluşturulmuş varlıklar kullanılarak gerekli özellik

Şehir, eyalet ve ülke genellikle kapalı bir liste kümesidir ve bu, zaman içinde çok değiştirmeyeceği anlamına gelir. Bu varlıklar ilgili önerilen özelliklere sahip olabilir ve bu özellikler gerekli olarak işaretlenebilir. Bu, tüm sevkiyat adresinin döndürülmediği anlamına gelir, gerekli özelliklere sahip varlıklar bulunamadı.

Şehir, eyalet veya ülke, deterance içinde, ancak bir konumda ya da lusıs 'in beklemediği argo durumunda ne olur? Bir kısmı çözmenize yardımcı olmak üzere bazı post işlemleri sağlamak istiyorsanız, LUSıS 'den düşük güvenilirlikli bir puan nedeniyle, özelliği gereken şekilde işaretlemeyin.

Sevkiyat adresi için gerekli bir özelliğin başka bir örneği, sokak numarasını gerekli [önceden oluşturulmuş](luis-reference-prebuilt-entities.md) bir sayı yapmak içindir. Bu, bir kullanıcının "1 Microsoft Way" veya "bir Microsoft Way" girmesini sağlar. Her ikisi de cadde numarası alt varlığı için bir sayıda "1" olarak çözümlenir.

### <a name="required-feature-using-list-entities"></a>Liste varlıklarını kullanan gerekli özellik

Bir [liste varlığı](reference-entity-list.md) , kendi eş anlamlılarıyla birlikte kurallı adların bir listesi olarak kullanılır. Gerekli bir özellik olarak, söylenişi kurallı adı ya da bir eş anlamlıyı içermiyorsa, varlık tahmin uç noktasının bir parçası olarak döndürülmez.

Sevkiyat Adresi örneğine devam etmek, şirketinizin yalnızca sınırlı bir ülke kümesine sevk edildiğini varsayalım. Müşterinizin ülkeye başvurmasına yönelik çeşitli yollar içeren bir liste varlığı oluşturabilirsiniz. LUSıS, utterance 'in metni içinde tam bir eşleşme bulamazsa, bu durumda varlık (liste varlığının gerekli özelliğine sahip olan) tahminde döndürülmez.

|Kurallı ad|Eş anlamlılar|
|--|--|
|Birleşik Devletler|ABD<br>U. S. A<br>ABD<br>ABD<br>0|

Bir sohbet botu gibi istemci uygulaması bir soru sorabilir, böylece müşteri Ülke seçiminin sınırlı ve _gerekli_olduğunu anlamıştır.

### <a name="required-feature-using-regular-expression-entities"></a>Normal ifade varlıklarını kullanan gerekli özellik

Gerekli özellik olarak kullanılan bir [normal ifade varlığı](reference-entity-regular-expression.md) , zengin metin eşleştirme özellikleri sağlar.

Sevkiyat adresine devam ederek, ülke posta kodlarının sözdizimi kurallarını yakalayan bir normal ifade oluşturabilirsiniz.

## <a name="global-features"></a>Genel Özellikler

En yaygın kullanım, belirli bir modele bir özellik uygulamak olsa da, Özelliği uygulamanızın tamamına uygulamak için **genel bir özellik** olarak yapılandırabilirsiniz.

Genel bir özellik için en yaygın kullanım, uygulamaya başka bir dilden sözcükler gibi ek bir sözlük eklemektir. Müşterileriniz birincil bir dil kullanıyorsa, ancak aynı utde başka bir dil kullanabiliyor olması beklendiğinde, ikincil dilden sözcükler içeren bir özellik ekleyebilirsiniz.

Kullanıcı herhangi bir amaç veya varlık genelinde ikinci dili kullanması beklenen için, ifade listesi genel özellik olarak yapılandırılmış bir tümcecik listesine eklenmelidir.

## <a name="best-practices"></a>En iyi uygulamalar
[En iyi yöntemleri](luis-concept-best-practices.md)öğrenin.

## <a name="next-steps"></a>Sonraki adımlar

* Uygulama modellerinizi tahmin çalışma zamanında [genişletme](schema-change-prediction-runtime.md)
* LUSıS uygulamanıza özellikler ekleme hakkında daha fazla bilgi edinmek için bkz. [özellik ekleme](luis-how-to-add-features.md) .
