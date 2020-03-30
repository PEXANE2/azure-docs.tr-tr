---
title: Azure Monitör'de günlük uyarıları
description: Belirttiğiniz analitik sorgu koşulları Azure Uyarıları için karşılandığında e-postaları, bildirimleri, çağrı web sitelerini URL'leri (webhooks) veya otomasyonu tetikleyin.
author: yanivlavi
ms.author: yalavi
ms.topic: conceptual
ms.date: 5/31/2019
ms.subservice: alerts
ms.openlocfilehash: a6abf4665c27771497037da35f85bb540e6e904e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77665230"
---
# <a name="log-alerts-in-azure-monitor"></a>Azure Monitör'de günlük uyarıları

Bu makalede, Log uyarılarının ayrıntıları [Azure Uyarıları](../../azure-monitor/platform/alerts-overview.md) içinde desteklenen uyarı türlerinden biridir ve kullanıcıların uyarı için temel olarak Azure'un analiz platformlarını kullanmasına olanak tanır.

Günlük Uyarısı, Azure Monitör [Günlükleri](../../azure-monitor/learn/tutorial-viewdata.md) veya [Uygulama Öngörüleri](../../azure-monitor/app/cloudservices.md#view-azure-diagnostics-events)için oluşturulan Günlük Arama kurallarından oluşur. Kullanımı hakkında daha fazla bilgi edinmek için [Azure'da günlük uyarıları oluşturmaya](../../azure-monitor/platform/alerts-log.md) bakın

> [!NOTE]
> Azure Monitor [Günlükleri'ndeki](../../azure-monitor/learn/tutorial-viewdata.md) popüler günlük verileri artık Azure Monitor'daki metrik platformda da kullanılabilir. Ayrıntılar için görünüm, [Günlükler için Metrik Uyarı](../../azure-monitor/platform/alerts-metric-logs.md)


## <a name="log-search-alert-rule---definition-and-types"></a>Günlük arama uyarısı kuralı - tanımı ve türleri

Belirtilen günlük sorgularını düzenli aralıklarla otomatik olarak çalıştırmak için, Azure Uyarıları tarafından günlük arama kuralları oluşturulur.  Günlük sorgusunun sonuçları belirli ölçütlerle eşleşiyorsa bir uyarı kaydı oluşturulur. Ardından kural, [Eylem Grupları](../../azure-monitor/platform/action-groups.md)'nı kullanıp bir veya birden çok eylemi otomatik olarak çalıştırabilir. Günlük uyarıları oluşturmak, değiştirmek ve güncelleştirmek için [Azure İzleme Katılımcısı](../../azure-monitor/platform/roles-permissions-security.md) rolü gerekebilir; uyarı kuralı veya uyarı sorgusunda analitik hedef(ler) için erişim & sorgu yürütme hakları ile birlikte. Kullanıcı oluşturma uyarı kuralı veya uyarı sorgusunda tüm analitik hedef(ler) erişimi yoksa - kural oluşturma başarısız olabilir veya günlük uyarı kuralı kısmi sonuçlarla yürütülür.

Günlük arama kuralları aşağıdaki ayrıntılarla tanımlanır:

- **Günlük Sorgusu**.  Uyarı kuralı her tetiklendiğinde çalıştırılan sorgu.  Bu sorgu tarafından döndürülen kayıtlar, bir uyarının tetiklenip tetiklenmeyeceğini belirlemek için kullanılır. Analytics sorgusu belirli bir Log Analytics çalışma alanı veya Application Insights uygulaması için olabilir ve hatta kullanıcının tüm kaynaklara erişim ve sorgu hakları sağlamaları koşuluyla [birden fazla Log Analytics ve Application Insights kaynaklarına](../../azure-monitor/log-query/cross-workspace-query.md#querying-across-log-analytics-workspaces-and-from-application-insights) yayılabilir. 
    > [!IMPORTANT]
    > Yalnızca [scheduledQueryRules API kullanılarak yapılandırılan Log Analytics](../../azure-monitor/platform/alerts-log-api-switch.md) için Uygulama Öngörüleri ve günlük uyarıları için günlük uyarılarında [çapraz kaynak sorgu](../../azure-monitor/log-query/cross-workspace-query.md#querying-across-log-analytics-workspaces-and-from-application-insights) desteği.

    Bazı analitik komutlar ve kombinasyonlar günlük uyarılarındaki kullanımla uyumsuzdur; daha fazla ayrıntı görüntülemek için, [Azure Monitor'da uyarı sorgularını günlüğe kaydedin.](../../azure-monitor/platform/alerts-log-query.md)

- **Zaman Dilimi**.  Sorguiçin zaman aralığını belirtir. Sorgu yalnızca bu geçerli zaman aralığı içinde oluşturulmuş olan kayıtları döndürür. Zaman dilimi kötüye kullanımı önlemek için günlük sorgusu için getirilen verileri kısıtlar ve günlük sorgusunda kullanılan herhangi bir zaman komutu (önceki gibi) atlatmak. <br>*Örneğin, zaman dilimi 60 dakika olarak ayarlanırsa ve sorgu 13:15'te çalıştırılırsa, günlük sorgusunu yürütmek için yalnızca 12:15 ile 13:15 arasında oluşturulan kayıtlar döndürülür. Şimdi, günlük sorgusu önceki gibi zaman komutunu kullanırsa (7d), günlük sorgusu yalnızca 12:15 ile 13:15 arasındaki veriler için çalıştırılacak - sanki veriler yalnızca son 60 dakika için var. Ve günlük sorgusunda belirtildiği gibi yedi günlük veri için değil.*

- **Frekans**.  Sorgunun ne sıklıkta çalıştırılması gerektiğini belirtir. 5 dakika ile 24 saat arasında herhangi bir değer olabilir. Zaman periyoduna eşit veya daha az olmalıdır.  Değer zaman döneminden büyükse, kayıtların kaçırılma riskini göze alabilirsiniz.<br>*Örneğin, 30 dakikalık bir zaman dilimi ve 60 dakikalık bir sıklık düşünün.  Sorgu saat 13:00'te çalıştırılırsa, 12:30 ile 13:00 arasında kayıtları döndürür.  Sorgunun bir sonraki çalışması, 1:30 ile 2:00 arasında kayıtları döndüreceği saat2:00'dir.  1:00 ile 1:30 arasında oluşturulan kayıtlar asla değerlendirilmez.*

- **Eşik**.  Günlük aramasonuçları bir uyarı oluşturulup oluşturulmaması gerektiğini belirlemek için değerlendirilir.  Eşik, farklı türde günlük arama uyarısı kuralları için farklıdır.

[Azure Monitör Günlükleri](../../azure-monitor/learn/tutorial-viewdata.md) veya Uygulama [Öngörüleri](../../azure-monitor/app/cloudservices.md#view-azure-diagnostics-events)için günlük arama kuralları iki tür olabilir. Bu türlerin her biri, izleyen bölümlerde ayrıntılı olarak açıklanmıştır.

- **[Sonuç sayısı.](#number-of-results-alert-rules)** Günlük araması tarafından döndürülen sayı kayıtları belirtilen sayıyı aştığında oluşturulan tek uyarı.
- **[Metrik ölçüm](#metric-measurement-alert-rules)**.  Belirtilen eşiği aşan değerlerle günlük arama sonuçlarında her nesne için oluşturulan uyarı.

Uyarı kural türleri arasındaki farklar aşağıdaki gibidir.

- Sonuç uyarı *kurallarının sayısı* her zaman tek bir uyarı oluştururken, *Metrik ölçüm* uyarı kuralı eşiği aşan her nesne için bir uyarı oluşturur.
- *Eşik* tek bir kez aşıldığında sonuç uyarı kuralları sayısı bir uyarı oluşturur. *Metrik ölçüm* uyarı kuralları, eşik belirli bir zaman aralığı nda belirli bir sayıda aşıldığında bir uyarı oluşturabilir.

### <a name="number-of-results-alert-rules"></a>Sonuç uyarı kurallarının sayısı

**Arama** sorgusu tarafından döndürülen kayıt sayısı belirtilen eşiği aştığında sonuç uyarı kuralları nın sayısı tek bir uyarı oluşturur. Bu tür bir uyarı kuralı, Windows olay günlükleri, Syslog, WebApp Yanıtı ve Özel günlükler gibi etkinliklerle çalışmak için idealdir.  Belirli bir hata olayı oluşturulduğunda veya belirli bir zaman dilimi içinde birden çok hata olayı oluşturulduğunda bir uyarı oluşturmak isteyebilirsiniz.

**Eşik**: Sonuç sayısı uyarı kurallarının eşik değeri belirli bir değerden büyük veya daha azdır.  Günlük araması tarafından döndürülen kayıt sayısı bu ölçütle eşleşirse, bir uyarı oluşturulur.

Tek bir olay da uyarmak için, sonuç sayısını 0'dan büyük olarak ayarlayın ve sorgunun çalıştırıldığı son tarihten bu yana oluşturulan tek bir olayın oluşumunu denetleyin. Bazı uygulamalar, mutlaka bir uyarı yükseltmek gerekir zaman zaman bir hata günlük olabilir.  Örneğin, uygulama hata olayı oluşturulan işlemi yeniden deneebilir ve sonra bir sonraki kez başarılı.  Bu durumda, belirli bir zaman dilimi içinde birden çok olay oluşturulmadığı sürece uyarıyı oluşturmak istemeyebilirsiniz.  

Bazı durumlarda, bir olayın yokluğunda bir uyarı oluşturmak isteyebilirsiniz.  Örneğin, bir işlem düzgün çalıştığını belirtmek için düzenli olayları günlüğe kaydedebilir.  Belirli bir zaman dilimi içinde bu olaylardan birini kaydetmiyorsa, bir uyarı oluşturulmalıdır.  Bu durumda, eşiği **1'den az**olarak ayarlarsınız.

#### <a name="example-of-number-of-records-type-log-alert"></a>Kayıt Sayısı türü günlük uyarısı örneği

Web tabanlı Uygulamanızın kod 500 (diğer bir diğer) Dahili Sunucu Hatası olan kullanıcılara yanıt verdiğini bilmek istediğiniz bir senaryo düşünün. Aşağıdaki ayrıntıları içeren bir uyarı kuralı oluşturursunuz:  

- **Sorgu:** istekler | nerede sonuçKodu == "500"<br>
- **Süre:** 30 dakika<br>
- **Uyarı frekansı:** beş dakika<br>
- **Eşik değeri:** 0'dan büyük<br>

Daha sonra uyarı, sonuç kodunun 500 olduğu herhangi bir kaydı aramak için 30 dakikalık veriyle birlikte sorguyu her 5 dakikada bir çalıştırır. Böyle bir kayıt bile bulunursa, uyarıyı tetikler ve yapılandırılan eylemi tetikler.

### <a name="metric-measurement-alert-rules"></a>Metrik ölçüm uyarı kuralları

**Metrik ölçüm** uyarı kuralları, sorgudaki her nesne için, belirtilen eşiği ve belirtilen tetikleyici koşulu aşan bir değere sahip bir uyarı oluşturur. Sonuç uyarı **kurallarının sayısından** farklı olarak, analitik sonuç bir zaman serisi sağladığında **Metrik ölçüm** uyarı kuralları çalışır. **Sonuç sayısı** uyarı kurallarından aşağıdaki farklı farklara sahiptirler.

- **Toplam fonksiyonu**: Gerçekleştirilen hesaplamayı ve potansiyel olarak sayısal bir alanın toplamını belirler.  Örneğin, **count()** sorgudaki kayıt sayısını döndürür, **avg (CounterValue)** aralık üzerinden CounterValue alanının ortalamasını döndürür. Sorgudaki toplam işlev, adlandırılmış/adlandırılmalıdır: AggregatedValue ve sayısal bir değer sağlar. 

- **Grup Alanı**: Bu alanın her örneği için toplu değeri olan bir kayıt oluşturulur ve her biri için bir uyarı oluşturulabilir.  Örneğin, her bilgisayar için bir uyarı oluşturmak istiyorsanız, **Bilgisayar tarafından**kullanırsınız. Durumda, uyarı sorgusunda belirtilen birden çok grup alanı varsa, kullanıcı **Toplamı Açık** (metricColumn) parametresini kullanarak sonuçları sıralamak için hangi alanın kullanılacağını belirtebilir

    > [!NOTE]
    > *Yalnızca* [scheduledQueryRules API kullanılarak yapılandırılan Log Analytics](../../azure-monitor/platform/alerts-log-api-switch.md) için Uygulama Öngörüleri ve günlük uyarıları için Metrik Ölçüm türü günlük uyarıları için Aggregate On (metricColumn) seçeneği mevcuttur.

- **Interval**: Verilerin toplandığı zaman aralığını tanımlar.  Örneğin, **beş dakika**belirtirseniz, uyarı için belirtilen süre boyunca 5 dakikalık aralıklarla toplanan grup alanının her örneği için bir kayıt oluşturulur.

    > [!NOTE]
    > Aralığı belirtmek için sorguda bin işlevi kullanılmalıdır. Bin() eşit olmayan zaman aralıklarına neden olabileceğinden - Alert, sabit bir noktayla sonuç elde etmek için çalışma zamanında uygun süreyle depo kutusu komutunu otomatik olarak bin_at komutuna dönüştürür. Günlük uyarısının metrik ölçüm türü, en fazla üç kutu() komutu örneği olan sorgularla çalışacak şekilde tasarlanmıştır
    
- **Eşik**: Metrik ölçüm uyarı kuralları için eşik, toplam değer ve bir dizi ihlalle tanımlanır.  Günlük aramasında ki herhangi bir veri noktası bu değeri aşarsa, bu bir ihlal olarak kabul edilir.  Sonuçlardaki herhangi bir nesne için ihlal sayısı belirtilen değeri aşarsa, o nesne için bir uyarı oluşturulur.

*Aggregate On* veya *metricColumn* seçeneğinin yanlış yapılandırılması uyarı kurallarının yanlış atlatır. Daha fazla bilgi [için, metrik ölçüm uyarı kuralı yanlış olduğunda sorun giderme'ye](alert-log-troubleshoot.md#metric-measurement-alert-rule-is-incorrect)bakın.

#### <a name="example-of-metric-measurement-type-log-alert"></a>Metrik Ölçüm türü günlük uyarısı örneği

Herhangi bir bilgisayar 30 dakika içinde üç kez% 90 işlemci kullanımı aştı eğer bir uyarı istedim bir senaryo düşünün.  Aşağıdaki ayrıntıları içeren bir uyarı kuralı oluşturursunuz:  

- **Sorgu:** Perf | nerede ObjectName == "İşlemci" ve CounterName == "% İşlemci Süresi" | summarize AggregatedValue = avg(CounterValue) by bin(TimeGenerated, 5m), Bilgisayar<br>
- **Süre:** 30 dakika<br>
- **Uyarı frekansı:** beş dakika<br>
- **Uyarı Mantığı - Koşul & Eşik:** 90'dan büyük<br>
- **Grup Alanı (Toplam-on):** Bilgisayar
- **Tetikleyici uyarı ya göre:** Toplam ihlaller 2'den büyük<br>

Sorgu, 5 dakikalık aralıklarla her bilgisayar için ortalama bir değer oluşturur.  Bu sorgu, önceki 30 dakika boyunca toplanan veriler için her 5 dakikada bir çalıştırılır. Grup Alanı (Aggregate-on) seçilen columnar 'Bilgisayar' olduğundan - AggregatedValue 'Bilgisayar' çeşitli değerler için bölünmüş ve her bilgisayar için ortalama işlemci kullanımı 5 dakikalık bir süre için belirlenir.  (say) üç bilgisayar için örnek sorgu sonucu, aşağıdaki gibi olacaktır.


|Zaman Oluşturuldu [UTC] |Bilgisayar  |Toplam Değer  |
|---------|---------|---------|
|20xx-xxT01:00:00Z     |   srv01.contoso.com      |    72     |
|20xx-xxT01:00:00Z     |   srv02.contoso.com      |    91     |
|20xx-xxT01:00:00Z     |   srv03.contoso.com      |    83     |
|...     |   ...      |    ...     |
|20xx-xxT01:30:00Z     |   srv01.contoso.com      |    88     |
|20xx-xxT01:30:00Z     |   srv02.contoso.com      |    84     |
|20xx-xxT01:30:00Z     |   srv03.contoso.com      |    92     |

Sorgu sonucu çizilecek olsaydı, bu şekilde görünür.

![Örnek sorgu sonuçları](media/alerts-unified-log/metrics-measurement-sample-graph.png)

Bu örnekte, her üç bilgisayar için 5 dakika kutularda görmek - ortalama işlemci kullanımı olarak 5 dakika için hesaplanmış. 90 eşik srv01 tarafından sadece bir kez 1:25 bin ihlal ediliyor. Buna karşılık, srv02 1:10, 1:15 ve 1:25 kutular 90 eşiği aşıyor; srv03 1:10, 1:15, 1:20 ve 1:30'da 90 barajını aşarsa.
Uyarı, toplam ihlallere göre tetiklemek üzere yapılandırıldığından, srv02 ve srv03'ün yalnızca ölçütleri karşıladığını görüyoruz. Bu nedenle, birden fazla zaman kutusu arasında %90 eşiğini iki kez aştıkları için srv02 ve srv03 için ayrı uyarılar oluşturulur.  Tetik *leme uyarısı temel alındıysa:* parametre bunun yerine *Sürekli ihlaller* seçeneği için yapılandırıldıysa, 1:10'dan 1:20'ye kadar art arda üç zaman kutusu için eşiği aşması nedeniyle yalnızca srv03 **için** bir uyarı ateşlenir. Ve srv02 için **değil,** 1:10'dan 1:15'e kadar iki ardışık zaman kutusu için eşiği aştı.

## <a name="log-search-alert-rule---firing-and-state"></a>Günlük arama uyarısı kuralı - ateş ve durum

Günlük arama uyarı kuralları yalnızca sorguya oluşturduğunuz mantık üzerinde çalışır. Uyarı sisteminin sistemin durumu, amacı veya sorgutarafından ima edilen temel nedeni başka bir bağlamı yoktur. Bu nedenle, günlük uyarıları devlet-az olarak adlandırılır. Koşullar her çalıştırıldıklarında "DOĞRU" veya "YANLIŞ" olarak değerlendirilir.  Uyarı koşulunun değerlendirilmesi "TRUE" olduğunda, daha önce ateşlenirse ateşlenir.    

Bu davranışı pratik bir örnekle iş başında görelim. Sonuç türü günlük uyarısı için [sağlanan örnekte](#example-of-number-of-records-type-log-alert)gösterildiği gibi yapılandırılan *Contoso-Log-Alert*adlı bir günlük uyarı kuralımız olduğunu varsayalım. Koşul, günlüklerde 500 sonuç kodu aramak için tasarlanmış özel bir uyarı sorgusudur. Günlüklerde bir 500 sonuç kodu daha bulunursa, uyarının durumu doğrudur. 

Aşağıdaki her aralıkta, Azure uyarıları sistemi *Contoso-Log-Alert'ün*durumunu değerlendirir.


| Zaman    | Günlük arama sorgusu yla döndürülen kayıtların num'u | Günlük durumu değerlendirme | Sonuç 
| ------- | ----------| ----------| ------- 
| SAAT 13:05 | 0 kayıt | 0 > 0 değil çok YANLıŞ |  Uyarı ateş etmez. Hiçbir eylem çağrıldı.
| 13:10 | 2 kayıt | 2 > 0 çok DOĞRU  | Uyarı yangınları ve eylem grupları denir. Alert durumu ACTIVE.
| SAAT 13:15 | 5 kayıt | 5 > 0 çok DOĞRU  | Uyarı yangınları ve eylem grupları denir. Alert durumu ACTIVE.
| SAAT 13:20 | 0 kayıt | 0 > 0 değil çok YANLıŞ |  Uyarı ateş etmez. Hiçbir eylem çağrıldı. Uyarı durumu ACTIVE'den ayrıldı.

Önceki servis talebinin örnek olarak kullanılması:

Saat 13:15'te Azure uyarıları, 1:10'da görülen temel sorunların devam edip etmediğini ve kayıtların net yeni hatalar mı yoksa 13:10'da eski hataların tekrarı mı olduğunu belirleyemez. Kullanıcı tarafından sağlanan sorgu, önceki kayıtları dikkate alıyor olabilir veya dikkate almayabilir ve sistem bunu bilmiyor. Azure uyarı sistemi, dikkatli olmak için oluşturulmuştur ve uyarıyı ve ilişkili eylemleri 13:15'te yeniden ateşler. 

Saat 13:20'de 500 sonuç koduyla sıfır kayıt görüldüğünde, Azure uyarıları, 13:10 ve 13:15'te görülen 500 sonuç kodunun nedeninin artık çözüldüğünden emin olamaz. 500 hata sorunları tekrar aynı nedenlerle olur eğer bilmiyor. Bu nedenle *Contoso-Log-Alert,* Azure Uyarı panosunda **Çözülmüş** olarak değişmez ve/veya uyarının çözüldüğünü belirten bildirimler gönderilmez. Yalnızca, analitik sorgusuna katışılmış mantığın tam durumunu veya nedenini anlayan siz, [uyarıyı](alerts-managing-alert-states.md) gerektiği gibi kapalı olarak işaretleyebilirsiniz.

## <a name="pricing-and-billing-of-log-alerts"></a>Günlük Uyarılarının Fiyatlandırması ve Faturalanması

Günlük Uyarıları için geçerli [fiyatlandırma, Azure Monitör Fiyatlandırması](https://azure.microsoft.com/pricing/details/monitor/) sayfasında belirtilir. Azure faturalarında, Günlük Uyarıları şu `microsoft.insights/scheduledqueryrules` şekilde tür olarak temsil edilir:

- Tam uyarı adı ile gösterilen Uygulama Öngörüleri'ndeki Uyarıları kaynak grubu ve uyarı özellikleriyle birlikte günlüğe kaydetme
- Tam uyarı adı ile gösterilen Günlük Analitiğinde Uyarıları, kaynak grubu ve uyarı özellikleriyle birlikte günlüğe kaydetme; [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) kullanılarak oluşturulduğunda

[Eski Log Analytics API'si,](../../azure-monitor/platform/api-alerts.md) Log Analytics Saved Search'ün bir parçası olarak uyarı eylemleri ve zamanlamalara sahiptir ve uygun [Azure Kaynakları](../../azure-resource-manager/management/overview.md)değildir. Bu nedenle, [Yeni API'ye geçmeden](../../azure-monitor/platform/alerts-log-api-switch.md) veya eski Log [Analytics API'sine](../../azure-monitor/platform/api-alerts.md) geçmeden Azure **portalını** kullanarak Log Analytics `microsoft.insights/scheduledqueryrules` için oluşturulan bu tür eski günlük uyarılarının faturalanmasını etkinleştirmek için , Azure'da faturalandırma için gizli sözde uyarı kuralları oluşturulur. Kaynak grubu ve uyarı özellikleriyle `microsoft.insights/scheduledqueryrules` birlikte `<WorkspaceName>|<savedSearchId>|<scheduleId>|<ActionId>` gösterildiği gibi faturalandırma için oluşturulan gizli sözde uyarı kuralları.

> [!NOTE]
> Bu gibi `<, >, %, &, \, ?, /` geçersiz karakterler varsa, bunlar gizli sözde `_` uyarı kuralı adında ve dolayısıyla Azure tasarısında da değiştirilir.

[Eski Log Analytics API](api-alerts.md)kullanarak uyarı kurallarının faturalanması için oluşturulan gizli zamanlamaQueryRules kaynaklarını kaldırmak için, kullanıcı aşağıdakilerden herhangi birini yapabilir:

- Kullanıcı, [Log Analytics çalışma alanındaki uyarı kuralları için API tercihini değiştirebilir](../../azure-monitor/platform/alerts-log-api-switch.md) ve uyarı kurallarını veya izlemeyi hiçbir şekilde Azure Kaynak Yöneticisi uyumlu [tarifeliQueryRules API'ye](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules)taşıyamaz. Bu nedenle faturalandırma için sözde gizli uyarı kuralları yapma gereksinimini ortadan kaldırır.
- Veya kullanıcı API tercihini değiştirmek istemiyorsa, kullanıcının [eski Log Analytics API'sini](api-alerts.md) kullanarak orijinal zamanlamayı ve uyarı eylemini **silmesi** veya [Azure portalında orijinal günlük uyarı kuralını](../../azure-monitor/platform/alerts-log.md#view--manage-log-alerts-in-azure-portal) silmesi gerekir

Ayrıca gizli programiçinQueryRules kaynakları [eski Log Analytics API](api-alerts.md)kullanarak uyarı kurallarının faturalanması için oluşturulan, PUT gibi herhangi bir değişiklik işlemi başarısız olur. Tür sözde kurallar eski [Log Analytics API](api-alerts.md)kullanılarak oluşturulan uyarı kuralları faturalandırma amacıyla olduğundan. `microsoft.insights/scheduledqueryrules` Herhangi bir uyarı kuralı değişikliği [eski Log Analytics API](api-alerts.md) (veya) kullanıcı [yerine scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) kullanmak [için uyarı kuralları için API tercihi geçiş](../../azure-monitor/platform/alerts-log-api-switch.md) yapabilirsiniz kullanılarak yapılmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure'da günlük uyarıları oluşturma](../../azure-monitor/platform/alerts-log.md)hakkında bilgi edinin.
* [Azure'daki günlük uyarılarındaki web hook'ları](alerts-log-webhook.md)anlayın.
* Azure [Uyarıları](../../azure-monitor/platform/alerts-overview.md)hakkında bilgi edinin.
* [Uygulama Öngörüleri](../../azure-monitor/app/analytics.md)hakkında daha fazla bilgi edinin.
* [Log Analytics](../../azure-monitor/log-query/log-query-overview.md)hakkında daha fazla bilgi edinin.
