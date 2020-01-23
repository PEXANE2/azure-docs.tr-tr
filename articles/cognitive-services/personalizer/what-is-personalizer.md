---
title: Kişiselleştirme nedir?
titleSuffix: Azure Cognitive Services
description: Kişiselleştirici, gerçek zamanlı davranışlarından öğrenerek kullanıcılarınıza gösterilecek en iyi deneyimi seçmenize olanak tanıyan bulut tabanlı bir API hizmetidir.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: overview
ms.date: 01/21/2020
ms.author: diberry
ms.openlocfilehash: 756363d0c46dee6f7d0037fda48ab22dbdaeb0b0
ms.sourcegitcommit: 38b11501526a7997cfe1c7980d57e772b1f3169b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76514315"
---
# <a name="what-is-personalizer"></a>Kişiselleştirme nedir?

Azure kişiselleştirici, istemci uygulamanızın her kullanıcıyı göstermek için en iyi, tek _içerik_ öğesini seçmesini sağlayan bulut tabanlı bir API hizmetidir. Hizmet, içerik ve bağlam hakkında sağladığınız toplu gerçek zamanlı bilgilere göre içerik öğelerinden en iyi öğeyi seçer.

Kullanıcı için içerik öğesini sunduktan sonra, sistem kullanıcı davranışını izler ve aldığı bağlam bilgilerine göre en iyi içeriği seçme yeteneğini geliştirmek için bir geri puanı Kişiselleştiriciye bildirir.

**İçerik** , metin, görüntü, URL veya e-posta gibi istediğiniz herhangi bir bilgi birimi olabilir.

<!--
![What is personalizer animation](./media/what-is-personalizer.gif)
-->

## <a name="how-does-personalizer-select-the-best-content-item"></a>Kişiselleştirici en iyi içerik öğesini nasıl seçer?

Kişiselleştirici, tüm kullanıcılar arasında toplu davranışa ve yeniden puanları temel alan en iyi öğeyi (_eylem_) seçmek için **pekiştirmeye dayalı Learning** kullanır. Eylemler, haber makaleleri, belirli filmler veya aralarından seçim yapabileceğiniz ürünler gibi içerik öğeleridir.

**Sıralama** çağrısı, eylem öğesini ve eylem özelliklerini ve en üstteki eylem öğesini seçmek için bağlam özelliklerini alır:

* **Özelliklerle Ilgili eylemler** -her bir öğeye özgü özelliklerle içerik öğeleri
* **Bağlam özellikleri** -uygulamanızın kullanımı sırasında kullanıcılarınızın özellikleri, bağlamları veya ortamları

Sıralama çağrısı, **geri dönüş EYLEMI kimliği** alanında kullanıcıya gösterilecek içerik öğesi, __eylem__kimliğini döndürür.
Kullanıcıya gösterilen __eylem__ , makine öğrenimi modelleriyle seçilir ve zaman içinde toplam yeniden kullanım miktarını en üst düzeye çıkarmaya çalışıyor.

Birkaç örnek senaryo şunlardır:

|İçerik türü|**Eylemler (özelliklerle birlikte)**|**Bağlam özellikleri**|Geri alınan geri dönüş eylemi KIMLIĞI<br>(Bu içeriği görüntüle)|
|--|--|--|--|
|Haber listesi|a. `The president...` (Ulusal, politika, [metin])<br>b. `Premier League ...` (küresel, spor, [metin, görüntü, video])<br> c. `Hurricane in the ...` (bölgesel, hava durumu, [metin, görüntü]|Cihaz haberleri okundu<br>Ay veya mevsim<br>|bir `The president...`|
|Filmler listesi|1. `Star Wars` (1977, [Action, Adventure, FI], George Lucas)<br>2. `Hoop Dreams` (1994, [belgesel, spor], Steve James<br>3. `Casablanca` (1942, [Romantik, Drama, war], Michael Curtiz)|Cihaz filminin izlenen<br>ekran boyutu<br>Kullanıcı türü<br>|3. `Casablanca`|
|Ürün listesi|i. `Product A` (3 kg, $ $ $ $, 24 saat içinde teslim et)<br>ii. `Product B` (20 kg, $ $, 2 hafta, gümrük ile teslim)<br>iii. `Product C` (3 kg, $ $ $, 48 saat içinde teslim)|Cihaz alışverişi buradan okundu<br>Kullanıcının harcama katmanı<br>Ay veya mevsim|ii. `Product B`|

Kişiselleştirici, pekiştirmeye dayalı öğrenimi, bir birleşimini temel alarak _eylem kimliği_olarak bilinen tek bir en iyi eylemi seçmek için kullanılır:
* Eğitilen model-son bilgiler kişiselleştirme hizmeti alındı
* Özelliklerle ve bağlam özellikleriyle geçerli veriye özgü eylemler

## <a name="when-to-call-personalizer"></a>Kişiselleştirici ne zaman çağrılacağını

Kişiselleştirici 'in **sıralama** [API 'si](https://go.microsoft.com/fwlink/?linkid=2092082) , _her içerik her seferinde_ gerçek zamanlı olarak çağrılır. Bu, _olay kimliği_ile belirtilen bir **olay**olarak bilinir.

Kişiselleştiriciye yönelik **Reward** [API 'si](https://westus2.dev.cognitive.microsoft.com/docs/services/personalizer-api/operations/Reward) gerçek zamanlı olarak çağrılabilir veya altyapınıza daha iyi uyum sağlamak için gecikebilir. İş gereksinimlerinize göre ödül puanı ' nı belirlersiniz. Bu, iyi için 1 ve hatalı için 0 gibi tek bir değer ya da iş hedeflerinizi ve ölçümünüzü göz önünde bulundurarak oluşturduğunuz bir algoritma tarafından üretilen bir sayı olabilir.

## <a name="personalizer-content-requirements"></a>Kişiselleştirici içerik gereksinimleri

İçeriğiniz için kişiselleştirici kullanın:

* Öğesinin arasından seçim yapmak için sınırlı sayıda öğe (en fazla ~ 50) vardır. Daha büyük bir listeniz varsa, listeyi 50 öğe olarak azaltmak için [bir öneri altyapısı kullanın](where-can-you-use-personalizer.md#use-personalizer-with-recommendation-engines) .
* , Derecelendirilir, Özellikler ve _bağlam özellikleriyle_ _Eylemler_ hakkında bilgi içerir.
* Kişiselleştirici için en az ~ 1k/gün içeriğe ilişkin olay ile ilgili olaylar geçerlidir. Kişiselleştirici gereken en düşük trafiği almazsa, hizmet en iyi tek içerik öğesini belirlemede daha uzun sürer.

Kişiselleştirici, tek en iyi içerik öğesini döndürmek için neredeyse gerçek zamanlı olarak toplu bilgiler kullandığından, hizmet şunları yapmaz:
* Kullanıcı profili bilgilerini kalıcı yapın ve yönetin
* Bireysel kullanıcıların tercihlerini veya geçmişini günlüğe kaydet
* Temizlenen ve etiketlenmiş içerik gerektir

## <a name="how-to-design-and-implement-personalizer-for-your-client-application"></a>İstemci uygulamanız için kişiselleştirici tasarlama ve uygulama

1. İçeriği, **_eylemleri_** ve **_bağlamı_** [tasarlayın](concepts-features.md) ve planlayın. **_Ödül_** puanı için yeniden dengeleme algoritmasını saptayın.
1. Oluşturduğunuz her bir [kişiselleştirici kaynağı](how-to-settings.md) 1 öğrenme döngüsü olarak kabul edilir. Döngü, bu içerik veya Kullanıcı deneyimi için hem derecelendirme hem de yeniden çağrıları alacaktır.
1. Web sitenize veya içerik sisteminize kişiselleştirici ekleyin:
    1. İçerik kullanıcıya gösterilmeden önce en iyi, tek _içerik_ öğesini belirleyebilmek için, uygulamanızda, Web sitenizde veya sisteminizde kişiselleştiriciye bir **Derecelendirme** çağrısı ekleyin.
    1. Kullanıcıya döndürülen geri dönüş _EYLEMI kimliği_olan en iyi, tek _içerik_ öğesini görüntüleyin.
    1. Kullanıcının nasıl davrandığını öğrenmek için, şu gibi, **yeniden** Puanlama sağlamak üzere toplanan bilgilere _algoritma_ uygulayın:

        |Davranış|Hesaplanan ödül puanı|
        |--|--|
        |Kullanıcı tarafından seçilen en iyi, tek _içerik_ öğesi (Reward eylem kimliği)|**1**|
        |Kullanıcı tarafından seçilen diğer içerik|**0**|
        |Kullanıcı duraklatıldı, en iyi ve tek içerik öğesini seçmeden önce, tek _içerik_ öğesi (Reward eylem kimliği)|**0,5**|

    1. Bir **ödül** çağrısı ekleyerek 0 ile 1 arasında bir ödül puanı gönderiliyor
        * İçeriğinizi gösterdikten hemen sonra
        * Veya sonraki bir süre sonra çevrimdışı bir sistem
    1. Bir kullanım süresinden sonra [çevriminizi](concepts-offline-evaluation.md) çevrimdışı değerlendirmede değerlendirin. Çevrimdışı bir değerlendirme, kodunuzu değiştirmeden veya Kullanıcı deneyimini etkilemeden, kişiselleştirici hizmetin verimliliğini test etmenize ve değerlendirmenize olanak tanır.

## <a name="next-steps"></a>Sonraki adımlar


* [Kişiselleştirici nasıl kullanılır?](how-personalizer-works.md)
* [Pekiştirmeye dayalı Learning nedir?](concepts-reinforcement-learning.md)
* [Sıralama isteğine yönelik özellikler ve eylemler hakkında bilgi edinin](concepts-features.md)
* [Reward isteği için puanı belirleme hakkında bilgi edinin](concept-rewards.md)
* [Hızlı başlangıçlar]()
* [Öğretici]()
* [Etkileşimli tanıtımı kullanma](https://personalizationdemo.azurewebsites.net/)
