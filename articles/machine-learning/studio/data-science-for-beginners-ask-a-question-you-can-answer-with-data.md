---
title: Soru sor verisi yanıtlayabilir
titleSuffix: ML Studio (classic) - Azure
description: Yeni Başlayanlar için Veri Bilimi video 3'te keskin bir veri bilimi sorusunu nasıl formüle edebilirsiniz öğrenin. Sınıflandırma ve regresyon sorularının karşılaştırması içerir.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: sdgilley
ms.author: sgilley
ms.date: 03/22/2019
ms.openlocfilehash: 26837337b49d79a26404fd6709b036f6907720f8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73838849"
---
# <a name="ask-a-question-you-can-answer-with-data"></a>Verilerle yanıtlayabileceğiniz bir soru sorun
## <a name="video-3-data-science-for-beginners-series"></a>Video 3: Yeni Başlayanlar serisi için Veri Bilimi
Yeni Başlayanlar için Veri Bilimi video 3'te bir veri bilimi problemini bir soruya nasıl formüle edebilirsiniz öğrenin. Bu video sınıflandırma ve regresyon algoritmaları için soruların bir karşılaştırma içerir.

Serinin en iyi şekilde elde etmek için, hepsini izleyin. [Videolar listesine gidin](#other-videos-in-this-series)
<br>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Data-science-for-beginners-Ask-a-question-you-can-answer-with-data/player]
>
>

## <a name="other-videos-in-this-series"></a>Bu serinin diğer videoları
*Yeni Başlayanlar için Veri Bilimi* beş kısa video veri bilimine hızlı bir giriştir.

* Video 1: [Veri bilimi cevapları 5 soru](data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 dk 14 sn)*
* Video 2: [Verileriniz veri bilimine hazır mı?](data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 dk 56 sn)*
* Video 3: Verilerle yanıtlayabildiğiniz bir soru sorun
* Video 4: [Basit bir modelle bir yanıtı tahmin edin](data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 dk 42 sn)*
* Video 5: [Veri bilimi yapmak için diğer kişilerin çalışmalarını kopyalayın](data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 dk 18 sn)*

## <a name="transcript-ask-a-question-you-can-answer-with-data"></a>Transkript: Verilerle yanıtlayabileceğiniz bir soru sorun
"Yeni Başlayanlar İçin Veri Bilimi" serisinin üçüncü videosuna hoş geldiniz.  

Bu nda, verilerle yanıtlanabileceğiniz bir soruyu formüle etmek için bazı ipuçları alırsınız.

Bu serinin önceki iki videosunu ilk olarak izlerseniz bu videodan daha fazla bilgi alabilirsiniz: "Veri biliminin yanıtlayabileceği 5 soru" ve "Verileriniz veri bilimine hazır mı?"

## <a name="ask-a-sharp-question"></a>Keskin bir soru sorun
Veri biliminin bir soruya yanıtı tahmin etmek için adları (kategoriler veya etiketler olarak da adlandırılır) ve sayıları kullanma işlemi olduğundan bahsettik. Ama herhangi bir soru olamaz; Bu keskin bir *soru olmalı.*

Belirsiz bir sorunun bir ad veya numarayla yanıtlanması gerekir. Keskin bir soru olmalı.

İstediğin soruya doğru cevap verecek bir cin olan sihirli bir lamba bulduğunu hayal et. Ama bu yaramaz bir cin, cevaplarını olabildiğince belirsiz ve kafa karıştırıcı yapmaya çalışacak. Onları o kadar hava geçirmez bir soruyla suçlamak istiyorsun ki bilmek istediklerini söylemekten başka bir şey söyleyebiliyorlar.

"Hisselerime ne olacak?" gibi belirsiz bir soru soracak olursanız, cin "Fiyat değişecek" diye cevap verebilir. Bu doğru bir cevap, ama pek yardımcı olmuyor.

Ama "Hisse senedimin satış fiyatı gelecek hafta ne olacak?" gibi keskin bir soru soracak olursanız, cin size özel bir cevap vermekten ve bir satış fiyatı tahmin etmekten başka bir şey yapamaz.

## <a name="examples-of-your-answer-target-data"></a>Yanıtınızın örnekleri: Hedef veriler
Sorunuz formüle edildikten sonra, verilerinizde yanıt örnekleri olup olmadığını kontrol edin.

Eğer sorumuz "Hisse senedimin satış fiyatı gelecek hafta ne olacak?" ise. o zaman verilerimizin hisse senedi fiyat geçmişini içerdiğinden emin olmalıyız.

Eğer sorumuz "Filomdaki hangi araba önce başarısız olacak?" ise. daha sonra, verilerimizin önceki hatalar la ilgili bilgileri içerdiğinden emin olmalıyız.

![Hedef veriler - yanıt örnekleri. Veri bilimi sorusunu formüle edin.](./media/data-science-for-beginners-ask-a-question-you-can-answer-with-data/target-data.png)

Bu yanıt örneklerine hedef denir. Hedef, ister bir kategori ister bir sayı olsun, gelecekteki veri noktaları hakkında tahmin etmeye çalıştığımız şeydir.

Herhangi bir hedef veriniz yoksa, biraz almanız gerekir. Soruna onsuz cevap veremezsin.

## <a name="reformulate-your-question"></a>Sorunuz yeniden formüle edin
Bazen daha yararlı bir yanıt almak için sorunuz yeniden kelime olabilir.

"Bu veri noktası A mı b mi?" sorusu. bir şeyin kategorisini (veya adını veya etiketini) tahmin eder. Cevap lamak için, bir *sınıflandırma algoritması*kullanıyoruz.

"Ne kadar?" sorusu. ya da "Kaç tane?" bir miktar tahmin eder. Cevap lamak için bir *regresyon algoritması*kullanıyoruz.

Bunları nasıl dönüştürebileceğimizi görmek için şu soruya bakalım: "Bu okuyucu için en ilginç haber hangisi?" Birçok olasılıktek bir seçim bir tahmin ister - başka bir deyişle "Bu A veya B veya C veya D mi?" - ve bir sınıflandırma algoritması kullanırdı.

Ama, bu soruyu cevaplamak daha kolay olabilir eğer "Ne kadar ilginç bu okuyucu için bu listedeki her hikaye?" olarak yeniden word? Şimdi her makaleye sayısal bir puan verebilir ve ardından en yüksek puanalan makaleyi belirlemek kolaydır. Bu bir regresyon sorusuna sınıflandırma sorusunun bir rephrasing veya Ne kadar?

![Sorunuz yeniden formüle edin. Sınıflandırma sorusu ve regresyon sorusu.](./media/data-science-for-beginners-ask-a-question-you-can-answer-with-data/classification-question-vs-regression-question.png)

Nasıl soru sorduğunuz, hangi algoritmanın size bir cevap verebileceğine dair bir ipucudur.

Bazı algoritma ailelerinin - haber hikaye örneğimizdeki gibi - yakından ilişkili olduğunu göreceksiniz. Size en yararlı yanıtı veren algoritmayı kullanmak için sorunuzda yeniden formüle edebilirsiniz.

Ama, en önemlisi, bu keskin soru sormak - veri ile cevaplayabilirsiniz soru. Ve bunu cevaplamak için doğru verilere sahip olduğundan emin olun.

Verilerle cevaplayabileceğiniz bir soru sormak için bazı temel ilkeler hakkında konuştuk.

Microsoft Azure Machine Learning Studio'nun (klasik) "Yeni Başlayanlar Için Veri Bilimi"ndeki diğer videolara göz atmayı unutmayın.

## <a name="next-steps"></a>Sonraki adımlar
* [Machine Learning Studio (klasik) ile ilk veri bilimi deneyin](create-experiment.md)
* [Microsoft Azure'da Machine Learning'e giriş alın](/azure/machine-learning/preview/overview-what-is-azure-ml)
