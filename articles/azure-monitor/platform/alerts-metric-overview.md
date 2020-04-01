---
title: Azure Monitor'da metrik uyarıların nasıl çalıştığını anlayın.
description: Azure Monitor'da metrik uyarılarla neler yapabileceğinize ve bunların nasıl çalıştığına genel bir bakış alın.
ms.date: 03/17/2020
ms.topic: conceptual
ms.subservice: alerts
ms.openlocfilehash: a6860cad077b597df923274f8971f5652d4ba9e3
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80397983"
---
# <a name="understand-how-metric-alerts-work-in-azure-monitor"></a>Azure İzleyici'de ölçüm uyarılarının nasıl çalıştığını anlama

Azure Monitor'daki metrik uyarılar çok boyutlu ölçümlerin üzerinde çalışır. Bu ölçümler [platform ölçümleri,](alerts-metric-near-real-time.md#metrics-and-dimensions-supported) [özel ölçümler,](../../azure-monitor/platform/metrics-custom-overview.md) [Azure Monitor'un ölçümlere dönüştürülmüş popüler günlükleri](../../azure-monitor/platform/alerts-metric-logs.md) ve Uygulama Öngörüleri ölçümleri olabilir. Metrik uyarılar, bir veya daha fazla metrik zaman serisindeki koşulların doğru olup olmadığını kontrol etmek ve değerlendirmeler karşılandığında sizi bilgilendirmek için düzenli aralıklarla değerlendirir. Metrik uyarılar durum durumu, diğer bir şekilde yalnızca durum değiştiğinde bildirim gönderirler.

## <a name="how-do-metric-alerts-work"></a>Metrik uyarılar nasıl çalışır?

İzlenecek bir hedef kaynak, metrik ad, koşul türü (statik veya dinamik) ve durum (işleç ve eşik/duyarlılık) ve uyarı kuralı devreye girdiğinde tetiklenecek bir eylem grubu belirterek bir metrik uyarı kuralı tanımlayabilirsiniz. Koşul türleri eşiklerin nasıl belirleneceğini etkiler. [Dinamik Eşikler durum türü ve duyarlılık seçenekleri hakkında daha fazla bilgi edinin.](alerts-dynamic-thresholds.md)

### <a name="alert-rule-with-static-condition-type"></a>Statik durum türüne sahip uyarı kuralı

Aşağıdaki gibi basit bir statik eşik metrik uyarı kuralı oluşturduğunuzu varsayalım:

- Hedef Kaynak (izlemek istediğiniz Azure kaynağı): myVM
- Metrik: Yüzde CPU
- Koşul Türü: Statik
- Zaman Toplama (Ham metrik değerlerin üzerinde çalıştırılabilen istatistik. Desteklenen zaman toplamaları Min, Max, Avg, Toplam, Sayım): Ortalama
- Dönem (Metrik değerlerin kontrol edildiği arka pencereye bakış): Son 5 dakika içinde
- Sıklık (Koşullar yerine getirilirse metrik uyarının kontrol ettiği sıklık): 1 dk
- Operatör: Büyük
- Eşik: 70

Uyarı kuralı oluşturulduğu andan itibaren, monitör her 1 dakika çalışır ve son 5 dakika için metrik değerlere bakar ve bu değerlerin ortalaması70'i aşArsa kontrol eder. Durum, son 5 dakikanın ortalama Yüzde CPU'su 70'i aşarsa, uyarı kuralı etkinleştirilmiş bir bildirimi çalıştırıyor. Uyarı kuralıyla ilişkili eylem grubunda bir e-posta veya web bağlantısı eylemi yapılandırmışsanız, her ikisinde de etkinleştirilmiş bir bildirim alırsınız.

Tek bir kuralda birden çok koşul kullandığınızda, koşulları birlikte "ves" kuralı.  Diğer bir deyişle, uyarıdaki tüm koşullar doğru olarak değerlendirildiğinde ve koşullardan biri artık doğru olmadığında çözümlendiğinde uyarı yangınları. Ve bu tür bir uyarı örneği "CPU%90'dan yüksek" ve "sıra uzunluğu 300'den fazla öğe" olduğunda uyarı olacaktır. 

### <a name="alert-rule-with-dynamic-condition-type"></a>Dinamik durum türüne sahip uyarı kuralı

Aşağıdaki gibi basit bir Dinamik Eşikler metrik uyarı kuralı oluşturduğunuzu varsayalım:

- Hedef Kaynak (izlemek istediğiniz Azure kaynağı): myVM
- Metrik: Yüzde CPU
- Koşul Türü: Dinamik
- Zaman Toplama (Ham metrik değerlerin üzerinde çalıştırılabilen istatistik. Desteklenen zaman toplamaları Min, Max, Avg, Toplam, Sayım): Ortalama
- Dönem (Metrik değerlerin kontrol edildiği arka pencereye bakış): Son 5 dakika içinde
- Sıklık (Koşullar yerine getirilirse metrik uyarının kontrol ettiği sıklık): 1 dk
- Operatör: Büyük
- Hassasiyet: Orta
- Geriye Bak Periyotlar: 4
- İhlal Sayısı: 4

Uyarı kuralı oluşturulduktan sonra, Dinamik Eşikler makine öğrenme algoritması kullanılabilir geçmiş verileri elde edecek, metrik seri davranış desenine en uygun eşiği hesaplar ve eşiği daha doğru hale getirmek için yeni verilere dayalı olarak sürekli olarak öğrenir.

Uyarı kuralı oluşturulduğu andan itibaren, monitör her 1 dakika çalışır ve son 20 dakika içinde 5 dakikalık dönemler halinde gruplanan metrik değerlere bakar ve 4 dönemin her birinde dönem değerlerinin ortalamasının beklenen eşiği aşıp aşmadığı kontrol eder. Durum, yani durum karşılanırsa, son 20 dakika (dört 5 dakika periyot) ortalama Yüzde CPU beklenen davranış dört kez sapmış, uyarı kuralı etkin leştirilmiş bir bildirim yangınları. Uyarı kuralıyla ilişkili eylem grubunda bir e-posta veya web bağlantısı eylemi yapılandırmışsanız, her ikisinde de etkinleştirilmiş bir bildirim alırsınız.

### <a name="view-and-resolution-of-fired-alerts"></a>Ateşlenen uyarıların görüntülenmesi ve çözümü

Yukarıdaki uyarı kuralları nın ateşlenme örnekleri, **Tüm Uyarılar** bıçağındaki Azure portalında da görülebilir.

"myVM" kullanımı sonraki kontrollerde eşiğin üzerinde olmaya devam söylüyorlar, koşullar çözülene kadar uyarı kuralı tekrar ateş olmaz.

Bir süre sonra, "myVM" kullanımı normale geri döner (eşiğin altına iner). Uyarı kuralı, çözülmüş bir bildirim göndermek için durumu iki kez daha izler. Uyarı kuralı, atlama koşullarında gürültüyü azaltmak için uyarı koşulu üç dönem üst üste karşılanmadığında çözülmüş/devre dışı bırakılmış bir ileti gönderir.

Çözülen bildirim web kancaları veya e-posta yoluyla gönderildiğinden, Azure portalındaki uyarı örneğinin (monitör durumu olarak adlandırılır) durumu da çözülecek şekilde ayarlanır.

### <a name="using-dimensions"></a>Boyutları kullanma

Azure Monitor'daki metrik uyarılar, birden çok boyut değer birleşimini tek bir kuralla izlemeyi de destekler. Bir örnek yardımıyla neden birden çok boyut birleşimleri kullanabileceğinizi anlayalım.

Web siteniz için bir Uygulama Hizmeti planınız olduğunu varsa. Web sitenizi/uygulamanızı çalıştıran birden çok durumda CPU kullanımını izlemek istiyorsunuz. Bunu aşağıdaki gibi bir metrik uyarı kuralı nı kullanarak yapabilirsiniz:

- Hedef kaynak: myAppServicePlan
- Metrik: Yüzde CPU
- Koşul Türü: Statik
- Boyutlar
  - Instance = InstanceName1, InstanceName2
- Zaman Toplama: Ortalama
- Periyot: Son 5 dakika içinde
- Sıklık: 1 dk
- Operatör: GreaterThan
- Eşik: 70

Daha önce olduğu gibi, son 5 dakikanın ortalama CPU kullanımı %70'i geçerse bu kural izler. Ancak, aynı kuralla web sitenizi çalıştıran iki örneği izleyebilirsiniz. Her örnek ayrı ayrı izlenir ve bildirimleri ayrı ayrı alırsınız.

Eğer büyük talep görüyor ve daha fazla örnek eklemek gerekir bir web uygulaması var diyelim. Yukarıdaki kural hala sadece iki örneği izler. Ancak, aşağıdaki gibi bir kural oluşturabilirsiniz:

- Hedef kaynak: myAppServicePlan
- Metrik: Yüzde CPU
- Koşul Türü: Statik
- Boyutlar
  - Örnek = *
- Zaman Toplama: Ortalama
- Periyot: Son 5 dakika içinde
- Sıklık: 1 dk
- Operatör: GreaterThan
- Eşik: 70

Bu kural, örneğin tüm değerlerini otomatik olarak izler. metrik uyarı kuralınızı yeniden değiştirmeye gerek kalmadan örneklerinizi yukarı çıktıkça izleyebilirsiniz.

Birden çok boyutu izlerken, Dinamik Eşikler uyarıları kuralı aynı anda yüzlerce metrik seri için özel eşikler oluşturabilir. Dinamik Eşikler, yönetmek için daha az uyarı kuralı ve yönetim ve uyarı kurallarının oluşturulmasında önemli zaman tasarrufu sağlar.

Birçok örneği olan bir web uygulamanız olduğunu ve en uygun eşiğin ne olduğunu bilmediğinizi varsayın. Yukarıdaki kurallar her zaman %70 eşiğini kullanır. Ancak, aşağıdaki gibi bir kural oluşturabilirsiniz:

- Hedef kaynak: myAppServicePlan
- Metrik: Yüzde CPU
- Koşul Türü: Dinamik
- Boyutlar
  - Örnek = *
- Zaman Toplama: Ortalama
- Periyot: Son 5 dakika içinde
- Sıklık: 1 dk
- Operatör: GreaterThan
- Hassasiyet: Orta
- Geriye Bak Periyotlar: 1
- İhlal Sayısı: 1

Bu kural, son 5 dakikadaki ortalama CPU kullanımının her örnek için beklenen davranışı aşması durumunda denetlenir. Aynı kural, metrik uyarı kuralınızı yeniden değiştirmeye gerek kalmadan durumları geldikleri gibi izleyebilirsiniz. Her örnek, metrik seri davranış desenine uyan bir eşik alır ve eşiği daha doğru hale getirmek için yeni verilere göre sürekli olarak değişir. Daha önce olduğu gibi, her örnek ayrı ayrı izlenir ve bildirimleri ayrı ayrı alırsınız.

Geri dönüş sürelerini ve ihlal sayısını artırmak, filtreleme uyarılarının yalnızca önemli bir sapma tanımınızı uyarmasına da izin verebilir. [Dinamik Eşikler gelişmiş seçenekleri hakkında daha fazla bilgi edinin.](alerts-dynamic-thresholds.md#what-do-the-advanced-settings-in-dynamic-thresholds-mean)

## <a name="monitoring-at-scale-using-metric-alerts-in-azure-monitor"></a>Azure Monitörü'nde metrik uyarıları kullanarak ölçekte izleme

Şimdiye kadar, tek bir Azure kaynağıyla ilgili bir veya çok sayıda metrik zaman serisini izlemek için tek bir metrik uyarının nasıl kullanılabileceğini gördünüz. Çoğu zaman, aynı uyarı kuralının birçok kaynağa uygulanmasını isteyebilirsiniz. Azure Monitor, aynı Azure bölgesinde bulunan kaynaklar için tek bir metrik uyarı kuralıyla birden çok kaynağın (aynı türde) izlenmesini de destekler. 

Bu özellik şu anda aşağıdaki Azure bulutlarında aşağıdaki hizmetler için platform ölçümleri (özel ölçümler için değil) için desteklenir:

| Hizmet | Genel Azure | Devlet | Çin |
|:--------|:--------|:--------|:--------|
| Sanal makineler  | **Evet** | Hayır | Hayır |
| SQL sunucu veritabanları | **Evet** | **Evet** | Hayır |
| SQL sunucu elastik havuzları | **Evet** | **Evet** | Hayır |
| Veri kutusu kenar aygıtları | **Evet** | **Evet** | Hayır |

İzleme nin kapsamını tek bir metrik uyarı kuralıyla üç şekilde belirtebilirsiniz. Örneğin, sanal makinelerde kapsamı aşağıdaki gibi belirtebilirsiniz:  

- abonelik içinde bir Azure bölgesindeki sanal makinelerin listesi
- abonelikteki bir veya daha fazla kaynak grubundaki tüm sanal makineler (bir Azure bölgesinde)
- tek bir abonelikteki tüm sanal makineler (tek bir Azure bölgesinde)

Birden çok kaynağı izleyen metrik uyarı kuralları oluşturmak, tek bir kaynağı izleyen başka bir [metrik uyarı oluşturmaya](alerts-metric.md) benzer. Tek fark, izlemek istediğiniz tüm kaynakları seçeceğinizdir. Bu kuralları [Azure Kaynak Yöneticisi şablonları](../../azure-monitor/platform/alerts-metric-create-templates.md#template-for-a-metric-alert-that-monitors-multiple-resources)aracılığıyla da oluşturabilirsiniz. İzlenen her kaynak için ayrı ayrı bildirimler alırsınız.

> [!NOTE]
>
> Birden çok kaynağı izleyen bir metrik uyarı kuralında yalnızca bir koşula izin verilir.

## <a name="typical-latency"></a>Tipik gecikme

Metrik uyarılar için, uyarı kuralı sıklığını 1 dakika olarak ayarlarsanız, genellikle 5 dakikadan kısa bir süre içinde bilgilendirilirsiniz. Bildirim sistemleri için ağır yük durumlarında, daha uzun bir gecikme durumu görebilirsiniz.

## <a name="supported-resource-types-for-metric-alerts"></a>Metrik uyarılar için desteklenen kaynak türleri

Desteklenen kaynak türlerinin tam listesini bu [makalede](../../azure-monitor/platform/alerts-metric-near-real-time.md#metrics-and-dimensions-supported)bulabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

- [Azure'da metrik uyarıları oluşturma, görüntüleme ve yönetme öğrenin](alerts-metric.md)
- [Azure Kaynak Yöneticisi şablonlarını kullanarak metrik uyarıları nasıl dağıtacağınız hakkında bilgi edinin](../../azure-monitor/platform/alerts-metric-create-templates.md)
- [Eylem grupları hakkında daha fazla bilgi edinin](action-groups.md)
- [Dinamik Eşikler koşul türü hakkında daha fazla bilgi edinin](alerts-dynamic-thresholds.md)
