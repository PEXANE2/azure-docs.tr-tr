---
title: Özellik değerlendirmesi - Personalizer
titleSuffix: Azure Cognitive Services
description: Azure portalından Kişiselleştirilmiş kaynağınızda bir Değerlendirme çalıştırdığınızda, Personalizer bağlam ve eylemlerin hangi özelliklerinin modeli etkilediği hakkında bilgi sağlar.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: 31243b5e9da55aafbc376fa416c1b00a4499c116
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "70242425"
---
# <a name="feature-evaluation"></a>Özellik değerlendirmesi

[Azure portalından](https://portal.azure.com)Kilikleştirici kaynağınızda bir Değerlendirme çalıştırdığınızda, Personalizer bağlam ve eylemlerin hangi özelliklerinin modeli etkilediği hakkında bilgi sağlar. 

Bu, aşağıdakiler için yararlıdır:

* Modelde hangi özelliklerden ilham alarak kullanabileceğiniz ek özellikler düşünün.
* Hangi özelliklerin önemli olmadığını görün ve bunları büyük ölçüde kaldırın veya kullanımı neyin etkilediğini daha fazla analiz edin.
* Editörlük veya küratörlük ekiplerine, kataloga getirilmeye değer yeni içerik veya ürünler hakkında rehberlik sağlayın.
* Kişiselleştiriciye özellikler gönderirken meydana gelen sık karşılaşılan sorunları ve hataları giderin.

Daha önemli özellikleri modelinde daha güçlü ağırlıkları var. Bu özellikler daha güçlü bir ağırliğe sahip olduğundan, Personalizer daha yüksek ödüller aldığında mevcut olma eğilimindedirler.

## <a name="getting-feature-importance-evaluation"></a>Özellik önem değerlendirmesini alma

Özellik önem sonuçlarını görmek için bir değerlendirme çalıştırmanız gerekir. Değerlendirme, değerlendirme döneminde gözlenen özellik adlarına göre insan tarafından okunabilir özellik etiketleri oluşturur.

Özellik önemi hakkında ortaya çıkan bilgiler geçerli Personalizer çevrimiçi modelini temsil eder. Değerlendirme, değerlendirme sırasında yapılan tüm eğitimlerden sonra, mevcut çevrimiçi öğrenme politikası ile, değerlendirme döneminin sonunda kaydedilen modelin özelliğinin önemini analiz eder. 

Özellik önem sonuçları, değerlendirme sırasında test edilen veya oluşturulan diğer ilkeleri ve modelleri temsil etmez.  Değerlendirme, değerlendirme süresinin bitiminden sonra Personalizer'a gönderilen özellikleri içermez.

## <a name="how-to-interpret-the-feature-importance-evaluation"></a>Özellik önem değerlendirmesi nasıl yorumlanır?

Personalizer benzer öneme sahip özellikleri "gruplar" oluşturarak özellikleri değerlendirir. Bir grubun diğerlerinden daha güçlü bir öneme sahip olduğu söylenebilir, ancak grup içinde, özelliklerin sıralanması alfabetik olarak dır.

Her Özellik hakkında bilgi şunları içerir:

* Özellik Bağlam veya Eylemler'den mi gelir.
* Özellik Anahtarı ve Değeri.

Örneğin, bir dondurma dükkanı sipariş uygulaması çok önemli bir özellik olarak "Context.Weather:Hot" görebilirsiniz.

Personalizer, birlikte hesaba alındığında daha yüksek ödüller üreten özelliklerin korelasyonlarını görüntüler.

Örneğin, "Context.Weather:Hot *with* Action.MenuItem:IceCream" yanı sıra "Context.Weather:Cold *with* Action.MenuItem:WarmTea:

## <a name="actions-you-can-take-based-on-feature-evaluation"></a>Özellik değerlendirmesine dayalı olarak yapabileceğiniz eylemler

### <a name="imagine-additional-features-you-could-use"></a>Kullanabileceğiniz ek özellikler düşünün

Modeldeki daha önemli özelliklerden ilham alın. Örneğin, bir video mobil uygulamasında "Context.MobileBattery:Low" görürseniz, bağlantı türünün müşterilerin bir video klibi diğerinden fazla görmeyi seçmelerine neden olabileceğini ve ardından uygulamanıza bağlantı türü ve bant genişliği yle ilgili özellikler ekleyebileceğini düşünebilirsiniz.

### <a name="see-what-features-are-not-important"></a>Hangi özelliklerin önemli olmadığını görün

Önemli olmayan özellikleri kaldırma veya kullanımı etkileyebilecek leri daha fazla analiz etmek gibi potansiyel olarak. Özellikler birçok nedenden dolayı düşük sırada yer alabilir. Bir gerçekten özelliği kullanıcı davranışını etkilemez olabilir. Ama aynı zamanda özelliği kullanıcı için belirgin olmadığı anlamına gelebilir. 

Örneğin, bir video sitesi "Action.VideoResolution=4k"ın düşük öneme sahip bir özellik olduğunu ve kullanıcı araştırmasıyla çeliştiğini görebilir. Bunun nedeni, uygulamanın video çözünürlüğünden bahsetmemesi veya göstermemesi olabilir, bu nedenle kullanıcılar bu çözünürlüğe göre davranışlarını değiştirmez.

### <a name="provide-guidance-to-editorial-or-curation-teams"></a>Editoryal veya küratörlük ekiplerine rehberlik sağlayın

Kataloga getirilmeden yeni içerik veya ürünler hakkında rehberlik sağlayın. Personalizer insan içgörü ve ekipleri artıran bir araç olarak tasarlanmıştır. Bunu yapmanın bir yolu, editör gruplarına davranışları yönlendiren ürünler, makaleler veya içerik hakkında ne hakkında bilgi sağlamaktır. Örneğin, video uygulama senaryosu "Action.VideoEntities.Cat:true" adlı önemli bir özellik olduğunu göstererek, editör ekibinin daha fazla kedi videosu getirmesini isteyebilir.

### <a name="troubleshoot-common-problems-and-mistakes"></a>Sık karşılaşılan sorunları ve hataları giderme

Sık karşılaşılan sorunlar ve hatalar, uygulama kodunuzu değiştirerek düzeltilebilir, böylece Personalizer'a uygunsuz veya yanlış biçimlendirilmiş özellikler göndermez. 

Özellikler gönderirken sık yapılan hatalar şunlardır:

* Kişisel olarak tanımlanabilir bilgilerin (PII) gönderilmesi. Personalizer ile bir kişiye özel kişisel bilgiler (ad, telefon numarası, kredi kartı numarası, IP Adresleri gibi) kullanılmamalıdır. Uygulamanızın kullanıcıları izlemesi gerekiyorsa, tanımsız bir UUID veya başka bir UserID numarası kullanın. Çoğu senaryoda bu da sorunludur.
* Çok sayıda kullanıcı yla, her kullanıcının etkileşiminin tüm popülasyonun etkileşiminden daha fazla olması olası değildir, bu nedenle kullanıcı i'lerinin gönderilmesi (kişisel bilgiler içermese bile) büyük olasılıkla modele değerden daha fazla gürültü katacak.
* Tarih-zaman alanlarını, işlenme zaman değerleri yerine kesin zaman damgası olarak gönderme. Context.TimeStamp.Day=Pazartesi veya "Context.TimeStamp.Hour" ="13" gibi özelliklere sahip olmak daha yararlıdır. Her biri için en fazla 7 veya 24 özellik değeri olacaktır. Ama "Context.TimeStamp":"1985-04-12T23:20:50.52Z" o kadar kesin ki bundan bir daha asla olmayacak, çünkü ondan bir daha öğrenmenin bir yolu olmayacak.

## <a name="next-steps"></a>Sonraki adımlar

Personalizer ile [ölçeklenebilirliği ve performansı](concepts-scalability-performance.md) anlayın.

