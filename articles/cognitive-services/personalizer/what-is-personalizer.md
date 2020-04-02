---
title: Kişiselleştirme nedir?
titleSuffix: Azure Cognitive Services
description: Personalizer, kullanıcılarınıza gerçek zamanlı davranışlarından ders çıkarılarak göstermek için en iyi deneyimi seçmenize olanak tanıyan bulut tabanlı bir API hizmetidir.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: overview
ms.date: 01/21/2020
ms.author: diberry
ms.openlocfilehash: ac6d72cbb69796851e58098138d51bb0988be4ec
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80520985"
---
# <a name="what-is-personalizer"></a>Kişiselleştirme nedir?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

Azure Personalizer, istemci uygulamanızın her kullanıcıyı göstermek için en iyi, tek _içerik_ öğesini seçmesine yardımcı olan bulut tabanlı bir API hizmetidir. Hizmet, içerik ve bağlam hakkında sağladığınız toplu gerçek zamanlı bilgilere dayanarak içerik öğelerinden en iyi öğeyi seçer.

İçerik öğesini kullanıcınıza sunduktan sonra, sisteminiz kullanıcı davranışını izler ve aldığı bağlam bilgilerine göre en iyi içeriği seçme yeteneğini geliştirmek için bir ödül puanını Personalizer'a geri bildirir.

**İçerik,** kullanıcınıza göstermek için seçmek istediğiniz metin, resim, url veya e-posta gibi herhangi bir bilgi birimi olabilir.

<!--
![What is personalizer animation](./media/what-is-personalizer.gif)
-->

## <a name="how-does-personalizer-select-the-best-content-item"></a>Personalizer en iyi içerik öğesini nasıl seçer?

Personalizer tüm kullanıcılar arasında kolektif davranış ve ödül puanları dayalı en iyi öğe _(eylem)_ seçmek için **takviye öğrenme** kullanır. Eylemler, haber makaleleri, belirli filmler veya seçim yapabileceğiniz ürünler gibi içerik öğeleridir.

**Rank** çağrısı eylem öğesini, eylem özellikleriyle birlikte alır ve en üstteki eylem öğesini seçmek için bağlam özellikleri:

* **Özelliklere sahip eylemler** - her öğeye özgü özelliklere sahip içerik öğeleri
* **Bağlam özellikleri** - uygulamanızı kullanırken kullanıcılarınızın özellikleri, bağlamları veya çevreleri

Rank çağrısı, **Ödül Eylem Kimliği** alanında kullanıcıya göstermek üzere hangi içerik öğesinin kimliğini, __eylemi__kullanıcıya gösterir.
Kullanıcıya gösterilen __eylem,__ zaman içinde toplam ödül miktarını en üst düzeye çıkarmaya çalışan makine öğrenimi modelleri ile seçilir.

Birkaç örnek senaryo şunlardır:

|İçerik türü|**Eylemler (özelliklere sahip)**|**Bağlam özellikleri**|İade Ödül Eylem Kimliği<br>(bu içeriği görüntüleyin)|
|--|--|--|--|
|Haber listesi|a. `The president...`(ulusal, siyaset, [metin])<br>b. `Premier League ...`(global, spor, [metin, resim, video])<br> c. `Hurricane in the ...`(bölgesel, hava durumu, [metin,resim]|Cihaz haberleri<br>Ay veya sezon<br>|A`The president...`|
|Film listesi|1. `Star Wars` (1977, [aksiyon, macera, fantezi], George Lucas)<br>2. `Hoop Dreams` (1994, [belgesel, spor], Steve James<br>3. `Casablanca` (1942, [romantizm, dram, savaş], Michael Curtiz)|Cihaz filmi izlenir<br>ekran boyutu<br>Kullanıcı türü<br>|3.`Casablanca`|
|Ürün listesi|i. `Product A`(3 kg, $$$$, 24 saat içinde teslim)<br>ii. `Product B`(20 kg, $$, gümrükile 2 hafta nakliye)<br>iii. `Product C`(3 kg, $$$, 48 saat içinde teslimat)|Cihaz alışverişi<br>Kullanıcının harcama katmanı<br>Ay veya sezon|ii. `Product B`|

Personalizer tek en iyi eylem seçmek için takviye öğrenme kullanılan, _ödül eylem kimliği_olarak bilinen , bir arada dayalı:
* Eğitimli model - Personalizer hizmetinin aldığı geçmiş bilgiler
* Güncel veriler - Özellikleri ve bağlam özellikleri ile belirli eylemler

## <a name="when-to-call-personalizer"></a>Personalizer ne zaman arama

Personalizer's **Rank** [API](https://go.microsoft.com/fwlink/?linkid=2092082) gerçek zamanlı olarak, içerik sunmak _her zaman_ denir. Bu bir **olay**olarak bilinir, bir _olay kimliği_ile kaydetti.

Personalizer'ın **Ödül** [API'si,](https://westus2.dev.cognitive.microsoft.com/docs/services/personalizer-api/operations/Reward) altyapınıza daha iyi uyacak şekilde gerçek zamanlı olarak çağrılabilir veya geciktirilebilir. Ödül puanını iş gereksinimlerinize göre belirlersiniz. Ödül puanı 0 ile 1 arasındadır. Bu, iyi için 1 ve kötü ler için 0 gibi tek bir değer veya iş hedefleriniz ve ölçümleriniz göz önünde bulundurularak oluşturduğunuz bir algoritma tarafından üretilen bir sayı olabilir.

## <a name="personalizer-content-requirements"></a>Personalizer içerik gereksinimleri

İçeriğiniz:

* Seçilecek sınırlı bir öğe kümesi (en fazla ~50) vardır. Daha büyük bir listenizin varsa, listeyi 50 öğeye düşürmek için [bir öneri altyapısı kullanın.](where-can-you-use-personalizer.md#how-to-use-personalizer-with-a-recommendation-solution)
* Sıralamalı istediğiniz içeriği açıklayan bilgilere sahiptir: özellikleri ve _bağlam özelliklerine_ _sahip eylemler._
* Personalizer'ın etkili olabilmek için en az ~1k/gün içerikle ilgili etkinliklere sahiptir. Personalizer gereken minimum trafiği almazsa, hizmetin tek en iyi içerik öğesini belirlemesi daha uzun sürer.

Personalizer, tek en iyi içerik öğesini döndürmek için toplu bilgileri neredeyse gerçek zamanlı olarak kullandığından, hizmet şunları yapmaz:
* Kullanıcı profili bilgilerini oluşturma ve yönetme
* Tek tek kullanıcıların tercihlerini veya geçmişini günlüğe kaydetme
* Temizlenmiş ve etiketlenmiş içerik gerektirme

## <a name="how-to-design-and-implement-personalizer-for-your-client-application"></a>Müşteri uygulamanız için Personalizer nasıl tasarlar ve uygularım?

1. [Tasarım](concepts-features.md) ve içerik, **_eylemler_** ve **_bağlam_** için plan . **_Ödül_** puanı için ödül algoritmasını belirleyin.
1. Oluşturduğunuz her [Personalizer Kaynak](how-to-settings.md) 1 Öğrenme Döngüsü olarak kabul edilir. Döngü, bu içerik veya kullanıcı deneyimi için hem Rütbe hem de Ödül çağrılarını alır.
1. Web sitenize veya içerik sisteminize Personalizer ekleyin:
    1. İçerik kullanıcıya gösterilmeden önce en iyi tek _içerik_ öğesini belirlemek için uygulamanızda, web sitenizde veya sisteminizde Personalizzer'a **Bir Rank** çağrısı ekleyin.
    1. Döndürülen _ödül eylem kimliği_olan en iyi, tek _içerik_ öğesini kullanıcıya görüntüleyin.
    1. Kullanıcının nasıl bir şekilde nasıl bir şekilde nasıl bir şekilde nasıl bir şekilde nasıl bir şekilde uygulandığıhakkında toplanan **bilgilere, ödül** puanını belirlemek için _aşağıdakiler_ gibi algoritma uygulayın:

        |Davranış|Hesaplanan ödül puanı|
        |--|--|
        |Kullanıcı en iyi, tek _içerik_ öğesi (ödül eylem kimliği) seçilen|**1**|
        |Kullanıcı seçilen diğer içerik|**0**|
        |Kullanıcı durakladı, kararsız bir şekilde gezinme, en iyi, tek _içerik_ öğesi (ödül eylem kimliği) seçmeden önce|**0.5**|

    1. 0 ile 1 arasında ödül puanı göndererek **ödül** çağrısı ekleme
        * İçeriğinizi gösterdikten hemen sonra
        * Veya bir süre sonra çevrimdışı sistemde
    1. Kullanım süresinden sonra [döngünüzü](concepts-offline-evaluation.md) çevrimdışı bir değerlendirmeyle değerlendirin. Çevrimdışı değerlendirme, kodunuzu değiştirmeden veya kullanıcı deneyimini etkilemeden Kisilikhizmetinin etkinliğini test etmenizi ve değerlendirmenizi sağlar.

## <a name="next-steps"></a>Sonraki adımlar


* [Kişiselleştirme nasıl çalışır?](how-personalizer-works.md)
* [Pekiştirme Öğrenme Nedir?](concepts-reinforcement-learning.md)
* [Rank isteğine ilişkin özellikler ve eylemler hakkında bilgi edinin](concepts-features.md)
* [Ödül isteği için puanı belirleme hakkında bilgi edinin](concept-rewards.md)
* [Hızlı Başlangıçlar](sdk-learning-loop.md)
* [Öğretici](tutorial-use-azure-notebook-generate-loop-data.md)
* [Etkileşimli demoyu kullanma](https://personalizationdemo.azurewebsites.net/)
