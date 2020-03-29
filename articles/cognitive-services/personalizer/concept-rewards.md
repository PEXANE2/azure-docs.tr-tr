---
title: Ödül puanı - Personalizer
description: Ödül puanı, kişiselleştirme seçimi Olan RewardActionID'nin kullanıcı için ne kadar iyi sonuçlandığını gösterir. Ödül puanının değeri, kullanıcı davranışı gözlemlerine dayalı olarak işletme mantığınız tarafından belirlenir. Personalizer ödülleri değerlendirerek makine öğrenimi modellerini eğitir.
ms.date: 02/20/2020
ms.topic: conceptual
ms.openlocfilehash: 734e4d0fdcec25884f8535ec61ccd10569fa8890
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79219373"
---
# <a name="reward-scores-indicate-success-of-personalization"></a>Ödül puanları kişiselleştirmenin başarısını gösterir

Ödül puanı, kişiselleştirme seçimi [RewardActionID'nin](https://docs.microsoft.com/rest/api/cognitiveservices/personalizer/rank/rank#response)kullanıcı için ne kadar iyi sonuçlandığını gösterir. Ödül puanının değeri, kullanıcı davranışı gözlemlerine dayalı olarak işletme mantığınız tarafından belirlenir.

Personalizer ödülleri değerlendirerek makine öğrenimi modellerini eğitir.

Personalizer kaynağınız için Azure portalında varsayılan ödül puanını [nasıl yapılandıracağız](how-to-settings.md#configure-rewards-for-the-feedback-loop) öğrenin.

## <a name="use-reward-api-to-send-reward-score-to-personalizer"></a>Personalizer'a ödül puanı göndermek için Ödül API'sini kullanın

Ödüller [Ödül API'sı](https://docs.microsoft.com/rest/api/cognitiveservices/personalizer/events/reward)tarafından Personalizer'a gönderilir. Genellikle ödül 0'dan 1'e kadar olan bir sayıdır. Negatif bir ödül, -1 değeri ile, belirli senaryolarda mümkündür ve sadece takviye öğrenme (RL) ile deneyimli ise kullanılmalıdır. Personalizer zaman içinde mümkün olan en yüksek ödül toplamını elde etmek için modeli eğitir.

Ödüller, kullanıcı davranışı gerçekleştikten sonra gönderilir ve bu da günler sonra olabilir. Personalizer'ın bir etkinliğin ödülü olmadığı veya varsayılan bir ödülün Azure portalındaki [Ödül Bekleme Süresi](#reward-wait-time) ile yapılandırılına kadar bekleyeceği maksimum süre.

Bir etkinliğin ödül puanı **Ödül Bekleme Süresi**içinde alınmamışsa, Varsayılan **Ödül** uygulanır. Genellikle, **[Varsayılan Ödül](how-to-settings.md#configure-reward-settings-for-the-feedback-loop-based-on-use-case)** sıfır olarak yapılandırılır.


## <a name="behaviors-and-data-to-consider-for-rewards"></a>Ödüller için göz önünde bulundurulması gereken davranışlar ve veriler

Ödül puanı bağlamında bu sinyalleri ve davranışları göz önünde bulundurun:

* Seçenekler söz konusu olduğunda öneriler için doğrudan kullanıcı girişi ("X'i mi kastediyorsun?").
* Oturum uzunluğu.
* Seanslar arasındaki zaman.
* Kullanıcının etkileşimlerinin duyarlılık analizi.
* Doğrudan sorular ve bot yararlılık, doğruluk hakkında geribildirim için kullanıcı sorar mini anketler.
* Uyarılara yanıt verme veya uyarılara yanıt verme gecikmesi.

## <a name="composing-reward-scores"></a>Ödül puanları oluşturma

Ödül puanı işletme mantığınızda hesaplanmalıdır. Puan şu şekilde temsil edilebilir:

* Bir kez gönderilen tek bir numara
* Hemen gönderilen bir puan (0,8 gibi) ve daha sonra gönderilen ek bir puan (genellikle 0,2).

## <a name="default-rewards"></a>Varsayılan Ödüller

[Ödül Bekleme Süresi](#reward-wait-time)içinde ödül alınmazsa , Rank çağrısından sonraki süre, Personalizer varsayılan **ödülü** bu Rank etkinliğine dolaylı olarak uygular.

## <a name="building-up-rewards-with-multiple-factors"></a>Birden fazla faktörle ödüller oluşturma

Etkili kişiselleştirme için, birden çok etkene göre ödül puanı oluşturabilirsiniz.

Örneğin, video içeriği listesini kişiselleştirmek için bu kuralları uygulayabilirsiniz:

|Kullanıcı davranışı|Kısmi puan değeri|
|--|--|
|Kullanıcı üst öğeyi tıklattı.|+0.5 ödül|
|Kullanıcı bu öğenin gerçek içeriğini açtı.|+0.3 ödül|
|Kullanıcı 5 dakika içerik veya% 30, hangisi daha uzun izledi.|+0.2 ödül|
|||

Daha sonra toplam ödülü API'ye gönderebilirsiniz.

## <a name="calling-the-reward-api-multiple-times"></a>Ödül API'sini birden çok kez arama

Ayrıca, aynı etkinlik kimliğini kullanarak farklı ödül puanları göndererek Ödül API'sini arayabilirsiniz. Personalizer bu ödülleri aldığında, bu etkinlik için son ödülü, Personalizer yapılandırmasında belirtildiği şekilde toplayarak belirler.

Toplama değerleri:

*  **İlk**: Etkinlik için alınan ilk ödül puanını alır ve geri kalanını atar.
* **Toplam**: EventId için toplanan tüm ödül puanlarını alır ve bir araya getirir.

**Ödül Bekleme Süresi'nden**sonra alınan bir etkinlik için tüm ödüller atılır ve modellerin eğitimini etkilemez.

Ödül puanları ekleyerek, nihai ödülünüz beklenen puan aralığının dışında olabilir. Bu, hizmetin başarısız olduğunu yapmaz.

## <a name="best-practices-for-calculating-reward-score"></a>Ödül puanını hesaplamak için en iyi uygulamalar

* **Başarılı kişiselleştirmenin gerçek göstergelerini göz önünde bulundurun**: Tıklamalar açısından düşünmek kolaydır, ancak iyi bir ödül, kullanıcılarınızın *insanların yapmasını*istediğiniz şey yerine *elde* etmesini istediğiniz şeye dayanır.  Örneğin, tıklamaları ödüllendirmek clickbait eğilimli içerik seçimine yol açabilir.

* **Kişiselleştirme nin ne kadar iyi çalıştığına dair bir ödül puanı kullanın**: Bir film önerisini kişiselleştirmek, kullanıcının filmi izlemesine ve yüksek bir reyting vermesine neden olur. Film reytingi muhtemelen birçok şeye (oyunculuğun kalitesine, kullanıcının ruh haline) bağlı olduğundan, *kişiselleştirmenin* ne kadar iyi çalıştığına dair iyi bir ödül sinyali değildir. Ancak filmin ilk birkaç dakikasını izleyen kullanıcı, kişiselleştirme etkinliğinin daha iyi bir sinyali olabilir ve 5 dakika sonra 1 ödül göndermek daha iyi bir sinyal olacaktır.

* **Ödüller sadece RewardActionID için geçerlidir**: Personalizer, RewardActionID'de belirtilen eylemin etkinliğini anlamak için ödülleri uygular. Diğer eylemleri görüntülemeyi seçerseniz ve kullanıcı bunları tıklatıyorsa, ödül sıfır olmalıdır.

* **İstenmeyen sonuçları göz önünde bulundurun**: [Etik ve sorumlu kullanım](ethics-responsible-use.md)la sorumlu sonuçlara yol açan ödül işlevleri oluşturun.

* **Artımlı Ödülleri Kullanın**: Daha küçük kullanıcı davranışları için kısmi ödüller eklemek Personalizer'ın daha iyi ödüller elde etmesini sağlar. Bu artımlı ödül algoritmanın kullanıcıyı istenen son davranışa yaklaştırmaya yaklaştığını bilmesini sağlar.
    * Bir film listesi gösteriyorsanız, kullanıcı daha fazla bilgi görmek için bir süre ilkinin üzerinde geziniyorsa, bazı kullanıcı etkileşiminin gerçekleştiğini belirleyebilirsiniz. Davranış 0,1 ödül puanı ile sayılabilir.
    * Kullanıcı sayfayı açıp sonra çıktıysa, ödül puanı 0,2 olabilir.

## <a name="reward-wait-time"></a>Ödül bekleme süresi

Personalizer, bir Rank çağrısının bilgilerini, modeli eğitmek için Ödül aramalarında gönderilen ödüllerle ilişkilendirecektir. Bunlar farklı zamanlarda gelebilir. Personalizer, Rank çağrısının etkin olmayan bir olay olarak yapılmış ve daha sonra etkinleştirilse bile, Rank çağrısının gerçekleştiği tarihten itibaren sınırlı bir süre bekler.

Ödül **Bekleme Süresi** sona eriyorsa ve ödül bilgisi yoksa, bu etkinliğe eğitim için varsayılan bir ödül uygulanır. Maksimum bekleme süresi 6 gündür.

## <a name="best-practices-for-reward-wait-time"></a>Ödül bekleme süresi için en iyi uygulamalar

Daha iyi sonuçlar için bu önerileri uygulayın.

* Kullanıcı geri bildirimi almak için yeterli zaman bırakırken Ödül Bekleme Süresini olabildiğince kısa hale getirin.

* Geri bildirim almak için gereken süreden daha kısa bir süre seçmeyin. Örneğin, ödüllerinizden bazıları bir kullanıcı bir videonun 1 dakikasını izledikten sonra gelirse, deneme uzunluğu en az iki katı olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

* [Pekiştirmeye dayalı öğrenme](concepts-reinforcement-learning.md)
* [Rank API'yi deneyin](https://westus2.dev.cognitive.microsoft.com/docs/services/personalizer-api/operations/Rank/console)
* [Ödül API'sini deneyin](https://westus2.dev.cognitive.microsoft.com/docs/services/personalizer-api/operations/Reward)
