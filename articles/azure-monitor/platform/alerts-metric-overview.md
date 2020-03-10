---
title: Ölçüm uyarılarının Azure Izleyici 'de nasıl çalıştığını anlayın.
description: Ölçüm uyarıları ile yapabileceklerinize ve bunların Azure Izleyici 'de nasıl çalışabileceklerini bir genel bakış alın.
ms.date: 12/5/2019
ms.topic: conceptual
ms.subservice: alerts
ms.openlocfilehash: 2f1734d30136be904aedf7d880922ba052130ec7
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78384496"
---
# <a name="understand-how-metric-alerts-work-in-azure-monitor"></a>Ölçüm uyarılarının Azure Izleyici 'de nasıl çalıştığını anlama

Azure Izleyici 'de ölçüm uyarıları, çok boyutlu ölçümlerin üzerine çalışır. Bu ölçümler, [Platform ölçümleri](alerts-metric-near-real-time.md#metrics-and-dimensions-supported), [özel ölçümler](../../azure-monitor/platform/metrics-custom-overview.md), [Azure izleyici 'deki popüler Günlükler ölçüm](../../azure-monitor/platform/alerts-metric-logs.md) ve Application Insights ölçümlerine dönüştürülebilir. Ölçüm uyarıları, bir veya daha fazla ölçüm zaman serisinde koşulların doğru olup olmadığını denetlemek için düzenli aralıklarla değerlendirilir ve değerlendirmelere uyulduğunda bildirim alın. Ölçüm uyarıları durum bilgisi olur, diğer bir deyişle, yalnızca durum değiştiğinde bildirimleri gönderir.

## <a name="how-do-metric-alerts-work"></a>Ölçüm uyarıları nasıl çalışır?

İzlenecek hedef kaynak, ölçüm adı, koşul türü (statik veya dinamik) ve koşul (bir operatör ve eşik/duyarlılık) ve uyarı kuralı tetiklendiğinde tetiklenecek bir eylem grubu belirterek bir ölçüm uyarı kuralı tanımlayabilirsiniz. Koşul türleri eşiklerin nasıl belirleneceğini etkiler. [Dinamik eşikler durum türü ve duyarlılık seçenekleri hakkında daha fazla bilgi edinin](alerts-dynamic-thresholds.md).

### <a name="alert-rule-with-static-condition-type"></a>Statik koşul türü ile uyarı kuralı

Şu şekilde basit bir statik eşik ölçümü uyarı kuralı oluşturduğunuzu varsayalım:

- Hedef kaynak (izlemek istediğiniz Azure kaynağı): myVM
- Ölçüm: CPU yüzdesi
- Koşul türü: statik
- Zaman toplama (ham ölçüm değerleri üzerinden çalıştırılan Istatistik. Desteklenen süre toplamaları en az, en fazla, ortalama, toplam, sayı): Ortalama
- Süre (ölçüm değerlerinin denetlenme geri arama penceresi): son 5 dakika boyunca
- Sıklık (koşulların karşılandığını ölçüm uyarısının denetlediği sıklık): 1 dk
- İşleç: büyüktür
- Eşik: 70

Uyarı kuralının oluşturulduğu zamandan itibaren izleyici 1 dakikada bir çalışır ve son 5 dakika boyunca ölçüm değerlerine bakar ve bu değerlerin ortalamasının 70 ' ı aşıp aşmadığını denetler. Koşul karşılanıyorsa, son 5 dakika boyunca ortalama yüzde CPU 70 değerini aşarsa, uyarı kuralı etkinleştirilmiş bir bildirimi tetikler. Uyarı kuralıyla ilişkili eylem grubunda bir e-posta veya Web kancası eylemi yapılandırdıysanız her ikisinde de etkinleştirilmiş bir bildirim alırsınız.

Tek bir kuralda birden çok koşul kullanırken, "and" kuralı koşulları ile birlikte kullanılır.  Diğer bir deyişle, uyarı, uyarı içindeki tüm koşullar doğru olarak değerlendirilir ve koşullardan biri artık doğru olmadığında çözümlendiğinde ateşlenir. Ve bu tür bir uyarı örneği "%90 ' den yüksek CPU ve" kuyruk uzunluğu 300 öğeden fazla olduğunda "uyarı verebilir. 

### <a name="alert-rule-with-dynamic-condition-type"></a>Dinamik koşul türü ile uyarı kuralı

Aşağıdaki gibi basit bir dinamik eşikler ölçüm uyarısı kuralı oluşturduğunuzu varsayalım:

- Hedef kaynak (izlemek istediğiniz Azure kaynağı): myVM
- Ölçüm: CPU yüzdesi
- Koşul türü: dinamik
- Zaman toplama (ham ölçüm değerleri üzerinden çalıştırılan Istatistik. Desteklenen süre toplamaları en az, en fazla, ortalama, toplam, sayı): Ortalama
- Süre (ölçüm değerlerinin denetlenme geri arama penceresi): son 5 dakika boyunca
- Sıklık (koşulların karşılandığını ölçüm uyarısının denetlediği sıklık): 1 dk
- İşleç: büyüktür
- Duyarlılık: Orta
- Dönemleri geri ara: 4
- Ihlal sayısı: 4

Uyarı kuralı oluşturulduktan sonra dinamik eşikler makine öğrenimi algoritması, mevcut geçmiş verileri alır, ölçüm serisi davranış düzenine en uygun eşiği hesaplar ve bunu yapmak için yeni verilere göre sürekli olarak öğrenirsiniz eşik daha doğru.

Uyarı kuralının oluşturulduğu zamandan itibaren izleyici, her 1 dakikada bir çalışır ve 5 dakikalık bir dönemdeki ölçüm değerlerine bakar ve 4 dönem içindeki dönem değerlerinin ortalamasının beklenen eşiği aşıp aşmadığını denetler. Koşul karşılanıyorsa, son 20 dakikada ortalama yüzde CPU (dört 5 dakikalık dönem), beklenen davranışdan dört kez eşit oranda dağıtılır, uyarı kuralı etkinleştirilmiş bir bildirim tetikler. Uyarı kuralıyla ilişkili eylem grubunda bir e-posta veya Web kancası eylemi yapılandırdıysanız her ikisinde de etkinleştirilmiş bir bildirim alırsınız.

### <a name="view-and-resolution-of-fired-alerts"></a>Tetiklenen uyarıları görüntüleme ve çözümleme

Yukarıdaki uyarı kuralları tetikme örnekleri, **tüm uyarılar** dikey penceresindeki Azure Portal de görüntülenebilir.

"MyVM" üzerindeki kullanım, sonraki denetimlerde eşiğin üzerinde olmaya devam eder, bu durum, koşullar çözümlenene kadar uyarı kuralının yeniden başlatılmadığını düşünelim.

Bir süre sonra, "myVM" üzerindeki kullanım normal duruma gelir (eşiğin altına gider). Uyarı kuralı, çözümlenmiş bir bildirim göndermek için koşulu iki kez daha izler. Uyarı kuralı, kapatma koşulları durumunda paraziti azaltmak için uyarı koşulu üç ardışık dönem için karşılanmazsa çözümlenmiş/devre dışı bırakılmış bir ileti gönderir.

Çözümlenen bildirim Web kancaları veya e-posta aracılığıyla gönderildiği için, Azure portal içindeki uyarı örneğinin durumu (izleyici durumu olarak adlandırılır) de çözüldü olarak ayarlanır.

### <a name="using-dimensions"></a>Boyutları kullanma

Azure Izleyici 'de ölçüm uyarıları, tek bir kuralla birden çok boyut değer birleşimlerinin izlenmesini de destekler. Bir örneğin yardımıyla birden çok boyut bileşimini nasıl kullanabilecediğinizi anlayalim.

Web siteniz için bir App Service planınız olduğunu varsayalım. Web sitenizi/uygulamanızı çalıştıran birden çok örnekte CPU kullanımını izlemek istiyorsunuz. Bunu aşağıdaki gibi bir ölçüm uyarı kuralı kullanarak yapabilirsiniz:

- Hedef kaynak: myAppServicePlan
- Ölçüm: CPU yüzdesi
- Koşul türü: statik
- Boyutlar
  - Örnek = InstanceName1, InstanceName2
- Süre toplama: Ortalama
- Süre: son 5 dakika boyunca
- Sıklık: 1 dk
- İşleç: GreaterThan
- Eşik: 70

Daha önce olduğu gibi, bu kural son 5 dakika boyunca ortalama CPU kullanımının %70 ' ü aşarsa izleyicilerini izler. Ancak, aynı kuralla, Web sitenizi çalıştıran iki örneği izleyebilirsiniz. Her örnek ayrı ayrı izlenir ve bildirimleri tek tek alırsınız.

Büyük ölçüde talep gösteren bir Web uygulamanız olduğunu ve daha fazla örnek eklemeniz gerektiğini varsayalım. Yukarıdaki kural hala yalnızca iki örneği izler. Ancak, aşağıdaki gibi bir kural oluşturabilirsiniz:

- Hedef kaynak: myAppServicePlan
- Ölçüm: CPU yüzdesi
- Koşul türü: statik
- Boyutlar
  - Örnek = *
- Süre toplama: Ortalama
- Süre: son 5 dakika boyunca
- Sıklık: 1 dk
- İşleç: GreaterThan
- Eşik: 70

Bu kural örneğin tüm değerlerini otomatik olarak izler, örn. burada, ölçüm uyarı kuralınızı değiştirmeye gerek kalmadan örneklerinizi izleyebilirsiniz.

Birden çok boyut izlenirken, dinamik eşik uyarıları kuralı her seferinde yüzlerce ölçüm serisi için özel eşikler oluşturabilir. Dinamik eşikler, yönetim ve uyarı kurallarının yönetimine ve oluşturulmasına göre daha az uyarı kuralına sahip olur ve yönetimi ve önemli zaman tasarrufu sağlar.

Birçok örneğe sahip bir Web uygulamanız olduğunu ve en uygun eşiğin ne olduğunu bilmemenizi varsayalım. Yukarıdaki kurallar her zaman %70 eşiğini kullanır. Ancak, aşağıdaki gibi bir kural oluşturabilirsiniz:

- Hedef kaynak: myAppServicePlan
- Ölçüm: CPU yüzdesi
- Koşul türü: dinamik
- Boyutlar
  - Örnek = *
- Süre toplama: Ortalama
- Süre: son 5 dakika boyunca
- Sıklık: 1 dk
- İşleç: GreaterThan
- Duyarlılık: Orta
- Dönemleri geri ara: 1
- Ihlal sayısı: 1

Bu kural, son 5 dakika boyunca ortalama CPU kullanımının her örnek için beklenen davranışı aşarsa izler. Aynı kural, ölçüm uyarı kuralınızı yeniden değiştirmeye gerek kalmadan, örnekleri takip ettikleri şekilde izleyebilirsiniz. Her örnek, ölçüm serisi davranış düzenine uyan bir eşik alır ve eşiği daha doğru hale getirmek için yeni verilere göre sürekli olarak değişecektir. Daha önce olduğu gibi, her örnek ayrı ayrı izlenir ve bildirimleri ayrı ayrı alırsınız.

Geri arama sürelerini ve ihlallerin sayısını artırmak, uyarıların yalnızca önemli bir sapma tanımınızda uyarı vermesi için de izin verebilir. [Dinamik eşikler gelişmiş seçenekleri hakkında daha fazla bilgi edinin](alerts-dynamic-thresholds.md#what-do-the-advanced-settings-in-dynamic-thresholds-mean).

## <a name="monitoring-at-scale-using-metric-alerts-in-azure-monitor"></a>Azure Izleyici 'de ölçüm uyarılarını kullanarak ölçeğe göre izleme

Şimdiye kadar, tek bir Azure kaynağıyla ilgili bir veya daha fazla ölçüm zaman serisini izlemek için tek bir ölçüm uyarısının nasıl kullanılabileceğini gördünüz. Birçok kez, aynı uyarı kuralının birçok kaynağa uygulanmasını isteyebilirsiniz. Azure Izleyici aynı Azure bölgesinde bulunan kaynaklar için tek bir ölçüm uyarısı kuralıyla birden fazla kaynağın (aynı türden) izlenmesini de destekler. Bu özellik şu anda yalnızca Azure genel bulutunda ve yalnızca sanal makineler, SQL Server veritabanları, SQL Server elastik havuzlar ve veri kutusu uç cihazları için desteklenir. Ayrıca, bu özellik yalnızca platform ölçümleri için kullanılabilir ve özel ölçümler için desteklenmez.

Tek bir ölçüm uyarısı kuralına göre izlemenin kapsamını üç şekilde belirtebilirsiniz:

- bir abonelik içindeki bir Azure bölgesindeki sanal makinelerin listesi olarak
- bir abonelikteki bir veya daha fazla kaynak grubunda bulunan tüm sanal makineler (bir Azure bölgesinde)
- bir abonelikteki tüm sanal makineler (bir Azure bölgesinde)

Birden çok kaynağı izleyen ölçüm uyarısı kuralları oluşturmak, tek bir kaynağı izleyen [başka bir ölçüm uyarısı oluşturmak](alerts-metric.md) gibidir. Yalnızca fark, izlemek istediğiniz tüm kaynakları seçecekti. Ayrıca, bu kuralları [Azure Resource Manager şablonları](../../azure-monitor/platform/alerts-metric-create-templates.md#template-for-a-metric-alert-that-monitors-multiple-resources)aracılığıyla da oluşturabilirsiniz. İzlenen her kaynak için bireysel bildirimler alacaksınız.

## <a name="typical-latency"></a>Tipik gecikme süresi

Ölçüm uyarıları için genellikle uyarı kuralı sıklığını 1 dak olarak ayarlarsanız 5 dakika boyunca bildirim alacaksınız. Bildirim sistemleri için ağır yük durumunda daha uzun bir gecikme görebilirsiniz.

## <a name="supported-resource-types-for-metric-alerts"></a>Ölçüm uyarıları için desteklenen kaynak türleri

Desteklenen kaynak türlerinin tam listesini bu [makalede](../../azure-monitor/platform/alerts-metric-near-real-time.md#metrics-and-dimensions-supported)bulabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

- [Azure 'da ölçüm uyarılarını oluşturma, görüntüleme ve yönetme hakkında bilgi edinin](alerts-metric.md)
- [Azure Resource Manager şablonlarını kullanarak ölçüm uyarılarını dağıtmayı öğrenin](../../azure-monitor/platform/alerts-metric-create-templates.md)
- [Eylem grupları hakkında daha fazla bilgi edinin](action-groups.md)
- [Dinamik eşikler durum türü hakkında daha fazla bilgi edinin](alerts-dynamic-thresholds.md)
