---
title: Özel etkinlik merkezlerine genel bakış - Azure Etkinlik Hub'ları | Microsoft Dokümanlar
description: Bu makalede, etkinlik hub'larının tek kiracılı dağıtımları sunan özel Azure Etkinlik Hub'larına genel bir bakış sunulmaktadır.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: f67be1d31125b21048deca4d9cafcc76f4ffc3b1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72516753"
---
# <a name="overview-of-event-hubs-dedicated"></a>Özel Etkinlik Hub'larına Genel Bakış

*Olay Hub'ları kümeleri,* en zorlu akış gereksinimlerine sahip müşteriler için tek kiracılı dağıtımlar sunar. Bu tek kiracılı teklif %99,99 SLA garantilidir ve yalnızca Özel fiyatlandırma katmanımızda mevcuttur. Bir Olay Hub'ları kümesi, garantili kapasite ve saniyenin altı gecikmesi ile saniyede milyonlarca olay meydana getirebilir. Adanmış küme içinde oluşturulan ad alanları ve etkinlik hub'ları, Standart teklifin tüm özelliklerini ve daha fazlasını içerir, ancak giriş sınırı olmadan. Ayrıca, veri akışlarını Azure Depolama veya Azure Veri Gölü'ne otomatik olarak toplu olarak ekleyip günlüğe kaydetmenize olanak tanıyan popüler [Event Hubs Capture](event-hubs-capture-overview.md) özelliğini de ek ücret ödemeden içerir. 

Kümeler, önceden tahsis edilmiş bir CPU ve bellek kaynağı olan **Kapasite Birimleri (CUs)** tarafından karşılanır ve faturalandırılır. Her küme için 1, 2, 4, 8, 12, 16 veya 20 CUs satın alabilirsiniz. CU başına ne kadar yutup aktarabileceğiniz, üretici ve tüketici sayısı, yük şekli, çıkış oranı gibi çeşitli faktörlere bağlıdır (daha fazla ayrıntı için aşağıdaki kıyaslama sonuçlarına bakın). 

> [!NOTE]
> Tüm Event Hub kümeleri varsayılan olarak Kafka özelliklidir ve mevcut Kafka tabanlı uygulamalarınız tarafından kullanılabilecek Kafka uç noktalarını destekler. Kafka'nın kümenizde etkin olması Kafka dışı kullanım durumlarınızı etkilemez; Kafka'yı bir küme üzerinde devre dışı kalmanız için bir seçenek ya da gerek yoktur.

## <a name="why-dedicated"></a>Neden Adanmış?

Özel Etkinlik Hub'ları, kurumsal düzeyde kapasiteye ihtiyaç duyan müşteriler için üç cazip avantaj sunar:

#### <a name="single-tenancy-guarantees-capacity-for-better-performance"></a>Tek kira, daha iyi performans için kapasiteyi garanti eder

Özel küme, kapasiteyi tam ölçekte garanti eder ve trafikteki herhangi bir patlamayı karşılamak için tam dayanıklı depolama ve ikinci gecikme ye sahip gigabaytlara kadar veri akışı sağlayabilir. 

#### <a name="inclusive-and-exclusive-access-to-features"></a>Özelliklere kapsamlı ve özel erişim 
Özel teklif, ek ücret ödemeden Capture gibi özelliklerin yanı sıra Bring Your Own Key (BYOK) gibi yaklaşan özelliklere özel erişim içerir. Hizmet ayrıca yük dengeleme, işletim sistemi güncelleştirmeleri, güvenlik yamaları ve müşteri için bölümleme yönetir, böylece altyapı bakımına daha az zaman ve istemci tarafı özellikleri oluşturmaya daha fazla zaman ayırabilirsiniz.  

#### <a name="cost-savings"></a>Maliyet Tasarrufu
Yüksek giriş hacimlerinde (>100 TUs), bir kümenin saat başına maliyeti, Standart teklifteki benzer miktardaki üretim birimi satın almaktan önemli ölçüde daha azdır.


## <a name="event-hubs-dedicated-quotas-and-limits"></a>Etkinlik Merkezleri Özel Kotalar ve Limitler

Etkinlik Hub'ları Özel teklifi, en az 4 saatlik kullanımla sabit bir aylık fiyata faturalandırılır. Özel katman, Standart planın tüm özelliklerini sunar, ancak kurumsal ölçek kapasitesi ve zorlu iş yüküne sahip müşteriler için sınırlar sunar. 

| Özellik | Standart | Ayrılmış |
| --- |:---:|:---:|
| Bant genişliği | 20 TOS (en fazla 40 TUs) | 20 cus |
| Ad Alanları |  1 | CU başına 50 |
| Event Hubs |  Ad alanı başına 10 | Ad alanı başına 1000 |
| Giriş olayları | Milyon etkinlik başına ödeme | Dahil |
| İleti Boyutu | 1 Milyon Bayt | 1 Milyon Bayt |
| Bölümler | Etkinlik Hub'ı başına 32 | Etkinlik Hub başına 1024 |
| Tüketici grupları | Etkinlik Hub'ı başına 20 | CU başına sınır yok, etkinlik hub'ı başına 1000 |
| Aracılı bağlantılar | 1.000 dahil, 5.000 max | 100 K dahil ve maksimum |
| İleti Bekletme | 7 gün, TU başına 84 GB dahil | 90 gün, CU başına 10 TB dahil |
| Capture | Saat başına ödeme | Dahil |

## <a name="how-to-onboard"></a>Gemiye nasıl binilir?

[Azure Portalı](https://aka.ms/eventhubsclusterquickstart) üzerinden [bir Olay Hub'ları kümesi oluşturmaya](event-hubs-dedicated-cluster-create-portal.md) yarayacak self servis deneyimi artık Önizleme'de. Özel Etkinlik Hub'larına binme konusunda herhangi bir sorunuz varsa veya yardıma ihtiyacınız varsa, lütfen [Etkinlik Hub'ları ekibiyle](mailto:askeventhubs@microsoft.com)iletişime geçin.

## <a name="faqs"></a>SSS

#### <a name="what-can-i-achieve-with-a-cluster"></a>Bir kümeyle ne yapabilirim?

Bir Etkinlik Hub'ları kümesi için, ne kadar yutup akış yapabileceğiniz, üreticileriniz, tüketicileriniz, sindirip işleme oranınız ve çok daha fazlası gibi çeşitli etkenlere bağlıdır. 

Aşağıdaki tablo, testlerimiz sırasında elde ettiğimiz kıyaslama sonuçlarını gösterir:

| Yük şekli | Alıcı | Giriş bant genişliği| İletileri girin | Çıkış bant genişliği | Çıkış mesajları | Toplam TUs | CU başına TUs |
| ------------- | --------- | ---------------- | ------------------ | ----------------- | ------------------- | --------- | ---------- |
| 100x1KB toplu iş | 2 | 400 MB/sn | 400k ileti/sn | 800 MB/sn | 800k ileti/sn | 400 TUs | 100 tüs | 
| 10x10KB toplu iş | 2 | 666 MB/sn | 66.6k ileti/sn | 1,33 GB/sn | 133k ileti/sn | 666 TUs | 166 TUs |
| 6x32KB toplu iş | 1 | 1,05 GB/sn | 34k mesajlar / sn | 1,05 GB/sn | 34k ileti/sn | 1000 TUs | 250 TÜs |

Testte aşağıdaki kriterler kullanılmıştır:

- Dört kapasite birimi (CUs) içeren özel katmanlı Olay Hub'ları kümesi kullanıldı. 
- Yutma için kullanılan olay hub'ı 200 bölümden osahipti. 
- Yutulan veriler, tüm bölümlerden alan iki alıcı uygulaması tarafından alındı.

#### <a name="can-i-scale-updown-my-cluster"></a>Kümemi büyütebilir miyim/küçültebilir miyim?

Oluşturulduktan sonra kümeler en az 4 saatlik kullanım için faturalandırılır. Self servis deneyiminin Önizleme sürümünde, Kümenizi yukarı veya aşağı ölçeklendirmek için Teknik > Kotası > *Özel Kümeyi Ölçeklendirme veya Küçültme Isteği* altındaki Olay Hub'ları ekibine bir destek [isteği](https://ms.portal.azure.com/#create/Microsoft.Support) gönderebilirsiniz. Kümenizi küçültme isteğinin tamamlanması 7 gün kadar sürebilir. 

#### <a name="how-will-geo-dr-work-with-my-cluster"></a>Geo-DR kümemle nasıl çalışacak?

Bir ad alanını, Adanmış katman kümesi altında, Adanmış katman kümesi altında başka bir ad alanıyla coğrafi olarak eşleştirebilirsiniz. Üretim limiti hatalara neden olacak uyumsuz olacağından, Standart teklifimizde bir ad alanıyla Özel katman ad alanı eşleştirmeyi teşvik etmeyiz. 

#### <a name="can-i-migrate-my-standard-namespaces-to-belong-to-a-dedicated-tier-cluster"></a>Standart ad alanlarımı Adanmış katman kümesine ait olacak şekilde geçirebilir miyim?
Şu anda olay hub'larınızı Standart ad alanından Özel bir adaktan geçirmek için otomatik bir geçiş işlemini destekliyoruz. 

## <a name="next-steps"></a>Sonraki adımlar

Özel Etkinlik Hub'ları hakkında ek bilgi almak için Microsoft satış temsilcinize veya Microsoft Destek'e başvurun. Ayrıca aşağıdaki bağlantıları ziyaret ederek bir küme oluşturabilir veya Etkinlik Hub'ları fiyatlandırma katmanları hakkında daha fazla bilgi edinebilirsiniz:

- [Azure Portalı'nda Etkinlik Hub'ları kümesi oluşturma](https://aka.ms/eventhubsclusterquickstart) 
- [Olay Hub'ları Özel fiyatlandırma](https://azure.microsoft.com/pricing/details/event-hubs/). Ayrıca, Etkinlik Hub'larına Özel kapasite hakkında ek bilgi almak için Microsoft satış temsilcinize veya Microsoft Destek'e başvurabilirsiniz.
- [Olay Hub'ları SSS](event-hubs-faq.md) fiyatlandırma bilgilerini içerir ve Olay Hub'ları hakkında sık sorulan bazı soruları yanıtlar.
