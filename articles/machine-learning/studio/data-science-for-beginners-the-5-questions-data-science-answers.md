---
title: Yeni Başlayanlar için Veri Bilimleri
titleSuffix: ML Studio (classic) - Azure
description: Yeni Başlayanlar Için Veri Bilimi, 5 Sorulu Veri Bilimi Cevapları ile başlayan 5 kısa videoda temel kavramları öğretir. Azure Machine Learning'den.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: sdgilley
ms.author: sgilley
ms.custom: seodec18
ms.date: 03/22/2019
ms.openlocfilehash: af0611e121228a587e159bd4e6529c807132d16d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79204452"
---
# <a name="data-science-for-beginners-video-1-the-5-questions-data-science-answers"></a>Yeni Başlayanlar için Veri Bilimi video 1: Veri bilimiyle ilgili 5 sorunun yanıtı
En iyi veri bilimcisinden beş kısa videoda *Yeni Başlayanlar Için Veri Bilimi'nden* veri bilimine hızlı bir giriş alın. Bu videolar temel ama yararlıdır, ister veri bilimi yapmak la ilgileniyor olun ister veri bilimcilerle çalışın.

Bu ilk video, veri biliminin cevaplayabileceği soru türleri hakkındadır. Serinin en iyi şekilde elde etmek için, hepsini izleyin. [Videolar listesine gidin](#other-videos-in-this-series)
<br>

> [!VIDEO https://channel9.msdn.com/Shows/SupervisionNotRequired/8/player]
>
>

## <a name="other-videos-in-this-series"></a>Bu serinin diğer videoları
*Yeni Başlayanlar için Veri Bilimi,* toplam da yaklaşık 25 dakika alan veri bilimine hızlı bir giriştir. Beş videonun tümüne de göz atın:

* Video 1: Veri bilimi tarafından cevaplanın 5 soru
* Video 2: [Verileriniz veri bilimine hazır mı?](data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 dk 56 sn)*
* Video 3: [Verilerle cevaplanabileceğiniz bir soru sorun](data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 dk 17 sn)*
* Video 4: [Basit bir modelle bir yanıtı tahmin edin](data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 dk 42 sn)*
* Video 5: [Veri bilimi yapmak için diğer kişilerin çalışmalarını kopyalayın](data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 dk 18 sn)*

## <a name="transcript-the-5-questions-data-science-answers"></a>Transkript: Veri bilimi cevaplarını 5 soru
Merhaba! *Yeni başlayanlar için*video serisi Data Science hoş geldiniz.

Veri Bilimi korkutucu olabilir, bu yüzden burada herhangi bir denklem veya bilgisayar programlama jargon olmadan temelleri tanıtacağım.

Bu ilk videoda, "Veri biliminin verdiği 5 soru" hakkında konuşacağız.

Veri Bilimi, soruların yanıtlarını tahmin etmek için sayılar ve adlar (kategoriler veya etiketler olarak da bilinir) kullanır.

Sizi şaşırtabilir, ancak *veri biliminin yanıtverdiği sadece beş soru vardır:*

* Bu A mı B mi?
* Bu garip mi?
* Ne kadar - veya - Kaç?
* Bu nasıl organize oluyor?
* Uygulamam gereken sonraki adım nedir?

Bu soruların her biri, algoritma adı verilen ayrı bir makine öğrenimi yöntemi ailesi tarafından yanıtlanır.

Bir algoritmayı bir tarif olarak düşünmek ve verilerinizi de malzemeler olarak düşünmek yararlıdır. Bir algoritma, bir yanıt almak için verilerin nasıl birleştirilip karıştırılacağını söyler. Bilgisayarlar blender gibidir. Onlar sizin için algoritmanın zor iş çoğu yapmak ve oldukça hızlı yapmak.

## <a name="question-1-is-this-a-or-b-uses-classification-algorithms"></a>Soru 1: Bu A mı B mi? sınıflandırma algoritmaları kullanır
Şu soruyla başlayalım: Bu A mı B mi?

![Sınıflandırma algoritmaları: Bu A mı B mi?](./media/data-science-for-beginners-the-5-questions-data-science-answers/classification-algorithms.png)

Bu algoritma ailesine iki sınıflı sınıflandırma denir.

Sadece iki olası cevabı olan herhangi bir soru için yararlıdır.

Örnek:

* Bu lastik önümüzdeki 1000 mil içinde başarısız olacak: Evet ya da hayır?
* Hangi daha fazla müşteri getiriyor: 5 $ kupon veya% 25 indirim?

Bu soru aynı zamanda ikiden fazla seçenek içerecek şekilde yeniden ifade edilebilir: Bu A veya B veya C veya D, vb mi?  Bu çok sınıflı sınıflandırma denir ve birkaç -veya birkaç bin - olası yanıtlar olduğunda yararlıdır. Çok sınıflı sınıflandırma en olası olanı seçer.

## <a name="question-2-is-this-weird-uses-anomaly-detection-algorithms"></a>Soru 2: Bu garip mi? anomali algılama algoritmaları kullanır
Veri biliminin cevaplayabileceği bir sonraki soru şu: Bu garip mi? Bu soru, anomali tespiti adı verilen bir algoritma ailesi tarafından yanıtlanır.

![Anomali Algılama algoritmaları: Bu garip mi?](./media/data-science-for-beginners-the-5-questions-data-science-answers/anomaly-detection-algorithms.png)

Kredi kartınız varsa, anomali tespitinden zaten yararlanmışsınızdır. Kredi kartı şirketiniz satın alma alışkanlıklarınızı analiz eder, böylece sizi olası sahtekarlığa karşı uyarabilirler. "Garip" olan ücretler, normalde alışveriş yapmadığınız veya alışılmadık fiyatlı bir ürün satın almadığınız bir mağazadan satın alma işlemi olabilir.

Bu soru birçok yönden yararlı olabilir. Örneğin:

* Basınç ölçerleri olan bir arabanız varsa, şunu bilmek isteyebilirsiniz: Bu basınç göstergesi normal mi?
* Eğer internet izliyorsanız, bilmek istiyorum: Bu mesaj internetten tipik mi?

Anomali algılama, beklenmeyen veya olağandışı olaylar veya davranışlar alabını işaretler. Nerede sorunları aramak için ipuçları verir.

## <a name="question-3-how-much-or-how-many-uses-regression-algorithms"></a>Soru 3: Ne kadar? ya da kaç tane? regresyon algoritmaları kullanır
Makine öğrenimi de ne kadar cevap tahmin edebilirsiniz? ya da kaç tane? Bu soruyu yanıtlayan algoritma ailesine regresyon denir.

![Regresyon algoritmaları: Ne kadar? ya da kaç tane?](./media/data-science-for-beginners-the-5-questions-data-science-answers/regression-algorithms.png)

Regresyon algoritmaları gibi sayısal tahminlerde bulunur:

* Sıcaklık önümüzdeki Salı ne kadar olacak?  
* Dördüncü çeyrek satışlarım ne olacak?

Onlar bir sayı için sorar herhangi bir soruya cevap yardımcı olur.

## <a name="question-4-how-is-this-organized-uses-clustering-algorithms"></a>Soru 4: Bu nasıl organize edilir? kümeleme algoritmaları kullanır
Şimdi son iki soru biraz daha gelişmiş.

Bazen bir veri kümesinin yapısını anlamak istersiniz - Bu nasıl düzenlenir? Bu soru için, zaten sonuçları bildiğiniz örnekler yok.

Verilerin yapısını kızdırmanın birçok yolu vardır. Bir yaklaşım kümeleme olduğunu. Daha kolay yorumlanması için verileri doğal "kümelere" ayırır. Kümeleme ile, kimse doğru cevap yoktur.

![Kümeleme algoritmaları: Bu nasıl düzenlenir?](./media/data-science-for-beginners-the-5-questions-data-science-answers/clustering-algorithms.png)

Kümeleme sorularının yaygın örnekleri şunlardır:

* Hangi izleyiciler aynı tür filmleri sever?
* Hangi yazıcı modelleri aynı şekilde başarısız olur?

Verilerin nasıl düzenlendiğini anlayarak, davranışları ve olayları daha iyi anlayabilir ve tahmin edebilirsiniz.  

## <a name="question-5-what-should-i-do-now-uses-reinforcement-learning-algorithms"></a>Soru 5: Şimdi ne yapmalıyım? güçlendirme öğrenme algoritmaları kullanır
Son soru - Şimdi ne yapmalıyım? – takviye öğrenme denilen algoritmalar bir aile kullanır.

Güçlendirme öğrenme fareler ve insanların beyinleri ceza ve ödüller yanıt nasıl esinlenerek oldu. Bu algoritmalar sonuçlardan öğrenir ve bir sonraki eyleme karar verir.

Tipik olarak, takviye öğrenme insan rehberlik olmadan küçük kararlar çok yapmak zorunda otomatik sistemler için iyi bir uyum olduğunu.

![Pekiştirme Öğrenme algoritmaları: Bundan sonra ne yapmalıyım?](./media/data-science-for-beginners-the-5-questions-data-science-answers/reinforcement-learning-algorithms.png)

Cevaplar sorular her zaman ne eylem alınmalıdır hakkında - genellikle bir makine veya bir robot tarafından. Örnekler şunlardır:

* Ben bir ev için bir sıcaklık kontrol sistemi iseniz: Sıcaklık ayarlayın ya da olduğu yerde bırakın?  
* Ben kendi kendine giden bir araba iseniz: Sarı ışıkta, fren veya hızlandırmak?  
* Bir robot vakum için: Vakumtutmak, ya da şarj istasyonuna geri dönmek?

Pekiştirme öğrenme algoritmaları, deneme yanılma dan öğrenme, gittikleri gibi veri toplamak.

İşte bu kadar - Veri biliminin cevaplayabileceği 5 soru.

## <a name="next-steps"></a>Sonraki adımlar
* [Machine Learning Studio (klasik) ile ilk veri bilimi deneyin](create-experiment.md)
* [Microsoft Azure'da Machine Learning'e giriş alın](/azure/machine-learning/preview/overview-what-is-azure-ml)
