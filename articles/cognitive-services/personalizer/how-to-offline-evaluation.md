---
title: Çevrimdışı değerlendirme-kişiselleştirici
titleSuffix: Azure Cognitive Services
description: Çevrimdışı değerlendirmede öğrenme döngünüzü çözümlemeyi öğrenin
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: f14403422e2c783d75634bb929d8c2130bd505b6
ms.sourcegitcommit: e3b0fb00b27e6d2696acf0b73c6ba05b74efcd85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68663884"
---
# <a name="how-to-analyze-your-learning-loop-with-an-offline-evaluation"></a>Çevrimdışı değerlendirmede öğrenme döngünüzü çözümleme


Çevrimdışı değerlendirmeyi tamamlamayı ve sonuçları anlamayı öğrenin.

Çevrimdışı değerlendirmeler, etkin kişiselleştirmenin uygulamanızın varsayılan davranışıyla nasıl karşılaştırıldığını ölçmenize olanak tanır, kişiselleştirmeye en çok katkıda bulunan özellikleri öğrenin ve yeni makine öğrenimi ayarlarını otomatik olarak bulabilir.

Daha fazla bilgi edinmek için [çevrimdışı değerlendirmeler](concepts-offline-evaluation.md) hakkında bölümünü okuyun.


## <a name="prerequisites"></a>Önkoşullar

1. Bir kişiselleştirici döngüsüyle yapılandırılmış olması gerekir
1. Kişiselleştirici döngüsü, anlamlı değerlendirme sonuçları için günlüklerinde en az 50.000 olay içermelidir.

İsteğe bağlı olarak, aynı değerlendirmede karşılaştırmak ve test yapmak için daha önce de daha önce _öğrendiğiniz öğrenme ilkesi_ dosyalarını vermiş olabilirsiniz.

## <a name="steps-to-start-a-new-offline-evaluation"></a>Yeni bir çevrimdışı değerlendirme başlatma adımları

1. Azure portal kişiselleştirme döngüsü kaynağınızı bulun.
1. "Değerlendirme" bölümüne gidin.
1. Yeni değerlendirmeye tıklayın
1. Çevrimdışı değerlendirme için bir başlangıç ve bitiş tarihi seçin. Bunlar geçmişte, değerlendirmede kullanılacak veri aralığını belirten tarihlerdir. [Veri saklama](how-to-settings.md) ayarında belirtilen şekilde bu verilerin günlüklerde bulunması gerekir.
1. İsteğe bağlı olarak kendi öğrenme ilkenizi karşıya yükleyebilirsiniz. 
1. Kişiselleştirmede bu süre içinde gözlenen Kullanıcı davranışına göre iyileştirilmiş bir öğrenme Ilkesi oluşturup oluşturulmayacağını belirtin.
1. Değerlendirmeyi Başlat

## <a name="results"></a>Sonuçlar

İşlenecek veri miktarına, Karşılaştırılacak öğrenme ilkesi sayısına ve bir iyileştirmenin istenip istenmediğine bağlı olarak, değerlendirmelere çalıştırılması uzun zaman alabilir.

Tamamlandıktan sonra, aşağıdaki sonuçları görebilirsiniz:

1. Öğrenme Ilkelerinin karşılaştırmaları, şunlar dahildir:
    * **Çevrimiçi ilke**: Kişiselleştirici içinde kullanılan geçerli öğrenme Ilkesi
    * **Taban çizgisi**: Uygulamanın varsayılan (derece çağrılarında gönderilen ilk eylem tarafından belirlendiği şekilde),
    * **Rastgele ilke**: Her zaman sağlanan Işlemlerden rastgele eylem seçimi döndüren sanal bir sıra davranışı.
    * **Özel ilkeler**: Değerlendirme başlatılırken ek öğrenme Ilkeleri karşıya yüklendi.
    * **Iyileştirilmiş ilke**: Değerlendirme, iyileştirilmiş bir ilkeyi bulma seçeneği ile başlatıldıysa, bu da karşılaştırılır ve bunu indirebilir veya çevrimiçi öğrenme ilkesi haline getirmek için geçerli olanı değiştirin.

1. Eylemler ve bağlam [özelliklerinin](concepts-features.md) verimliliği.


## <a name="more-information"></a>Daha Fazla Bilgi

* [Çevrimdışı değerlendirmelerinin nasıl çalıştığını](concepts-offline-evaluation.md)öğrenin.