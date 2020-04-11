---
title: Azure Akış Analizi'nde zaman işlemeyi anlama
description: Azure Akış Analizi'nde zaman işlemenin nasıl çalıştığını (en iyi başlangıç saatini seçme, geç ve erken olayları nasıl işleyeceğiniz ve zaman işleme ölçümleri gibi) öğrenin.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/05/2018
ms.openlocfilehash: 55537fb923b26de4e02be35fdb817dee147584d7
ms.sourcegitcommit: fb23286d4769442631079c7ed5da1ed14afdd5fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81115132"
---
# <a name="understand-time-handling-in-azure-stream-analytics"></a>Azure Akış Analizi'nde zaman işlemeyi anlama

Bu makalede, Azure Akış Analizi hizmetinde pratik zaman işleme sorunlarını çözmek için tasarım seçimleri nasıl yapabileceğinizi tartışıyoruz. Zaman işleme tasarım kararları, olay sıralama faktörleri ile yakından ilişkilidir.

## <a name="background-time-concepts"></a>Arka plan zaman kavramları

Tartışmayı daha iyi çerçevelemek için bazı arka plan kavramlarını tanımlayalım:

- **Olay zamanı**: Orijinal olayın gerçekleştiği saat. Örneğin, otobanda hareket eden bir araba gişelere yaklaştığında.

- **İşlem süresi**: Olayın işleme sistemine ulaştığı ve gözlemlendiği zaman. Örneğin, bir gişe sensörü arabayı gördüğünde ve bilgisayar sisteminin verileri işlemesi birkaç dakika sürer.

- **Filigran**: Akış işlemcisine hangi nokta olaylarının girildiğini gösteren bir olay zaman işaretidir. Filigranlar, sistemin olayları yutma konusunda net bir ilerleme göstermesini sağlar. Akışların doğası gereği, gelen olay verileri asla durmaz, bu nedenle filigranlar akıştaki belirli bir noktaya ilerlemeyi gösterir.

   Filigran kavramı önemlidir. Filigranlar, Stream Analytics'in sistemin ne zaman geri çekilmesi gerekmeyen tam, doğru ve tekrarlanabilir sonuçlar üretebileceğini belirlemesine olanak sağlar. İşlem, öngörülebilir ve tekrarlanabilir garantili bir şekilde yapılabilir. Örneğin, bazı hata işleme durumu için yeniden sayım yapılması gerekiyorsa, filigranlar güvenli başlangıç ve bitiş noktalarıdır.

Bu konuda ek kaynaklar olarak, Tyler Akidau's blog mesajları [Streaming 101](https://www.oreilly.com/ideas/the-world-beyond-batch-streaming-101) ve [Streaming 102](https://www.oreilly.com/ideas/the-world-beyond-batch-streaming-102)bakın.

## <a name="choosing-the-best-starting-time"></a>En iyi başlangıç saatini seçme

Stream Analytics, kullanıcılara etkinlik süresini seçmek için iki seçenek sunar:

1. **Varış saati**  

   Varış saati, olay kaynağa ulaştığında giriş kaynağına atanır. Olay Hub'ları girişleri için **EventEnqueuedUtcTime** özelliğini, IoT Hub için **IoTHub.EnqueuedTime** özelliğini ve blob girişi için **BlobProperties.LastModi** özelliğini kullanarak varış saatine erişebilirsiniz.

   Varış süresini kullanmak varsayılan davranıştır ve zamansal mantığın gerekli olmadığı veri arşivleme senaryoları için en iyi şekilde kullanılır.

2. **Uygulama süresi** (Etkinlik Saati olarak da adlandırılır)

   Olay oluşturulduğunda uygulama süresi atanır ve olay yükünün bir parçasıdır. Olayları uygulama süresine göre işlemek için, seç sorgusundaki **zaman damgasını** yan tümceye göre kullanın. Maddeye göre **Zaman Damgası** yoksa, olaylar varış saatine göre işlenir.

   Zamansal mantık söz konusu olduğunda yükte zaman damgası kullanmak önemlidir. Bu şekilde, kaynak sistemdeki veya ağdaki gecikmeler hesaba katılabilir.

## <a name="how-time-progresses-in-azure-stream-analytics"></a>Azure Akış Analizi'nde zaman nasıl ilerler?

Uygulama süresini kullanırken, zaman ilerlemesi gelen olayları temel adatır. Akış işleme sisteminin olay olup olmadığını veya olayların gecikip geciktirilmediğini bilmesi zordur. Bu nedenle, Azure Akış Analizi her giriş bölümü için aşağıdaki yollarla sezgisel filigranlar oluşturur:

1. Ne zaman gelen bir olay olsa, filigran şimdiye kadar gördüğümüz en büyük olay zamanı, sıra dışı tolerans penceresi boyutu ndan uzaktır.

2. Gelen bir olay olmadığında filigran, geçerli tahmini varış süresidir (vm'nin arkasında geçen süre, bir giriş olayının son görüldüğünden olayları işleme ve giriş olayının varış süresi) eksi geç varış tolerans penceresidir.

   Gerçek varış saati, olayları işleyen Azure Akış Analizi VM'si değil, Olay Hub'ları gibi giriş olay aracısında oluşturulduğundan, varış süresi yalnızca tahmin edilebilir.

Tasarım filigran oluşturmanın yanı sıra iki ek amama hizmet eder:

1. Sistem, gelen olaylarla veya gelmeden zamanında sonuç üretir.

   Çıktı sonuçlarını ne zaman görmek istediklerini kontrol edebilirsiniz. Azure portalında, Akış Analizi işinin **Etkinlik sıralama** **sayfasında, Sıra Dışı olaylar** ayarını yapılandırabilirsiniz. Bu ayarı yapılandırırken, olay akışındaki sıra dışı olaylara tolerans gösteren güncellik lerin dengesini göz önünde bulundurun.

   Geç varış tolerans penceresi gelen olayların yokluğunda bile filigran oluşturmaya devam etmek önemlidir. Bazen, bir olay giriş akışı seyrek olduğu gibi, gelen olayların gelmediği bir dönem olabilir. Bu sorun, giriş olay aracısında birden çok bölüm kullanımı ile daha da şiddetlenir.

   Geç varış tolerans penceresi olmadan veri işleme sistemleri akışı, girişler seyrek olduğunda ve birden fazla bölüm kullanıldığında gecikmiş çıktılardan muzdarip olabilir.

2. Sistem davranışı tekrarlanabilir olmalıdır. Tekrarlanabilirlik, akışlı veri işleme sisteminin önemli bir özelliğidir.

   Filigran varış saati ve uygulama süresinden türetilmiştir. Her ikisi de olay komisyoncu su anda devam eder ve böylece tekrarlanabilir. Olayların yokluğunda varış saatinin tahmin edilmesi gerektiğinde, Azure Akış Analizi, hata kurtarma amacıyla tekrarlanabilirlik sırasında tekrarlanabilirlik için tahmini varış saatini günlükler.

**Varış saatini** etkinlik saati olarak kullanmayı seçtiğinizde, sipariş dışı toleransı ve geç varış toleransını yapılandırmaya gerek olmadığına dikkat edin. Giriş etkinliği aracısında **varış süresinin** monoton olarak artması garanti olduğundan, Azure Akış Analizi yapılandırmaları göz ardı eder.

## <a name="late-arriving-events"></a>Geç gelen etkinlikler

Azure Akış Analizi, gelen her etkinlik için geç varış tolerans penceresi nin tanımıolarak **etkinlik süresini** **varış saati**ile karşılaştırır; Olay süresi tolerans penceresinin dışındaysa, sistemi olayı bırakacak veya olayın zamanını tolerans içinde olacak şekilde ayarlayabilirsiniz.

Filigranlar oluşturulduktan sonra, hizmetin filigrandan daha düşük olay süresine sahip olaylar alabileceğini göz önünde bulundurun. Hizmeti, bu olayları **bırakacak** veya olayın zamanını filigran değerine **ayarlayabilirsin.**

Ayarlamanın bir parçası olarak, etkinliğin **System.Timestamp'ı** yeni değere ayarlanır, ancak **olay zaman** alanının kendisi değiştirilmez. Bu ayarlama, bir olayın **System.Timestamp'Inin** olay zaman alanındaki değerden farklı olabileceği ve beklenmeyen sonuçların oluşturulmasına neden olabileceği tek durumdur.

## <a name="handling-time-variation-with-substreams"></a>Alt akışlarla zaman değişimini işleme

Burada açıklanan buluşsal filigran oluşturma mekanizması, zamanın çoğunlukla çeşitli olay gönderenler arasında senkronize edildiği vakaların çoğunda iyi çalışır. Ancak, gerçek hayatta, özellikle birçok IoT senaryolarında, sistem olay gönderenler üzerinde saat üzerinde çok az kontrole sahiptir. Olay gönderenler, belki de donanım ve yazılımın farklı sürümlerinde, alanında cihazların her türlü olabilir.

Akış Analizi, giriş bölümündeki tüm olaylar için filigran globalkullanmak yerine, size yardımcı olacak alt akışlar adı verilen başka bir mekanizmaya sahiptir. [**TIMESTAMP BY**](/stream-analytics-query/timestamp-by-azure-stream-analytics) yan tümcesini ve **OVER**anahtar kelimesini kullanan bir iş sorgusu yazarak işinizdeki alt akışları kullanabilirsiniz. Alt akışı belirlemek için, **over** anahtar kelimesinden sonra bir `deviceid`anahtar sütun adı sağlayın, örneğin , sistemin zaman ilkelerini o sütuna göre uyguladığını. Her alt akım kendi bağımsız filigranalır. Bu mekanizma, olay gönderenler arasında büyük saat eğrilikleri veya ağ gecikmeleri ile uğraşırken, zamanında çıktı üretimine izin vermek için yararlıdır.

Alt akışlar Azure Akış Analizi tarafından sağlanan benzersiz bir çözümdür ve diğer veri akış sistemleri tarafından sunulmaz. Stream Analytics, alt akışlar kullanıldığında gelen olaylara geç varış toleransı penceresini uygular. Varsayılan ayar (5 saniye) büyük olasılıkla farklı zaman damgaları olan aygıtlar için çok küçüktür. 5 dakika ile başlamanızı ve cihaz saati eğrilme desenine göre ayarlamalar yapmanızı öneririz.

## <a name="early-arriving-events"></a>Erken gelen olaylar

Erken varış penceresi denilen başka bir kavram fark etmiş olabilirsiniz, geç varış tolerans penceresinin tersi gibi görünüyor. Bu pencere 5 dakika sabitlenir ve geç varış bir farklı bir amaca hizmet eder.

Azure Akış Analizi her zaman tam sonuç ürettiğini garanti ettiği için, **iş başlangıç saatini** giriş süresi olarak değil, yalnızca işin ilk çıktı zamanı olarak belirtebilirsiniz. İş başlangıç saati, pencerenin tamamının sadece pencerenin ortasından değil, işlenmesi için gereklidir.

Akış Analizi daha sonra başlangıç saatini sorgu belirtiminden türeter. Ancak, giriş olay komisyoncusu yalnızca varış saatine göre dizine eklenmiştir, çünkü sistem başlangıç olay saatini varış saatine çevirmek zorundadır. Sistem, giriş olay aracısındaki bu noktadan olayları işlemeye başlayabilir. Erken gelen pencere sınırı ile çeviri basittir. Bu başlangıç olay süresi eksi 5 dakika erken gelen penceredir. Bu hesaplama aynı zamanda sistemin varış saatinden 5 dakika daha yüksek bir olay süresine sahip olarak görülen tüm olayları düşüreceği anlamına gelir.

Bu kavram, nereden çıktı almaya başlarsanız başlayın, işlemin yinelenebilir olmasını sağlamak için kullanılır. Böyle bir mekanizma olmadan, diğer birçok akış sisteminin iddia ettikleri gibi tekrarlanabilirliği garanti etmek mümkün olmaz.

## <a name="side-effects-of-event-ordering-time-tolerances"></a>Olay sipariş zaman toleranslarının yan etkileri

Akış Analizi işlerinin çeşitli **Etkinlik sıralama** seçenekleri vardır. İki tane Azure portalında yapılandırılabilir: Sıra dışı olaylar ayarı **(sıra** dışı tolerans) ve **geç ayarı gelen Olaylar** (geç varış toleransı). **Erken varış** toleransı sabittir ve ayarlanamaz. Bu zaman ilkeleri, Stream Analytics tarafından güçlü garantiler sağlamak için kullanılır. Ancak, bu ayarların bazen beklenmeyen bazı etkileri vardır:

1. Yanlışlıkla çok erken olayları gönderiyorum.

   Erken olaylar normal çıktı olmamalıdır. Gönderenin saati çok hızlı çalışıyorsa, erken olayların çıktıya gönderilmesi mümkündür. Tüm erken gelen olaylar bırakılır, bu nedenle çıktı dan bunların hiçbirini görmezsiniz.

2. Azure Akış Analizi tarafından işlenecek eski etkinlikleri Etkinlik Hub'larına gönderme.

   Eski olaylar ilk başta zararsız gibi görünse de, geç varış toleransının uygulanması nedeniyle, eski olaylar iptal edilebilir. Olaylar çok eskiyse, Olay alma sırasında **System.Timestamp** değeri değiştirilir. Bu davranış nedeniyle, şu anda Azure Akış Analizi, geçmiş olay işleme senaryoları yerine neredeyse gerçek zamanlı olay işleme senaryoları için daha uygundur. Bazı durumlarda bu davranışın etrafında çalışmak üzere **geç gelen Olayları** mümkün olan en büyük değere (20 gün) ayarlayabilirsiniz.

3. Çıktılar gecikiyor gibi görünüyor.

   İlk filigran hesaplanan zamanda oluşturulur: sistemin şimdiye kadar gözlemlediği **maksimum olay süresi,** sıra dışı tolerans penceresi boyutu hariç. Varsayılan olarak, sıra dışı tolerans sıfıra (00 dakika ve 00 saniye) yapılandırılır. Daha yüksek, sıfır olmayan bir zaman değerine ayarladığınızda, akış işinin ilk çıktısı, hesaplanan ilk filigran süresi nedeniyle bu zaman değeri (veya daha büyük) tarafından geciktirilir.

4. Girişler seyrektir.

   Belirli bir bölümde giriş olmadığında filigran süresi varış **saati** eksi geç varış tolerans penceresi olarak hesaplanır. Sonuç olarak, giriş olayları seyrek ve seyrek sayılsa, çıktı bu süre yle gecikebilir. Geç **değere gelen** varsayılan Olaylar 5 saniyedir. Örneğin, giriş olaylarını teker teker gönderirken biraz gecikme görmeyi beklemelisiniz. **Geç gelen olayları** büyük bir değere ayarladığınızda gecikmeler daha da kötüleşebilir.

5. **System.Timestamp** değeri **olay zaman** alanında zaman farklıdır.

   Daha önce açıklandığı gibi, sistem etkinlik süresini sıra dışı toleransa veya geç varış tolerans pencerelerine göre ayarlar. Olayın **System.Timestamp** değeri ayarlanır, ancak olay **zaman** alanı ayarlanmaz.

## <a name="metrics-to-observe"></a>Gözlemlemek için ölçümler

[Akış Analizi iş ölçümleri](stream-analytics-monitoring.md)aracılığıyla olay sipariş zaman toleransı efektleri bir dizi gözlemleyebilirsiniz. Aşağıdaki ölçümler ilgilidir:

|Ölçüm  | Açıklama  |
|---------|---------|
| **Sıra dışı Etkinlikler** | Sıra dışı olarak alınan ve bırakılan veya ayarlanmış bir zaman damgası verilen olay sayısını gösterir. Bu metrik, Azure portalındaki işteki **Etkinlik siparişi** sayfasında sıra **dışı olaylar** ayarı yapılandırması tarafından doğrudan etkilenir. |
| **Geç Giriş Olayları** | Kaynaktan geç gelen olayların sayısını gösterir. Bu metrik, bırakılan veya zaman damgası ayarlanan olayları içerir. Bu metrik, Azure portalındaki iş teki **Etkinlik sıralama** sayfasında geç **gelen Etkinliklerin** yapılandırması tarafından doğrudan etkilenir. |
| **Erken Giriş Olayları** | Kaynaktan erken gelen olayların sayısını gösterir veya 5 dakikadan daha erken yse zaman damgası ayarlanır. |
| **Filigran Gecikmesi** | Akış veri işleme işinin gecikmesini gösterir. Aşağıdaki bölümde daha fazla bilgi bakın.|

## <a name="watermark-delay-details"></a>Filigran Gecikme ayrıntıları

**Filigran gecikme** ölçümü, işleme düğümünün duvar saati saati eksi şimdiye kadar gördüğü en büyük filigran olarak hesaplanır. Daha fazla bilgi için [filigran gecikme blog yazısına](https://azure.microsoft.com/blog/new-metric-in-azure-stream-analytics-tracks-latency-of-your-streaming-pipeline/)bakın.

Bu metrik değerin normal çalışma altında 0'dan büyük olmasının birkaç nedeni olabilir:

1. Akış ardışık akışının doğal işlem gecikmesi. Normalde bu gecikme nominal.

2. Filigran tolerans penceresinin boyutuna göre azaltıldığı için, sıra dışı tolerans penceresi gecikmeye neden oldu.

3. Filigran tolerans penceresi boyutu tarafından azaltılır, çünkü geç varış penceresi gecikme tanıttı.

4. Metrik oluşturan işleme düğümünün saat eğriliği.

Akış ardışık akışının yavaşlamasına neden olabilecek bir dizi başka kaynak kısıtlaması vardır. Filigran gecikme ölçümü aşağıdaki nedenlerden dolayı artabilir:

1. Akış Analizi'nde giriş olaylarının hacmini işlemek için yeterli işlem kaynağı yok. Kaynakları ölçeklendirmek için Akış [Birimlerini Anla ve Ayarla'ya](stream-analytics-streaming-unit-consumption.md)bakın.

2. Giriş olay brokerları içinde yeterli iş kadar, bu yüzden daraltılır. Olası çözümler için bkz. [Azure Olay Hub'ları iş bulma birimlerini otomatik olarak ölçeklendirin.](../event-hubs/event-hubs-auto-inflate.md)

3. Çıktı lavaboları yeterli kapasiteile sağlanmaz, bu nedenle daraltılırlar. Olası çözümler, kullanılan çıktı hizmetinin lezzetine bağlı olarak büyük farklılıklar gösterir.

## <a name="output-event-frequency"></a>Çıkış olay frekansı

Azure Akış Analizi, çıktı olayları üretmek için tek tetikleyici olarak filigran ilerlemesini kullanır. Filigran giriş verilerinden türetildiği için, hata kurtarma sırasında ve kullanıcı tarafından başlatılan yeniden işleme sırasında yinelenebilir.

[Pencereli agregalar](stream-analytics-window-functions.md)kullanırken, hizmet yalnızca pencerelerin sonunda çıktı üretir. Bazı durumlarda, kullanıcılar pencerelerden oluşturulan kısmi toplamları görmek isteyebilir. Azure Akış Analizi'nde kısmi toplamlar şu anda desteklenmez.

Diğer akış çözümlerinde, çıktı olayları dış koşullara bağlı olarak çeşitli tetik noktalarında somutlaştırılabilir. Bazı çözümlerde, belirli bir zaman penceresi için çıktı olaylarının birden çok kez oluşturulabileceği mümkündür. Giriş değerleri rafine edildikçe, toplam sonuçlar daha doğru hale gelir. Olaylar ilk başta spekülasyon olabilir ve zaman içinde revize edilebilir. Örneğin, belirli bir aygıt ağdan çevrimdışı olduğunda, tahmini bir değer bir sistem tarafından kullanılabilir. Daha sonra, aynı cihaz ağa çevrimiçi olarak gelir. Sonra gerçek olay verileri giriş akışına dahil edilebilir. Bu zaman penceresiişleme çıktı sonuçları daha doğru çıktı üretir.

## <a name="illustrated-example-of-watermarks"></a>Filigran resimli örnek

Aşağıdaki resimler, filigranların farklı koşullarda nasıl ilerlediğini göstermektedir.

Bu tablo, aşağıda grafiklenen örnek verileri gösterir. Olay zamanı nın ve varış saatinin değiştiğine, bazen eşleşip bazen de değişmediğini unutmayın.

| Etkinlik zamanı | Varış saati | DeviceId |
| --- | --- | --- |
| 12:07 | 12:07 | cihaz1
| 12:08 | 12:08 | cihaz2
| 12:17 | 12:11 | cihaz1
| 12:08 | 12:13 | cihaz3
| 12:19 | 12:16 | cihaz1
| 12:12 | 12:17 | cihaz3
| 12:17 | 12:18 | cihaz2
| 12:20 | 12:19 | cihaz2
| 12:16 | 12:21 | cihaz3
| 12:23 | 12.22 | cihaz2
| 12.22 | 12:24 | cihaz2
| 12:21 | 12:27 | cihaz3

Bu resimde aşağıdaki toleranslar kullanılır:

- Erken varış pencereleri 5 dakikadır
- Geç gelen pencere 5 dakikadır
- Yeniden sipariş penceresi 2 dakikadır

1. Filigran Bu olaylar arasında ilerleyen İllüstrasyon:

   ![Azure Akış Analizi filigran illüstrasyon](media/stream-analytics-time-handling/WaterMark-graph-1.png)

   Önceki grafikte gösterilen önemli işlemler:

   1. İlk olay (device1) ve ikinci olay (device2) hizalanmış kez ve ayarlamalar olmadan işlenir. Filigran her olay da ilerler.

   2. Üçüncü olay (device1) işlendiğinde, varış saati (12:11) olay saatinden (12:17) önce gelir. Olay 6 dakika erken geldi, bu yüzden olay 5 dakikalık erken varış toleransı nedeniyle düştü.

      Filigran erken bir olay bu durumda ilerleme yok.

   3. Dördüncü olay (device3) ve beşinci olay (device1) hizalanmış kez ve ayar olmadan işlenir. Filigran her olay da ilerler.

   4. Altıncı olay (device3) işlendiğinde, varış saati (12:17) ve etkinlik saati (12:12) filigran seviyesinin altındadır. Olay süresi su işareti seviyesine göre ayarlanır (12:17).

   5. On ikinci olay (device3) işlendiğinde, varış saati (12:27) etkinlik saatinden 6 dakika ileridedir (12:21). Geç varış politikası uygulanır. Olay süresi (12:22) filigran (12:21) üzerinde, böylece başka bir ayarlama uygulanır ayarlanır.

2. Filigran Erken varış politikası olmadan ilerleyen ikinci örnek:

   ![Azure Akışı Analytics erken ilke filigran illüstrasyonu yok](media/stream-analytics-time-handling/watermark-graph-2.png)

   Bu örnekte, erken varış politikası uygulanmaz. Erken gelen aykırı olaylar filigranı önemli ölçüde yükseltir. Üçüncü olayın (deviceId1 saat 12:11) bu senaryoda düşmediğini ve filigranın 12:15'e yükseltildiğine dikkat edin. Dördüncü olay süresi, sonuç olarak 7 dakika (12:08-12:15) olarak ayarlanır.

3. Son resimde, alt akımlar (OVER the DeviceId) kullanılır. Birden fazla filigran izlenir, akış başına bir. Sonuç olarak zamanları ayarlanmış daha az olay vardır.

   ![Azure Akış Analizi filigran çizimini alt akışları](media/stream-analytics-time-handling/watermark-graph-3.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Akışı Analytics etkinlik siparişi hususları](stream-analytics-out-of-order-and-late-events.md)
- [Akış Analizi iş ölçümleri](stream-analytics-monitoring.md)
