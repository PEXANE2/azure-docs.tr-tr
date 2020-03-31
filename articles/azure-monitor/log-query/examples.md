---
title: Azure Monitor günlük sorgu örnekleri | Microsoft Dokümanlar
description: Kusto sorgu dilini kullanarak Azure Monitor'daki günlük sorgularına örnekler.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/16/2020
ms.openlocfilehash: 18cd74ac9298b7dd058de2b224f677ec0d8f2d64
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79480292"
---
# <a name="azure-monitor-log-query-examples"></a>Azure Monitor günlük sorgu örnekleri
Bu makalede, Azure Monitor'dan farklı türde günlük verileri almak için [Kusto sorgu dilini](/azure/kusto/query/) kullanan çeşitli [sorgu](log-query-overview.md) örnekleri yer almaktadır. Verileri birleştirmek ve çözümlemek için farklı yöntemler kullanılır, böylece bu örnekleri kendi gereksinimleriniz için kullanabileceğiniz farklı stratejileri belirlemek için kullanabilirsiniz.  

Bu örneklerde kullanılan farklı anahtar kelimelerle ilgili ayrıntılar için [Kusto dil referansına](https://docs.microsoft.com/azure/kusto/query/) bakın. Azure [Monitor'da](get-started-queries.md) yeniyseniz sorgu oluşturma konusunda bir dersten geçin.

## <a name="events"></a>Olaylar

### <a name="search-application-level-events-described-as-cryptographic"></a>"Şifreleme" olarak tanımlanan uygulama düzeyindeki olayları arama
Bu örnek, **EventLog'un** _Uygulama_ olduğu ve **RenderedDescription'ın** _şifreleme_içerdiği kayıtlar için **Olaylar** tablosunu arar. Son 24 saate ait kayıtları içeriyor.

```Kusto
Event
| where EventLog == "Application" 
| where TimeGenerated > ago(24h) 
| where RenderedDescription contains "cryptographic"
```

### <a name="search-events-related-to-unmarshaling"></a>Mareşallik ile ilgili arama etkinlikleri
Arama tabloları **Olay** ve **GüvenlikOlayları** _unmarshaling_söz kayıtları için .

```Kusto
search in (Event, SecurityEvent) "unmarshaling"
```

## <a name="heartbeat"></a>Sinyal

### <a name="chart-a-week-over-week-view-of-the-number-of-computers-sending-data"></a>Veri gönderen bilgisayar sayısının haftadan uzun bir görünümünü grafik

Aşağıdaki örnek, her hafta sinyal gönderen farklı bilgisayarların sayısını listeler.

```Kusto
Heartbeat
| where TimeGenerated >= startofweek(ago(21d))
| summarize dcount(Computer) by endofweek(TimeGenerated) | render barchart kind=default
```

### <a name="find-stale-computers"></a>Eski bilgisayarları bulma

Aşağıdaki örnek, son gün etkin olan ancak son bir saat içinde sinyal göndermeyen bilgisayarları bulur.

```Kusto
Heartbeat
| where TimeGenerated > ago(1d)
| summarize LastHeartbeat = max(TimeGenerated) by Computer
| where isnotempty(Computer)
| where LastHeartbeat < ago(1h)
```

### <a name="get-the-latest-heartbeat-record-per-computer-ip"></a>Bilgisayar ipi başına en son sinyal kaydını alın

Bu örnek, her bilgisayar IP'si için son sinyal kaydını döndürür.
```Kusto
Heartbeat
| summarize arg_max(TimeGenerated, *) by ComputerIP
```

### <a name="match-protected-status-records-with-heartbeat-records"></a>Korumalı durum kayıtlarını sinyal kayıtlarıyla eşleştirme

Bu örnek, hem Bilgisayar hem de zaman üzerinde eşleşen ilgili koruma durum kayıtlarını ve sinyal kayıtlarını bulur.
Zaman alanının en yakın dakikaya yuvarlanmaya dikkat edin. Bunu yapmak için runtime bin `round_time=bin(TimeGenerated, 1m)`hesaplamasını kullandık: .

```Kusto
let protection_data = ProtectionStatus
    | project Computer, DetectionId, round_time=bin(TimeGenerated, 1m);
let heartbeat_data = Heartbeat
    | project Computer, Category, round_time=bin(TimeGenerated, 1m);
protection_data | join (heartbeat_data) on Computer, round_time
```

### <a name="server-availability-rate"></a>Sunucu kullanılabilirlik oranı

Sinyal kayıtlarını temel alan sunucu kullanılabilirlik oranını hesaplayın. Kullanılabilirlik "saatte en az 1 kalp atışı" olarak tanımlanır.
Bu nedenle, bir sunucu 100 saatin 98'inde kullanılabilirse, kullanılabilirlik oranı %98'dir.

```Kusto
let start_time=startofday(datetime("2018-03-01"));
let end_time=now();
Heartbeat
| where TimeGenerated > start_time and TimeGenerated < end_time
| summarize heartbeat_per_hour=count() by bin_at(TimeGenerated, 1h, start_time), Computer
| extend available_per_hour=iff(heartbeat_per_hour>0, true, false)
| summarize total_available_hours=countif(available_per_hour==true) by Computer 
| extend total_number_of_buckets=round((end_time-start_time)/1h)+1
| extend availability_rate=total_available_hours*100/total_number_of_buckets
```


## <a name="multiple-data-types"></a>Birden çok veri türü

### <a name="chart-the-record-count-per-table"></a>Tablo başına kayıt sayısını grafik
Aşağıdaki örnek, son beş saatteki tüm tabloların tüm kayıtlarını toplar ve her tabloda kaç kayıt olduğunu sayar. Sonuçlar bir zaman çizelgesinde gösterilir.

```Kusto
union withsource=sourceTable *
| where TimeGenerated > ago(5h) 
| summarize count() by bin(TimeGenerated,10m), sourceTable
| render timechart
```

### <a name="count-all-logs-collected-over-the-last-hour-by-type"></a>Son saat içinde toplanan tüm günlükleri türüne göre sayma
Aşağıdaki örnek, son bir saat içinde bildirilen her şeyi arar ve her tablonun kayıtlarını **Türüne**göre sayar. Sonuçlar çubuk grafikte görüntülenir.

```Kusto
search *
| where TimeGenerated > ago(1h) 
| summarize CountOfRecords = count() by Type
| render barchart
```

## <a name="azurediagnostics"></a>AzureDiagnostics

### <a name="count-azure-diagnostics-records-per-category"></a>Azure tanılama kayıtlarını kategori başına sayma
Bu örnek, her benzersiz kategori için tüm Azure tanılama kayıtlarını sayar.

```Kusto
AzureDiagnostics 
| where TimeGenerated > ago(1d)
| summarize count() by Category
```

### <a name="get-a-random-record-for-each-unique-category"></a>Her benzersiz kategori için rastgele bir kayıt alın
Bu örnek, her benzersiz kategori için tek bir rasgele Azure tanılama kaydı alır.

```Kusto
AzureDiagnostics
| where TimeGenerated > ago(1d) 
| summarize any(*) by Category
```

### <a name="get-the-latest-record-per-category"></a>Kategori başına en son kaydı alın
Bu örnek, her benzersiz kategorideki en son Azure tanılama kaydını alır.

```Kusto
AzureDiagnostics
| where TimeGenerated > ago(1d) 
| summarize arg_max(TimeGenerated, *) by Category
```

## <a name="network-monitoring"></a>Ağ izleme

### <a name="computers-with-unhealthy-latency"></a>Sağlıksız gecikme stoyonu olan bilgisayarlar
Bu örnek, sağlıksız gecikme sayılmı olan farklı bilgisayarların bir listesini oluşturur.

```Kusto
NetworkMonitoring 
| where LatencyHealthState <> "Healthy" 
| where Computer != "" 
| distinct Computer
```

## <a name="performance"></a>Performans

### <a name="join-computer-perf-records-to-correlate-memory-and-cpu"></a>Bellek ve CPU'u ilişkilendirmek için bilgisayar perf kayıtlarını birleştirme
Bu örnek, belirli bir bilgisayarın **perf** kayıtlarını ilişkilendirer ve iki zaman çizelgeleri, ortalama CPU ve maksimum bellek oluşturur.

```Kusto
let StartTime = now()-5d;
let EndTime = now()-4d;
Perf
| where CounterName == "% Processor Time"  
| where TimeGenerated > StartTime and TimeGenerated < EndTime
| project TimeGenerated, Computer, cpu=CounterValue 
| join kind= inner (
   Perf
    | where CounterName == "% Used Memory"  
    | where TimeGenerated > StartTime and TimeGenerated < EndTime
    | project TimeGenerated , Computer, mem=CounterValue 
) on TimeGenerated, Computer
| summarize avgCpu=avg(cpu), maxMem=max(mem) by TimeGenerated bin=30m  
| render timechart
```

### <a name="perf-cpu-utilization-graph-per-computer"></a>Perf CPU Bilgisayar Başına Kullanım grafiği
Bu örnek, _Contoso_ile başlayan bilgisayarların CPU kullanımını hesaplar ve listeler.

```Kusto
Perf
| where TimeGenerated > ago(4h)
| where Computer startswith "Contoso" 
| where CounterName == @"% Processor Time"
| summarize avg(CounterValue) by Computer, bin(TimeGenerated, 15m) 
| render timechart
```

## <a name="protection-status"></a>Koruma durumu

### <a name="computers-with-non-reporting-protection-status-duration"></a>Raporlama dışı koruma durumu süresine sahip bilgisayarlar
Bu örnek, _Raporlama Değil_ koruma durumu ve bu durumda oldukları süreye sahip bilgisayarları listeler.

```Kusto
ProtectionStatus
| where ProtectionStatus == "Not Reporting"
| summarize count(), startNotReporting = min(TimeGenerated), endNotReporting = max(TimeGenerated) by Computer, ProtectionStatusDetails
| join ProtectionStatus on Computer
| summarize lastReporting = max(TimeGenerated), startNotReporting = any(startNotReporting), endNotReporting = any(endNotReporting) by Computer
| extend durationNotReporting = endNotReporting - startNotReporting
```

### <a name="match-protected-status-records-with-heartbeat-records"></a>Korumalı durum kayıtlarını sinyal kayıtlarıyla eşleştirme
Bu örnek, ilgili koruma durum kayıtlarını ve hem Bilgisayar hem de zaman üzerinde eşleşen sinyal kayıtlarını bulur.
Zaman **alanı, depo kutusu**kullanılarak en yakın dakikaya yuvarlanır.

```Kusto
let protection_data = ProtectionStatus
    | project Computer, DetectionId, round_time=bin(TimeGenerated, 1m);
let heartbeat_data = Heartbeat
    | project Computer, Category, round_time=bin(TimeGenerated, 1m);
protection_data | join (heartbeat_data) on Computer, round_time
```


## <a name="security-records"></a>Güvenlik kayıtları

### <a name="count-security-events-by-activity-id"></a>Etkinlik kimliğine göre güvenlik olaylarını sayma


Bu örnek, **Etkinlik** sütununun sabit yapısına\>-\<\>dayanır: \<Kimlik Adı.
**Etkinlik** değerini iki yeni sütuna ayrıştırır ve her **etkinlikkimliğinin**oluşumunu sayar.

```Kusto
SecurityEvent
| where TimeGenerated > ago(30m) 
| project Activity 
| parse Activity with activityID " - " activityDesc
| summarize count() by activityID
```

### <a name="count-security-events-related-to-permissions"></a>İzinlerle ilgili güvenlik olaylarını sayma
Bu örnek, **Etkinlik** sütununun tüm _izinleri_içerdiği **güvenlikOlay** kayıtlarının sayısını gösterir. Sorgu, son 30 dakika içinde oluşturulan kayıtlar için geçerlidir.

```Kusto
SecurityEvent
| where TimeGenerated > ago(30m)
| summarize EventCount = countif(Activity has "Permissions")
```

### <a name="find-accounts-that-failed-to-log-in-from-computers-with-a-security-detection"></a>Güvenlik algılamasıyla bilgisayarlardan oturum açamayan hesapları bulma
Bu örnek, üzerinde güvenlik algılaması tanımladığımız bilgisayarlardan oturum açamayan hesapları bulur ve sayar.

```Kusto
let detections = toscalar(SecurityDetection
| summarize makeset(Computer));
SecurityEvent
| where Computer in (detections) and EventID == 4624
| summarize count() by Account
```

### <a name="is-my-security-data-available"></a>Güvenlik verilerim kullanılabilir mi?
Başlangıç veri arama genellikle veri kullanılabilirlik kontrolü ile başlar. Bu örnek, son 30 dakikadaki **SecurityEvent** kayıtlarının sayısını gösterir.

```Kusto
SecurityEvent 
| where TimeGenerated  > ago(30m) 
| count
```

### <a name="parse-activity-name-and-id"></a>Ayrıştırma etkinlik adı ve kimliği
Aşağıdaki iki örnek **Etkinlik** sütununun sabit yapısına\>-\<\>dayanır: \<Kimlik Adı . İlk örnek, değerleri iki yeni sütuna atamak için **ayrıştırma** işleci kullanır: **activityID** ve **activityDesc**.

```Kusto
SecurityEvent
| take 100
| project Activity 
| parse Activity with activityID " - " activityDesc
```

Bu örnek, ayrı değerler dizisi oluşturmak için **bölme** işleci kullanır
```Kusto
SecurityEvent
| take 100
| project Activity 
| extend activityArr=split(Activity, " - ") 
| project Activity , activityArr, activityId=activityArr[0]
```

### <a name="explicit-credentials-processes"></a>Açık kimlik bilgileri işlemleri
Aşağıdaki örnek, son hafta içinde açık kimlik bilgilerini kullanan işlemlerin bir pasta grafiğini gösterir

```Kusto
SecurityEvent
| where TimeGenerated > ago(7d)
// filter by id 4648 ("A logon was attempted using explicit credentials")
| where EventID == 4648
| summarize count() by Process
| render piechart 
```

### <a name="top-running-processes"></a>En çok çalışan süreçler

Aşağıdaki örnek, son üç gün içinde en yaygın beş işlem için bir zaman çizgisi gösterir.

```Kusto
// Find all processes that started in the last three days. ID 4688: A new process has been created.
let RunProcesses = 
    SecurityEvent
    | where TimeGenerated > ago(3d)
    | where EventID == "4688";
// Find the 5 processes that were run the most
let Top5Processes =
RunProcesses
| summarize count() by Process
| top 5 by count_;
// Create a time chart of these 5 processes - hour by hour
RunProcesses 
| where Process in (Top5Processes) 
| summarize count() by bin (TimeGenerated, 1h), Process
| render timechart
```


### <a name="find-repeating-failed-login-attempts-by-the-same-account-from-different-ips"></a>Farklı IP'lerden aynı hesaptarafından yinelenen başarısız giriş denemelerini bulma

Aşağıdaki örnek, son altı saat içinde beşten fazla farklı IP'den aynı hesabın başarısız giriş denemelerini bulur.

```Kusto
SecurityEvent 
| where AccountType == "User" and EventID == 4625 and TimeGenerated > ago(6h) 
| summarize IPCount = dcount(IpAddress), makeset(IpAddress)  by Account
| where IPCount > 5
| sort by IPCount desc
```

### <a name="find-user-accounts-that-failed-to-log-in"></a>Oturum açmada başarısız olan kullanıcı hesaplarını bulma 
Aşağıdaki örnek, son gün içinde beş defadan fazla oturum açamayan ve en son ne zaman oturum açmaya çalıştıkları kullanıcı hesaplarını tanımlar.

```Kusto
let timeframe = 1d;
SecurityEvent
| where TimeGenerated > ago(1d)
| where AccountType == 'User' and EventID == 4625 // 4625 - failed log in
| summarize failed_login_attempts=count(), latest_failed_login=arg_max(TimeGenerated, Account) by Account 
| where failed_login_attempts > 5
| project-away Account1
```

**Join** **ve** let deyimlerini kullanarak aynı şüpheli hesapların daha sonra başarılı bir şekilde oturum açıp açabildiğini kontrol edebiliriz.

```Kusto
let timeframe = 1d;
let suspicious_users = 
    SecurityEvent
    | where TimeGenerated > ago(timeframe)
    | where AccountType == 'User' and EventID == 4625 // 4625 - failed login
    | summarize failed_login_attempts=count(), latest_failed_login=arg_max(TimeGenerated, Account) by Account 
    | where failed_login_attempts > 5
    | project-away Account1;
let suspicious_users_that_later_logged_in = 
    suspicious_users 
    | join kind=innerunique (
        SecurityEvent
        | where TimeGenerated > ago(timeframe)
        | where AccountType == 'User' and EventID == 4624 // 4624 - successful login,
        | summarize latest_successful_login=arg_max(TimeGenerated, Account) by Account
    ) on Account
    | extend was_login_after_failures = iif(latest_successful_login>latest_failed_login, 1, 0)
    | where was_login_after_failures == 1
;
suspicious_users_that_later_logged_in
```

## <a name="usage"></a>Kullanım

Veri `Usage` türü, yutulan veri hacmini çözüme veya veri türüne göre izlemek için kullanılabilir. Bilgisayar [veya](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#data-volume-by-computer) [Azure aboneliği, kaynak grubu veya kaynağa](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#data-volume-by-azure-resource-resource-group-or-subscription)göre alınan veri hacimlerini incelemek için başka teknikler de vardır.

#### <a name="data-volume-by-solution"></a>Çözüme göre veri hacmi

Son bir ay içinde (son kısmi gün hariç) çözüme göre faturalandırılabilir veri hacmini görüntülemek için kullanılan sorgu:

```kusto
Usage 
| where TimeGenerated > ago(32d)
| where StartTime >= startofday(ago(31d)) and EndTime < startofday(now())
| where IsBillable == true
| summarize BillableDataGB = sum(Quantity) / 1000. by bin(StartTime, 1d), Solution | render barchart
```

Yan tümcenin, `where IsBillable = true` yutma ücreti olmayan belirli çözümlerden veri türlerini filtrelediğini unutmayın.  Ayrıca, azure `TimeGenerated` portalındaki sorgu deneyiminin varsayılan 24 saatin ötesine bakmasını sağlamak için de yan tümcedir. Kullanım veri türünü `StartTime` kullanırken `EndTime` ve sonuçların sunulduğu zaman kovalarını temsil edin. 

#### <a name="data-volume-by-type"></a>Türüne göre veri hacmi

Veri türüne göre veri eğilimlerini görmek için daha fazla detaya inebilirsiniz:

```kusto
Usage 
| where TimeGenerated > ago(32d)
| where StartTime >= startofday(ago(31d)) and EndTime < startofday(now())
| where IsBillable == true
| summarize BillableDataGB = sum(Quantity) / 1000. by bin(StartTime, 1d), DataType | render barchart
```

Ya da çözüme göre bir tablo görmek ve son bir ay için yazın,

```kusto
Usage 
| where TimeGenerated > ago(32d)
| where StartTime >= startofday(ago(31d)) and EndTime < startofday(now())
| where IsBillable == true
| summarize BillableDataGB = sum(Quantity) / 1000. by Solution, DataType
| sort by Solution asc, DataType asc
```

> [!NOTE]
> Kullanım veri türünün bazı alanları, hala şemadayken, küçümsenmiş ve değerleri artık doldurulmayacak. Bu **Bilgisayar** yanı sıra yutma ile ilgili alanlar **(TotalBatches**, **BatchesWithinSla**, **BatchesOutsideSla**, **BatchesCapped** ve **AverageProcessingTimeMs**.

## <a name="updates"></a>Güncelleştirmeler

### <a name="computers-still-missing-updates"></a>Bilgisayarlar Hala Eksik Güncelleştirmeler
Bu örnek, birkaç gün önce bir veya daha fazla kritik güncelleştirme eksik olan ve hala eksik güncelleştirmeleri olan bilgisayarların listesini gösterir.

```Kusto
let ComputersMissingUpdates3DaysAgo = Update
| where TimeGenerated between (ago(30d)..ago(1h))
| where Classification !has "Critical" and UpdateState =~ "Needed"
| summarize makeset(Computer);
Update
| where TimeGenerated > ago(1d)
| where Classification has "Critical" and UpdateState =~ "Needed"
| where Computer in (ComputersMissingUpdates3DaysAgo)
| summarize UniqueUpdatesCount = dcount(Product) by Computer, OSType
```


## <a name="next-steps"></a>Sonraki adımlar

- Dil hakkındaki ayrıntılar için [Kusto dil referansına](/azure/kusto/query) bakın.
- [Azure Monitor'da günlük sorguları yazma konusunda](get-started-queries.md)bir derste yürüyün.