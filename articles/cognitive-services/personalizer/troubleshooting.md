---
title: Sorun Giderme - Personalizer
description: Bu makalede, Personalizer hakkında sık sorulan sorun giderme sorularının yanıtlarını içerir.
ms.topic: troubleshooting
ms.date: 02/26/2020
ms.author: diberry
ms.openlocfilehash: 904953f028eb31afe42cf477ac05be43e8b72a4d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80336025"
---
# <a name="personalizer-troubleshooting"></a>Personalizer Sorun Giderme

Bu makalede, Personalizer hakkında sık sorulan sorun giderme sorularının yanıtlarını içerir.

## <a name="transaction-errors"></a>İşlem hataları

<details>
<summary><b>Hizmetten bir HTTP 429 (Çok fazla istek) yanıtı alıyorum. Ne yapabilirim ki?</b></summary>

**Cevap**: Personalizer örneğini oluşturduğunuzda ücretsiz bir fiyat katmanı seçtiyseniz, izin verilen Rank isteklerinin sayısında bir kota sınırı vardır. Rank API için API çağrı oranınızı gözden geçirin (Personalizer kaynağınız için Azure portalındaki Ölçümler bölmesinde) ve çağrı hacminizin seçilen fiyatlandırma katmanı için eşiğin ötesine artması bekleniyorsa fiyatlandırma katmanını (Fiyatlandırma Katmanı bölmesinde) ayarlayın.

</details>

<details>
<summary><b>Ben Rank veya Ödül API'ler bir 5xx hata alıyorum. Ne yapmalıyım?</b></summary>

**Cevap**: Bu hususlar şeffaf olmalıdır. Devam ederlerse, Personalizer kaynağınız için Azure portalında **Destek + sorun giderme** bölümünde **yeni destek isteği** seçerek desteğe başvurun.

</details>

## <a name="learning-loop"></a>Öğrenme döngüsü

<details>
<summary>
<b>Öğrenme döngüsü Personalizer olmadan sisteme% 100 maç elde etmez. Bunu nasıl düzeltebilirim?</b></summary>

**Cevap**: Öğrenme döngüsü ile amacınıza ulaşamamanızın nedenleri:
* Rank API çağrısıyla gönderilen yeterli özellik yok
* Gönderilen özelliklerdeki hatalar - zaman damgaları gibi toplu olmayan özellik verilerinin Rank API'sine gönderilmesi gibi
* Döngü işleme hataları - örneğin ödüller api'sine etkinlikler için ödül verisi göndermeme

Düzeltmek için, döngüye gönderilen özellikleri değiştirerek işlemi değiştirmeniz veya ödülün Rank'ın yanıtının kalitesinin doğru bir değerlendirmesi olduğundan emin olmanız gerekir.

</details>

<details>
<summary>
<b>Öğrenme döngüsü öğrenmek için görünmüyor. Bunu nasıl düzeltebilirim?</b></summary>

**Cevap**: Rank çağrıları etkin bir şekilde öncelik lendirmeden önce öğrenme döngüsübirkaç bin Ödül çağrısına ihtiyaç duyar.

Öğrenme döngünüzün şu anda nasıl davrandığından emin değilseniz, [çevrimdışı bir değerlendirme](concepts-offline-evaluation.md)çalıştırın ve düzeltilmiş öğrenme ilkesini uygulayın.

</details>

<details>
<summary><b>Ben tüm öğeler için tüm aynı olasılıklar ile rütbe sonuçları almaya devam. Personalizer'ın öğrendiğini nasıl bileceğim?</b></summary>

**Cevap**: Personalizer, yeni başladı ve _boş_ bir modele sahip olduğunda veya Personalizer Loop'u sıyrırken ve modeliniz hala **Model güncelleştirme sıklık** döneminiz içinde olduğunda, Sıralama API sonucuyla aynı olasılıkları döndürür.

Yeni güncelleştirme dönemi başladığında, güncelleştirilmiş model kullanılır ve olasılıkların değiştiğini görürsünüz.

</details>

<details>
<summary><b>Öğrenme döngüsü öğrenme ama artık öğrenmek değil gibi görünüyor, ve Rank sonuçlarının kalitesi o kadar iyi değil. Ne yapmalıyım?</b></summary>

**Cevap**:
* Bu Personalizer kaynağı (öğrenme döngüsü) için Azure portalında bir değerlendirmeyi tamamladığınızdan ve uyguladığınıza emin olun.
* Tüm ödüllerin Ödül API'si aracılığıyla gönderildiğinden ve işlendiğinden emin olun.

</details>


<details>
<summary><b>Öğrenme döngüsünün düzenli olarak güncelleştirilmekte olduğunu ve verilerimi puanlamak için kullanıldığını nasıl bilebilirim?</b></summary>

**Cevap**: Modelin en son güncelleştirildiğinde Azure portalının **Model ve Öğrenme Ayarları** sayfasında bulabilirsiniz. Eski bir zaman damgası görürseniz, bunun nedeni büyük olasılıkla Rank ve Reward çağrılarını göndermediğinizdir. Hizmetin gelen verisi yoksa, öğrenmeyi güncelleştirmez. Öğrenme döngüsünün yeterince sık güncellenmediğini görürseniz, döngünün Model **Güncelleştirme sıklığını**güncelleyebilirsiniz.

</details>

## <a name="offline-evaluations"></a>Çevrimdışı değerlendirmeler

<details>
<summary><b>Çevrimdışı değerlendirmenin özelliğinin önemi, yüzlerce veya binlerce öğeiçeren uzun bir liste yi döndürür. Ne oldu?</b></summary>

**Cevap**: Bu genellikle zaman damgaları, kullanıcı iT'leri veya gönderilen diğer bazı ince taneli özellikler nedeniyle.

</details>

<details>
<summary><b>Ben çevrimdışı bir değerlendirme oluşturdu ve neredeyse anında başarılı oldu. Neden bu? Sonuç bulamadım mı?</b></summary>

**Cevap**: Çevrimdışı değerlendirme, o zaman dilimindeki olaylardan elde edilen eğitilen model verilerini kullanır. Değerlendirmenin başlangıç ve bitiş saati arasındaki zaman diliminde herhangi bir veri göndermediyseniz, herhangi bir sonuç olmadan tamamlanacaktır. Personalizer'a gönderildiğini bildiğiniz etkinliklerle bir zaman aralığı seçerek yeni bir çevrimdışı değerlendirme gönderin.

</details>


## <a name="learning-policy"></a>Öğrenme politikası

<details>
<summary><b>Öğrenme politikasını nasıl içerim?</b></summary>

**Cevap**: [Öğrenme politikası kavramları](concept-active-learning.md#understand-learning-policy-settings) ve yeni bir öğrenme politikasının nasıl [uygulanacağı](how-to-manage-model.md) hakkında daha fazla bilgi edinin. Bir öğrenme ilkesi seçmek istemiyorsanız, geçerli olaylarınızı temel alan bir öğrenme ilkesi önermek için [çevrimdışı değerlendirmeyi](how-to-offline-evaluation.md) kullanabilirsiniz.

</details>

## <a name="security"></a>Güvenlik

<details>
<summary><b>Döngümün API anahtarı ele geçirildi. Ne yapabilirim ki?</b></summary>

**Cevap**: Müşterilerinizi diğer anahtarı kullanmak üzere değiştirdikten sonra bir anahtarı yeniden oluşturabilirsiniz. İki tuşa sahip olmak, herhangi bir kapalı kalma süresine sahip olmak zorunda kalmadan anahtarı tembel bir şekilde yaymanızı sağlar. Bunu bir güvenlik önlemi olarak normal bir döngüde yapmanızı öneririz.

</details>

## <a name="next-steps"></a>Sonraki adımlar

[Model güncelleştirme sıklığını yapılandırma](how-to-settings.md#model-update-frequency)