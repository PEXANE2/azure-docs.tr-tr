---
title: Sorun giderme-kişiselleştirici
description: Bu makale, kişiselleştirici hakkında sık sorulan sorun giderme sorularına yanıtlar içerir.
ms.topic: troubleshooting
ms.date: 02/26/2020
ms.author: diberry
ms.openlocfilehash: 904953f028eb31afe42cf477ac05be43e8b72a4d
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80336025"
---
# <a name="personalizer-troubleshooting"></a>Kişiselleştirici sorunlarını giderme

Bu makale, kişiselleştirici hakkında sık sorulan sorun giderme sorularına yanıtlar içerir.

## <a name="transaction-errors"></a>İşlem hataları

<details>
<summary><b>Hizmetten bir HTTP 429 (çok fazla istek) yanıtı alıyorum. Ne yapabilirim?</b></summary>

**Cevap**: kişiselleştirici örneği oluşturduğunuzda ücretsiz bir fiyat katmanı oluşturduysanız, Izin verilen derecelendirme isteği sayısında bir kota sınırı vardır. Derecelendirme API 'SI için API çağrı hızınızı gözden geçirin (kişisel kaynağınız için Azure portal ölçümler bölmesinde) ve çağrı biriminiz seçili fiyatlandırma katmanı eşiğinin ötesinde artması bekleniyorsa fiyatlandırma katmanını (Fiyatlandırma Katmanı bölmesinde) ayarlayın.

</details>

<details>
<summary><b>Derecelendirmek veya yeniden geçirdim API 'Lerinde 5 xx hatası alıyorum. Ne yapmam gerekir?</b></summary>

**Cevap**: Bu sorunlar saydam olmalıdır. Devam ederse, kişiselleştirme kaynağınız için Azure portal **destek + sorun giderme** bölümünde **Yeni destek isteği** ' ni seçerek desteğe başvurun.

</details>

## <a name="learning-loop"></a>Öğrenme döngüsü

<details>
<summary>
<b>Öğrenme döngüsü, kişiselleştirmeden sisteme %100 eşleşme karşılamıyor. Bu Nasıl yaparım? düzeltilsin mi?</b></summary>

**Cevap**: öğrenme döngüsüyle amacınız ile ilgili bilgi verme nedenleri:
* Rank API çağrısıyla gönderilen yeterli özellik yok
* Gönderilen özelliklerde hatalar-derecelendirme API 'sine zaman damgaları gibi toplu olmayan özellik verileri gönderme
* Olay için API 'leri yeniden almak için veri gönderme gibi döngü işleme içeren hatalar

Bu sorunu gidermek için, döngüye gönderilen özellikleri değiştirerek işlemeyi değiştirmeniz veya bir değerlendirmenin, derece yanıtının kalitesi için doğru bir değerlendirme olduğundan emin olmanız gerekir.

</details>

<details>
<summary>
<b>Öğrenme döngüsü öğreniyor gibi görünüyor. Bu Nasıl yaparım? düzeltilsin mi?</b></summary>

**Cevap**: öğrenme döngüsünün, derece çağrıları etkin bir şekilde önceliklendirilmesi için birkaç bin yeniden çağrısı gerekir.

Öğrenme döngünüz Şu anda çalışır durumda olduğundan emin değilseniz, [çevrimdışı bir değerlendirme](concepts-offline-evaluation.md)çalıştırın ve düzeltilen öğrenme ilkesini uygulayın.

</details>

<details>
<summary><b>Tüm öğeler için aynı olasılıkların bulunduğu derece sonuçları almaya devam ediyorum. Kişiselleştirici öğreneceğimizi Nasıl yaparım? mı?</b></summary>

**Cevap**: kişiselleştirici, yeni başlayan ve _boş_ bir MODELI olan bir derece API sonucu içindeki aynı olasılıkların sonucunu döndürür ya da kişiselleştirici döngüsünü sıfırladığınızda ve modeliniz hala **model güncelleştirme sıklığı** döneminizin içindeyse.

Yeni güncelleştirme dönemi başladığında, güncelleştirilmiş model kullanılır ve bu da olasılıkların değişikliğini görürsünüz.

</details>

<details>
<summary><b>Öğrenme döngüsü öğreniyor ancak artık bilgi vermiyor gibi görünüyor ve sıra sonuçlarının kalitesi iyi değil. Ne yapmam gerekir?</b></summary>

**Cevap**:
* Bu kişiselleştirici kaynak (öğrenme döngüsü) için Azure portal bir değerlendirmeyi tamamladığınızdan ve uyguladığınızdan emin olun.
* Tüm yeniden temeller, Reward API aracılığıyla gönderildiğinden ve işlendiğinizden emin olun.

</details>


<details>
<summary><b>Nasıl yaparım? öğrenme döngüsünün düzenli olarak güncelleştirildiğini ve verilerime puan vermek için kullanıldığını öğrenmek mi istiyorsunuz?</b></summary>

**Cevap**: Azure Portal model **ve öğrenme ayarları** sayfasında modelin en son ne zaman güncelleştirildiği saati bulabilirsiniz. Eski bir zaman damgası görürseniz, bu durum büyük olasılıkla derecelendirme ve yeniden çağrıları göndermemelidir. Hizmetin gelen verileri yoksa Öğrenimini güncelleştirmez. Öğrenme döngüsünün yeterince sık güncelleştirilmediğinden, döngünün **model güncelleştirme sıklığını**düzenleyebilirsiniz.

</details>

## <a name="offline-evaluations"></a>Çevrimdışı değerlendirmeler

<details>
<summary><b>Çevrimdışı değerlendirme özelliğinin önem derecesi yüzlerce veya binlerce öğe içeren uzun bir liste döndürür. Ne oldu?</b></summary>

**Cevap**: Bu, genellikle zaman damgalarının, kullanıcı kimliklerinin veya diğer bazı ayrıntılı özelliklerden kaynaklanır.

</details>

<details>
<summary><b>Çevrimdışı bir değerlendirme oluşturdum ve neredeyse anında başarılı oldu. Neden? Hiç sonuç görmüyorum mi?</b></summary>

**Cevap**: çevrimdışı değerlendirme, bu dönemdeki olaylardaki eğitilen model verilerini kullanır. Değerlendirmenin başlangıç ve bitiş saati arasındaki zaman diliminde herhangi bir veri göndermediyseniz, bu işlem herhangi bir sonuç olmadan tamamlanır. Kişiselleştirmede bildiğiniz bir olay ile zaman aralığı seçerek yeni bir çevrimdışı değerlendirme gönderebilirsiniz.

</details>


## <a name="learning-policy"></a>Öğrenme ilkesi

<details>
<summary><b>Bir öğrenme ilkesini içeri Nasıl yaparım? mı?</b></summary>

**Cevap**: [öğrenme ilkesi kavramları](concept-active-learning.md#understand-learning-policy-settings) ve yeni bir öğrenme ilkesinin [nasıl uygulanacağı](how-to-manage-model.md) hakkında daha fazla bilgi edinin. Bir öğrenme ilkesi seçmek istemiyorsanız, geçerli olaylarınız temelinde bir öğrenme ilkesi önermek için [çevrimdışı değerlendirmeyi](how-to-offline-evaluation.md) kullanabilirsiniz.

</details>

## <a name="security"></a>Güvenlik

<details>
<summary><b>Döngülerimin API anahtarı tehlikeye geçti. Ne yapabilirim?</b></summary>

**Cevap**: istemcilerinizi diğer anahtarı kullanacak şekilde değiştirdikten sonra bir anahtarı yeniden oluşturabilirsiniz. İki anahtara sahip olmak, herhangi bir kesinti olması gerekmeden anahtarı geç bir biçimde yayalmanıza olanak sağlar. Bunu bir güvenlik önlemi olarak düzenli bir döngüde yapmanızı öneririz.

</details>

## <a name="next-steps"></a>Sonraki adımlar

[Model güncelleştirme sıklığını yapılandırma](how-to-settings.md#model-update-frequency)