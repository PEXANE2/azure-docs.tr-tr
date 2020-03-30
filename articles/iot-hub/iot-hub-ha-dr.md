---
title: Azure IoT Hub yüksek kullanılabilirlik ve olağanüstü durum kurtarma | Microsoft Dokümanlar
description: Olağanüstü durum kurtarma özelliklerine sahip yüksek oranda kullanılabilir Azure IoT çözümleri oluşturmanıza yardımcı olan Azure ve IoT Hub özelliklerini açıklar.
author: jlian
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 03/17/2020
ms.author: philmea
ms.openlocfilehash: 615dc1b7bd1a31069a542ebb7ea44693c404cb40
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79499102"
---
# <a name="iot-hub-high-availability-and-disaster-recovery"></a>IoT Hub yüksek kullanılabilirlik ve olağanüstü durum kurtarma

Esnek bir IoT çözümlerini uygulama yolunda ilk adım olarak, mimarlar, geliştiriciler ve işletme sahipleri, inşa ettikleri çözümleriçin çalışma zamanı hedeflerini tanımlamalıdır. Bu hedefler öncelikle her senaryo için belirli iş hedeflerine göre tanımlanabilir. Bu bağlamda, [Azure İş Sürekliliği Teknik Kılavuzu](https://docs.microsoft.com/azure/architecture/resiliency/) makalesi, iş sürekliliği ve olağanüstü durum kurtarma hakkında düşünmenize yardımcı olacak genel bir çerçeveyi açıklar. [Olağanüstü Durum kurtarma ve Azure uygulamaları için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/architecture/reliability/disaster-recovery) makalesi, Yüksek Kullanılabilirlik (HA) ve Olağanüstü Durum Kurtarma (DR) elde etmek için Azure uygulamaları için stratejiler hakkında mimari kılavuz sağlar.

Bu makalede, Özellikle IoT Hub hizmeti tarafından sunulan HA ve DR özellikleri açıkılmaktadır. Bu makalede tartışılan geniş alanlar şunlardır:

- Bölge içi HA
- Çapraz bölge DR
- Çapraz bölge HA elde

IoT çözümleriniz için tanımladığınız çalışma süresi hedeflerine bağlı olarak, aşağıda özetlenen seçeneklerden hangisinin işletme hedeflerinize en uygun olduğunu belirlemeniz gerekir. Bu HA/DR alternatiflerinden herhangi birini IoT çözümünüze dahil etmek, aşağıdakiler arasındaki dengelerin dikkatli bir şekilde değerlendirilmesini gerektirir:

- Gereksinim duyduğunuz esneklik düzeyi 
- Uygulama ve bakım karmaşıklığı
- COGS etkisi

## <a name="intra-region-ha"></a>Bölge içi HA

IoT Hub hizmeti, hizmetin hemen hemen tüm katmanlarında fazlalıklar uygulayarak bölge içi HA sağlar. [IoT Hub hizmeti tarafından yayınlanan SLA,](https://azure.microsoft.com/support/legal/sla/iot-hub) bu işten çıkarmalardan yararlanılarak elde edilir. Bu HA özelliklerinden yararlanmak için bir IoT çözüm geliştiricileri tarafından ek bir çalışma gerekmez. IoT Hub makul derecede yüksek çalışma süresi garantisi sunsa da, dağıtılmış bilgi işlem platformlarında olduğu gibi geçici hatalar beklenebilir. Çözümlerinizi şirket içi bir çözümden buluta kaydırmaya yeni başlıyorsanız, odak noktanızın "hatalar arasındaki ortalama süreyi" en iyi duruma getirmekten "kurtarma süresi ne kadar" olarak değiştirmesi gerekir. Başka bir deyişle, geçici hatalar karışımdaki bulutla çalışırken normal olarak kabul edilmelidir. Geçici hatalarla başa çıkmak için bulut uygulamasıyla etkileşimde bulunan bileşenlere uygun [yeniden deneme ilkeleri](iot-hub-reliability-features-in-sdks.md) oluşturulmalıdır.

> [!NOTE]
> Bazı Azure hizmetleri, [Kullanılabilirlik Bölgeleri (AZs)](../availability-zones/az-overview.md)ile tümleştirerek bir bölge içinde ek kullanılabilirlik katmanları da sağlar. AZ'lar şu anda IoT Hub hizmeti tarafından desteklenmez.

## <a name="cross-region-dr"></a>Çapraz bölge DR

Bir veri merkezinin, elektrik kesintileri veya fiziksel varlıkları içeren diğer arızalar nedeniyle uzun süreli kesintiler yaşadığı nadir durumlar olabilir. Bu tür olaylar sırasında intra bölge HA yeteneği yukarıda açıklanan her zaman yardımcı olmayabilir nadirdir. IoT Hub, bu tür uzun kesintileri atlatmak için birden çok çözüm sağlar. 

Böyle bir durumda müşteriler için kullanılabilir kurtarma seçenekleri [Microsoft tarafından başlatılan failover](#microsoft-initiated-failover) ve [manuel failover](#manual-failover)vardır. İkisi arasındaki temel fark, Microsoft'un eskisini, kullanıcının ise ikincisini başlatmasıdır. Ayrıca, manuel failover, Microsoft tarafından başlatılan başarısızlık seçeneğine kıyasla daha düşük bir kurtarma süresi hedefi (RTO) sağlar. Her seçenekle sunulan özel RTO'lar aşağıdaki bölümlerde ele alınmıştır. Bir IoT hub'ının birincil bölgesinden başarısız olmasını gerçekleştirmek için bu seçeneklerden biri çalıştırıldığında, hub ilgili [Azure coğrafi eşleştirilmiş bölgede](../best-practices-availability-paired-regions.md)tamamen işlevsel hale gelir.

Bu başarısız seçeneklerin her ikisi de aşağıdaki kurtarma noktası hedeflerini (RMO'lar) sunar:

| Veri türü | Kurtarma noktası hedefleri (RPO) |
| --- | --- |
| Kimlik kaydı |0-5 dakika veri kaybı |
| Aygıt ikiz verileri |0-5 dakika veri kaybı |
| Buluttan cihaza iletiler<sup>1</sup> |0-5 dakika veri kaybı |
| Üst<sup>1</sup> ve aygıt işleri |0-5 dakika veri kaybı |
| Cihazdan buluta iletiler |Tüm okunmamış iletiler kaybolur |
| İşlemleri izleme iletileri |Tüm okunmamış iletiler kaybolur |
| Buluttan cihaza geri bildirim iletileri |Tüm okunmamış iletiler kaybolur |

<sup>1.1.2</sup> Buluttan aygıta iletiler ve üst işler, el ile başarısız olmasının bir parçası olarak kurtarılamaz.

IoT hub'ı için arıza işlemi tamamlandıktan sonra, cihazdan ve arka uç uygulamalarından gelen tüm işlemlerin manuel müdahale gerektirmeden çalışmaya devam etmesi beklenmektedir. Bu, aygıttan buluta iletilerinizin çalışmaya devam etmesi ve tüm aygıt kayıt defterinin sağlam olduğu anlamına gelir. Olay Izgarası üzerinden yayılan etkinlikler, bu Olay Izgara abonelikleri kullanılabilir olmaya devam ettiği sürece daha önce yapılandırılan aynı abonelik(ler) aracılığıyla tüketilebilir.

> [!CAUTION]
> - IoT Hub'ının Olay Hub'ı uyumlu adı ve bitiş noktası, başarısız olduktan sonra Etkinlikler bitiş noktası değişikliği ve yapılandırılmış tüketici grupları kaldırılır (bu, Mayıs 2020'den önce düzeltilecek bir hatadır). Olay Hub istemcisi veya olay işlemci ana bilgisayarını kullanarak yerleşik uç noktadan telemetri iletileri alırken, bağlantıyı kurmak için [IoT hub bağlantı dizesini kullanmanız](iot-hub-devguide-messages-read-builtin.md#read-from-the-built-in-endpoint) gerekir. Bu, arka uç uygulamalarınızın manuel müdahale sonrası başarısızlık gerektirmeden çalışmaya devam etmesini sağlar. Uygulamanızda Event Hub uyumlu ad ve bitiş noktasını doğrudan kullanırsanız, kullandıkları tüketici grubunu yeniden yapılandırmanız ve işlemlere devam etmek için başarısız olduktan sonra [yeni Event Hub uyumlu bitiş noktasını getirmeniz](iot-hub-devguide-messages-read-builtin.md#read-from-the-built-in-endpoint) gerekir. Yerleşik bitiş noktasını bağlamak için Azure İşlevlerini veya Azure Akış Analizi'ni kullanıyorsanız, yeniden **başlatma**yapmanız gerekebilir.
>
> - Depolamaya yönlendirme yaparken, tüm lekelerin veya dosyaların bölümleme varsayımı nda bulunmadan okunduğundan emin olmak için lekeleri veya dosyaları listelemenizi ve sonra bunların üzerine tekrarlamanızı öneririz. Microsoft tarafından başlatılan bir hata veya el ile başarısızlık sırasında bölüm aralığı değişebilir. Dosya listesi için blobs listesini veya [ADLS Gen2 API](https://docs.microsoft.com/rest/api/storageservices/datalakestoragegen2/path/list) listesini listelemek için [Liste Blobs API'yi](https://docs.microsoft.com/rest/api/storageservices/list-blobs) kullanabilirsiniz. 

## <a name="microsoft-initiated-failover"></a>Microsoft tarafından başlatılan failover

Microsoft tarafından başlatılan başarısız lık, Microsoft tarafından nadir durumlarda etkilenen bir bölgeden ilgili coğrafi eşleştirilmiş bölgeye kadar tüm IoT hub'larının başarısız olması için çalıştırılır. Bu işlem varsayılan bir seçenektir (kullanıcıların devre dışı bırakmaları için hiçbir yol yoktur) ve kullanıcıdan müdahale gerektirmez. Microsoft, bu seçeneğin ne zaman uygulanacağını belirleme hakkını saklı tutar. Bu mekanizma, kullanıcının hub'ı başarısız olmadan önce kullanıcı onayı içermez. Microsoft tarafından başlatılan failover 2-26 saat bir kurtarma süresi hedefi (RTO) vardır. 

Büyük RTO, Microsoft'un başarısız işlemi o bölgedeki etkilenen tüm müşteriler adına gerçekleştirmesi gerektiğidir. Yaklaşık bir günlük bir kesintiyi sürdürebilecek daha az kritik bir IoT çözümü çalıştırıyorsanız, IoT çözümünüz için genel olağanüstü durum kurtarma hedeflerini karşılamak için bu seçenebağlık bu seçeneği tercih elabilirsiniz. Bu işlem tetiklendikten sonra çalışma zamanı işlemlerinin tam olarak çalışmaya devam etmesi için toplam süre "Kurtarma Zamanı" bölümünde açıklanmıştır.

## <a name="manual-failover"></a>El ile yük devretme

Microsoft'un başlattığı başarısızlık sağladığı RTO tarafından iş çalışma süresi hedefleriniz karşılanmazsa, başarısızlık işlemini kendiniz tetiklemek için manuel hata yı kullanmayı düşünün. Bu seçeneği kullanan RTO 10 dakika ile birkaç saat arasında olabilir. RTO şu anda IoT hub örneğine karşı kayıtlı aygıt sayısının bir işlevidir. RTO'nun yaklaşık 100.000 cihazı barındıran bir hub için 15 dakikalık bir sahada olmasını bekleyebilirsiniz. Bu işlem tetiklendikten sonra çalışma zamanı işlemlerinin tam olarak çalışmaya devam etmesi için toplam süre "Kurtarma Zamanı" bölümünde açıklanmıştır.

El ile başarısız etme seçeneği, birincil bölgenin kapalı kalma süresi yaşanıp yaşanmadığına bakılmaksızın her zaman kullanılabilir. Bu nedenle, bu seçenek potansiyel olarak planlanan arızaları gerçekleştirmek için kullanılabilir. Planlı arızaların bir örneği periyodik arıza matkapları gerçekleştirmektir. Ancak bir nokta, planlanan bir arıza işleminin bu seçenek için RTO tarafından tanımlanan dönem için hub için bir kesintiyle sonuçlandığı ve ayrıca yukarıdaki RPO tablosunda tanımlandığı gibi bir veri kaybına neden olmasıdır. Gerçek bir felaket olduğunda uçuççözümlerinizi çalışır hale getirebilme yeteneğinize güven kazanmak için planlanan başarısızlık seçeneğini düzenli aralıklarla uygulamak için bir test IoT hub örneği ayarlamayı düşünebilirsiniz.

Adım adım talimatlar için [Bkz. Öğretici: Bir IoT hub'ı için manuel hata yapma](tutorial-manual-failover.md)

### <a name="running-test-drills"></a>Test matkaplarını çalıştırma

Test matkapları, üretim ortamlarınızda kullanılan IoT hub'larına yapılmamalıdır.

### <a name="dont-use-manual-failover-to-migrate-iot-hub-to-a-different-region"></a>IoT hub'ı farklı bir bölgeye geçirmek için manuel hata yapmayın

El ile başarısız olma, hub'ınızı Azure coğrafi eşlemalı bölgeler arasında kalıcı olarak geçirmek için bir mekanizma olarak *kullanılmamalıdır.* Bunu yapmak, eski birincil bölgede bulunan aygıtlardan IoT hub'ına karşı gerçekleştirilen işlemlerin gecikme süresini artırır.

## <a name="failback"></a>Yeniden çalışma

Eski birincil bölgeye geri başarısız başka bir zaman failover eylem tetikleyerek elde edilebilir. Özgün birincil bölgedeki uzun süreli kesintiden kurtarmak için orijinal başarısız etme işlemi gerçekleştirildiyse, hub'ın kesinti durumundan kurtarıldıktan sonra hub'ın orijinal konuma geri dönmesini önerdik.

> [!IMPORTANT]
> - Kullanıcılar sadece günde 2 başarılı failover ve 2 başarılı failback işlemleri gerçekleştirmek için izin verilir.
>
> - Arka arkaya failover/failback işlemlerine izin verilmez. Bu işlemler arasında 1 saat beklemeniz gerekir.

## <a name="time-to-recover"></a>Kurtarma zamanı

IoT hub örneğinin FQDN (ve dolayısıyla bağlantı dizesi) aynı posta failover kalır iken, altta yatan IP adresi değişir. Bu nedenle, başarısız olma işlemi tetiklendikten sonra IoT hub örneğinize karşı gerçekleştirilen çalışma zamanı işlemlerinin genel süresi aşağıdaki işlev kullanılarak ifade edilebilir.

Kurtarma süresi = RTO [10 dk - manuel arıza için 2 saat | 2 - Microsoft tarafından başlatılan failover için 26 saat] + DNS yayılma gecikmesi + Önbelleğe alınmış ioT Hub IP adresini yenilemek için istemci uygulaması tarafından alınan süre.

> [!IMPORTANT]
> IoT SDK'lar IoT hub'ının IP adresini önbelleğe almaz. SDK'larla birarada bulunan kullanıcı kodunun IoT hub'ının IP adresini önbelleğe almaması gerektiğini öneririz.

## <a name="achieve-cross-region-ha"></a>Çapraz bölge HA elde

Microsoft tarafından başlatılan başarısız veya el ile başarısız seçeneklerin sağladığı RTO tarafından karşılanmazsa, cihaz başına otomatik çapraz bölge arıza mekanizması uygulamayı düşünmelisiniz.
IoT çözümlerinde dağıtım topolojilerinin tam bir tedavisi bu makalenin kapsamı dışındadır. Makalede, yüksek kullanılabilirlik ve olağanüstü durum kurtarma amacıyla *bölgesel başarısız* dağıtım modeli tartışılmaktadır.

Bölgesel bir başarısız modelde, çözüm arka uç öncelikle tek bir veri merkezi konumunda çalışır. İkincil bir IoT hub'ı ve arka uç başka bir veri merkezi konumunda dağıtılır. Birincil bölgedeki IoT hub'ı bir kesintiyle karşılanırsa veya aygıttan birincil bölgeye ağ bağlantısı kesintiye uğrarsa, aygıtlar ikincil bir hizmet bitiş noktası kullanır. Tek bir bölge içinde kalmak yerine bölgeler arası bir hata modeli uygulayarak çözüm kullanılabilirliğini artırabilirsiniz. 

Yüksek düzeyde, IoT Hub ile bölgesel bir başarısız model uygulamak için aşağıdaki adımları yapmanız gerekir:

* **İkincil bir IoT hub ve aygıt yönlendirme mantığı**: Birincil bölgenizdeki hizmet kesintiye uğrarsa, aygıtlar ikincil bölgenize bağlanmaya başlamalıdır. Söz konusu hizmetlerin çoğunun devlet tarafından farkında olması göz önüne alındığında, çözüm yöneticilerinin bölgeler arası başarısızlık işlemini tetiklemesi yaygındır. İşlemin denetimini korurken, yeni bitiş noktasını aygıtlara iletmenin en iyi yolu, geçerli etkin bitiş noktası için bir *konsiyerj* hizmetini düzenli olarak denetlemelerini sağlamaktır. Konsiyerj hizmeti, DNS yeniden yönlendirme teknikleri kullanılarak çoğaltılan ve erişilebilen bir web uygulaması olabilir (örneğin, [Azure Trafik Yöneticisi'ni](../traffic-manager/traffic-manager-overview.md)kullanarak).

   > [!NOTE]
   > IoT hub hizmeti, Azure Trafik Yöneticisi'nde desteklenen bir uç nokta türü değildir. Öneri, önerilen konsiyerj hizmetini son nokta sağlık sondası API'sini uygulayarak Azure trafik yöneticisiyle tümleştirmektir.

* **Kimlik kayıt çoğaltma**: Kullanılabilir olması için ikincil IoT hub'ı çözüme bağlanabilen tüm aygıt kimliklerini içermelidir. Çözüm, aygıt kimliklerinin coğrafi olarak çoğaltılan yedeklerini tutmalı ve aygıtlar için etkin bitiş noktasını değiştirmeden önce ikincil IoT hub'ına yüklemelidir. IoT Hub'ın aygıt kimlik dışa aktarma işlevi bu bağlamda yararlıdır. Daha fazla bilgi için [IoT Hub geliştirici kılavuzuna](iot-hub-devguide-identity-registry.md)bakın - kimlik kayıt defteri.

* **Birleştirme mantığı**: Birincil bölge yeniden kullanılabilir olduğunda, ikincil sitede oluşturulan tüm durum ve veriler birincil bölgeye geri geçirilmelidir. Bu durum ve veriler çoğunlukla birincil IoT hub'ı ve birincil bölgedeki uygulamaya özgü diğer mağazalarla birleştirilmesi gereken aygıt kimlikleri ve uygulama meta verileriyle ilgilidir. 

Bu adımı basitleştirmek için, idempotent işlemleri kullanmalısınız. İktidarlı operasyonlar, olayların nihai tutarlı dağılımının ve yinelenenlerin veya sıra dışı olayların tesliminin yan etkilerini en aza indirir. Buna ek olarak, uygulama mantığı olası tutarsızlıkları veya biraz güncel olmayan durumu tolere etmek için tasarlanmalıdır. Bu durum, sistemin kurtarma noktası hedeflerine (RPO) göre iyileşmesi için gereken ek süre nedeniyle oluşabilir.

## <a name="choose-the-right-hadr-option"></a>Doğru HA/DR seçeneğini seçin

Burada, çözümünüz için çalışan doğru seçeneği seçmek için başvuru çerçevesi olarak kullanılabilecek bu makalede sunulan HA/DR seçeneklerinin bir özeti verilmiştir.

| HA/DR seçeneği | Rto | RPO | Manuel müdahale gerektirir? | Uygulama karmaşıklığı | Ek maliyet etkisi|
| --- | --- | --- | --- | --- | --- |
| Microsoft tarafından başlatılan failover |2 - 26 saat|Yukarıdaki RPO tablosuna bakın|Hayır|None|None|
| El ile yük devretme |10 dk - 2 saat|Yukarıdaki RPO tablosuna bakın|Evet|Çok alçak. Bu operasyonu portaldan tetiklemeniz gerekiyor.|None|
| Çapraz bölge HA |< 1 dk|Özel HA çözümünüzün çoğaltma frekansına bağlıdır|Hayır|Yüksek|1 IoT hub'ının 1x > maliyeti|

## <a name="next-steps"></a>Sonraki adımlar

* [Azure IoT Hub nedir?](about-iot-hub.md)
* [IoT Hub'ları (Quickstart) ile başlayın](quickstart-send-telemetry-dotnet.md)
* [Öğretici: Bir IoT hub'ı için manuel hata yapma](tutorial-manual-failover.md)