---
title: Terminoloji - Personalizer
description: Personalizer takviye öğrenme terminolojisi kullanır. Bu terimler Azure portalında ve API'lerde kullanılır.
ms.topic: conceptual
ms.date: 02/18/2020
ms.openlocfilehash: f75437c5afd5d3fd7f7570079be410d3db1ca8db
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77624280"
---
# <a name="terminology"></a>Terminoloji

Personalizer takviye öğrenme terminolojisi kullanır. Bu terimler Azure portalında ve API'lerde kullanılır.

## <a name="conceptual-terminology"></a>Kavramsal terminoloji

* **Öğrenme Döngüsü**: Uygulamanızın kişiselleştirmeden yararlanabilecek her bölümü için _öğrenme döngüsü_adı verilen bir Kiskizer kaynağı oluşturursunuz. Kişiselleştirmek için birden fazla deneyiminiz varsa, her biri için bir döngü oluşturun.

* **Model**: Personalizer modeli, kullanıcı davranışı hakkında öğrenilen tüm verileri yakalar, Sıralama ve Ödül çağrılarına gönderdiğiniz bağımsız değişkenlerin birleşiminden ve Öğrenme Politikası tarafından belirlenen bir eğitim davranışıyla eğitim verilerini alır.

## <a name="personalizer-configuration"></a>Personalizer yapılandırması

Personalizer [Azure portalından](https://portal.azure.com)yapılandırılır.

* **Ödüller**: ödül bekleme süresi, varsayılan ödül ve ödül toplama ilkesi için varsayılan değerleri yapılandırın.

* **Keşif**: keşif için kullanılacak Rank çağrılarının yüzdesini yapılandırma

* **Model güncelleştirme sıklığı**: Modelin ne sıklıkta yeniden eğitildiği.

* **Veri saklama**: Depolanması gereken veri sayısı kaç gündür. Bu, öğrenme döngünüzün iyileştirilmesi için kullanılan çevrimdışı değerlendirmeleri etkileyebilir.

## <a name="use-rank-and-reward-apis"></a>Sıralama ve Ödül API'lerini kullanma

* **Sıralama**: Özelliklere ve bağlam özelliklerine sahip eylemler göz önüne alındığında, en üstteki eylemi (içerik öğesi) döndürmek için keşfet veya yararlanma özelliğini kullanın.

    * **Eylemler**: Eylemler, ürün veya promosyonlar gibi içerik öğelerinin aralarından seçim yapIlebilmesidir. Personalizer, Rank API üzerinden kullanıcılarınıza göstermek için en iyi eylemi (döndürülen ödül eylem kimliği) seçer.

    * **Bağlam**: Daha doğru bir sıralama sağlamak için, bağlamınız hakkında bilgi sağlayın, örneğin:
        * Kullanıcınız.
        * Kullandıkları cihaz.
        * Geçerli saat.
        * Mevcut durumla ilgili diğer veriler.
        * Kullanıcı veya bağlam hakkında geçmiş veriler.

        Özel uygulamanız farklı bağlam bilgilerine sahip olabilir.

    * **[Özellikler](concepts-features.md)**: İçerik öğesi veya kullanıcı bağlamı hakkında bilgi birimidir. Yalnızca toplanan özellikleri kullandığınızdan emin olun. Belirli süreleri, kullanıcı adlarını veya diğer toplu olmayan verileri özellik olarak kullanmayın.

        * _Eylem özelliği,_ içerikle ilgili meta verilerdir.
        * _Bağlam özelliği,_ içeriğin sunulduğu bağlam la ilgili meta verilerdir.

* **Keşif**: Personalizer hizmeti, en iyi eylemi döndürmek yerine, kullanıcı için farklı bir eylem seçtiğinde bunu keşfediyor. Personalizer hizmeti sürüklenme, durgunluk önler ve keşfederek devam eden kullanıcı davranışına adapte olabilir.

* **Yararlanma**: Personalizer hizmeti, geçmiş verilere dayanarak en iyi eylemi karar vermek için geçerli modeli kullanır.

* **Deneme Süresi**: Personalizer hizmetinin ödül beklediği süre, o etkinlik için Rank çağrısının gerçekleştiği andan itibaren.

* **Etkin Olmayan Olaylar**: Etkin olmayan bir olay, Rank'ı aradığınız bir olaydır, ancak istemci uygulama kararları nedeniyle kullanıcının sonucu göreceğinden emin olmazsınız. Etkin olmayan olaylar, kişiselleştirme sonuçları oluşturmanıza ve depolamanıza olanak sağlar, ardından makine öğrenme modelini etkilemeden bunları daha sonra atmaya karar verir.


* **Ödül**: Kullanıcının Rank API'nin iade edilen ödül eylem kimliğine 0 ile 1 arasında bir puan olarak nasıl yanıt verdiklerinin ölçüsüdür. 0'dan 1'e kadar olan değer, seçimin kişiselleştirme iş hedeflerinize ulaşmasına nasıl yardımcı olduğuna bağlı olarak iş mantığınız tarafından ayarlanır. Öğrenme döngüsü bu ödülü bireysel kullanıcı geçmişi olarak saklamaz.

## <a name="offline-evaluations"></a>Çevrimdışı değerlendirmeler

* **Değerlendirme**: Çevrimdışı değerlendirme, döngünüzün verilerine dayanarak döngünüz için en iyi öğrenme ilkesini belirler.

* **Öğrenme Politikası**: Personalizer'ın her olay da bir modeli nasıl eğittiği, makine öğrenme algoritmasının nasıl çalıştığını etkileyen bazı parametrelerle belirlenecektir. Yeni bir öğrenme döngüsü, orta düzeyde performans sağlayabilen varsayılan bir **Öğrenme Politikası**ile başlar. [Değerlendirmeleri](concepts-offline-evaluation.md)çalıştırırken, Personalizer döngünüzün kullanım durumlarına özel olarak optimize edilmiş yeni öğrenme ilkeleri oluşturur. Personalizer, Değerlendirme sırasında oluşturulan her belirli döngü için optimize edilmiş ilkelerle önemli ölçüde daha iyi performans gösterir. Öğrenme ilkesi, Azure portalındaki Personalizer kaynağının **Model'deki** _öğrenme ayarları_ ve öğrenme ayarları olarak adlandırılır.