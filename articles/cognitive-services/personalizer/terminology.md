---
title: Terminoloji-kişiselleştirici
description: Kişiselleştirici, pekiştirmeye dayalı Learning 'teki terminolojiyi kullanır. Bu terimler Azure portal ve API 'lerde kullanılır.
ms.topic: conceptual
ms.date: 02/18/2020
ms.openlocfilehash: f75437c5afd5d3fd7f7570079be410d3db1ca8db
ms.sourcegitcommit: 5a71ec1a28da2d6ede03b3128126e0531ce4387d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77624280"
---
# <a name="terminology"></a>Terminoloji

Kişiselleştirici, pekiştirmeye dayalı Learning 'teki terminolojiyi kullanır. Bu terimler Azure portal ve API 'lerde kullanılır.

## <a name="conceptual-terminology"></a>Kavramsal terminoloji

* **Öğrenme döngüsü**: uygulamanızın, kişiselleştirmeye faydalanabilir her bölümü için, _öğrenme döngüsü_olarak adlandırılan bir kişiselleştirici kaynağı oluşturursunuz. Kişiselleştirmek için birden fazla deneyim varsa, her biri için bir döngü oluşturun.

* **Model**: bir kişiselleştirici modeli, Kullanıcı davranışı hakkında öğrenilen tüm verileri yakalar, daha fazla bilgi almak için göndereceğiniz bağımsız değişkenlerin birleşiminden eğitim verileri alma ve öğrenme ilkesi tarafından belirlenen bir eğitim davranışı.

## <a name="personalizer-configuration"></a>Kişiselleştirici yapılandırması

Kişiselleştirici [Azure Portal](https://portal.azure.com)bir şekilde yapılandırılmıştır.

* **Ödül**: yeniden geçen bekleme süresi, varsayılan değer ve ödül toplama ilkesi için varsayılan değerleri yapılandırın.

* **Araştırma**: araştırma Için kullanılacak derece çağrılarının yüzdesini yapılandırma

* **Model güncelleştirme sıklığı**: modelin ne sıklıkta geri çekilmesi.

* **Veri saklama**: kaç gün değer depolayacak veri. Bu, öğrenme döngünüzü geliştirmek için kullanılan çevrimdışı değerlendirmeleri etkileyebilir.

## <a name="use-rank-and-reward-apis"></a>Derecelendirme ve Reward API 'Leri kullanma

* **Derece**: Özellikler ve bağlam özellikleriyle eylemler verildiğinde, en üstteki eylemi (içerik öğesi) döndürmek için keşfet veya Exploit kullanın.

    * **Eylemler**: Eylemler, ürün veya promosyon gibi, aralarından seçim yapabileceğiniz içerik öğeleridir. Kişiselleştirme API 'SI aracılığıyla kullanıcılarınıza göstermek için, kişiselleştirici, en iyi eylemi (döndürülen geri dönüş eylem KIMLIĞI) seçer.

    * **Bağlam**: daha doğru bir derece sağlamak için, içeriğiniz hakkında bilgi sağlayın, örneğin:
        * Kullanıcı.
        * Bulundukları cihaz.
        * Geçerli saat.
        * Geçerli durumla ilgili diğer veriler.
        * Kullanıcı veya bağlamla ilgili geçmiş verileri.

        Özel uygulamanız farklı bağlam bilgilerine sahip olabilir.

    * **[Özellikler](concepts-features.md)** : bir içerik öğesi veya Kullanıcı bağlamı hakkındaki bilgi birimi. Yalnızca toplanmış özellikleri kullandığınızdan emin olun. Belirli zamanları, Kullanıcı kimliklerini veya diğer toplu olmayan verileri özellik olarak kullanmayın.

        * Bir _eylem özelliği_ , içerik hakkında meta verilerdir.
        * _Bağlam özelliği_ , içeriğin sunulduğu bağlamla ilgili meta verilerdir.

* **Araştırma**: kişiselleştirici hizmeti, en iyi eylemi döndürmek yerine, Kullanıcı için farklı bir eylem seçerse, ne zaman araştırma yapılır. Kişiselleştirici hizmeti, ve ' yi inceleyerek devam eden kullanıcı davranışına uyum sağlayabilir.

* **Yararlanma**: kişiselleştirici hizmeti, geçmiş verilere göre en iyi eyleme karar vermek için geçerli modeli kullanır.

* **Deneme süresi**: Bu etkinlik için sıralama çağrısının başladığı andan Itibaren, kişiselleştirici hizmetin bir azalma için beklediği süre miktarı.

* **Etkin olmayan olaylar**: etkin olmayan bir olay, derece olarak adlandırduğunuz bir olaydır, ancak istemci uygulama kararlarından dolayı kullanıcının sonucu göreceğinden emin olmadığınızı unutmayın. Etkin olmayan olaylar, kişiselleştirme sonuçları oluşturup depolamanıza olanak tanır ve ardından makine öğrenimi modelini etkilemeden daha sonra atmayı seçebilirsiniz.


* **Ödül**: kullanıcının sıralama API 'si tarafından döndürülen geri alma eylemi kimliğine nasıl yanıt verdiğini gösteren bir ölçü, 0 ile 1 arasında bir puan olarak. 0-1 değeri, iş mantığınızla belirlenir ve bu seçenek, nasıl kişiselleştirmenin iş hedeflerine nasıl ulaştığından yardımcı olur. Öğrenme döngüsü, bu yeniden Kullanıcı Geçmişi olarak depolamaz.

## <a name="offline-evaluations"></a>Çevrimdışı değerlendirmeler

* **Değerlendirme**: çevrimdışı bir değerlendirme, döngünüz için en iyi öğrenme ilkesini döngünün verilerine göre belirler.

* **Öğrenme ilkesi**: her olay üzerinde kişiselleştirici, makine öğrenimi algoritmasının nasıl çalıştığını etkileyen bazı parametrelere göre belirlenir. Yeni bir öğrenme döngüsü, varsayılan bir **öğrenme ilkesiyle**başlar ve bu, orta düzeyde performans sağlayabilir. [Değerlendirme](concepts-offline-evaluation.md)çalıştırılırken, kişiselleştirici, özel olarak döngülerinizin kullanım örneklerine iyileştirilmiş yeni öğrenme ilkeleri oluşturur. Kişiselleştirmede, değerlendirme sırasında oluşturulan her bir döngü için en iyi duruma getirilmiş ilkelerle önemli ölçüde daha iyi işlem yapılır. Öğrenme ilkesi, Azure portal ' deki kişiselleştirici kaynak için **model ve öğrenme ayarları** ' nda _öğrenme ayarları_ olarak adlandırılır.