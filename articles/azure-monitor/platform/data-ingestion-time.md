---
title: Azure Monitör'de veri alma süresini kaydetme | Microsoft Dokümanlar
description: Azure Monitor'da günlük veritoplamada gecikme yi etkileyen farklı faktörleri açıklar.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 07/18/2019
ms.openlocfilehash: 99d5594dd3ebe3750cb0a09ea803065e2aeb5ba2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77666646"
---
# <a name="log-data-ingestion-time-in-azure-monitor"></a>Azure İzleyici'de günlük verileri alım süresi
Azure Monitor, her ay terabaytlarce veri gönderen binlerce müşteriye artan bir hızla hizmet veren yüksek ölçekli bir veri hizmetidir. Günlük verilerinin toplandıktan sonra kullanılabilir hale gelmesi için gereken süre yle ilgili sık sık sorular vardır. Bu makalede, bu gecikme yi etkileyen farklı etkenler açıklanmaktadır.

## <a name="typical-latency"></a>Tipik gecikme
Gecikme süresi, verilerin izlenen sistemde oluşturulduğu zamanı ve Azure Monitor'da çözüme dönüştürülmek üzere kullanıma sunulduğu zamanı ifade eder. Günlük verilerini yutmak için tipik gecikme sayısı 2 ile 5 dakika arasındadır. Belirli bir veri için belirli gecikme sayılma, aşağıda açıklanan çeşitli faktörlere bağlı olarak değişir.


## <a name="factors-affecting-latency"></a>Gecikme süresini etkileyen faktörler
Belirli bir veri kümesinin toplam yutma süresi aşağıdaki üst düzey alanlara ayrılabilir. 

- Aracı lama süresi - Bir olayı keşfetme, toplama ve ardından günlük kaydı olarak Azure İzleme giriş noktasına gönderme zamanı. Çoğu durumda, bu işlem bir aracı tarafından işlenir.
- İşlem hattı süresi - Alım işlem hattının günlük kaydını işleme süresi. Bu, olayın özelliklerini ayrışdırma ve hesaplanmış bilgiler eklemeyi içerir.
- Dizin oluşturma süresi – Azure Monitor büyük veri deposuna bir günlük kaydı nı almak için harcanan süre.

Bu işlemde tanıtılan farklı gecikme ile ilgili ayrıntılar aşağıda açıklanmıştır.

### <a name="agent-collection-latency"></a>Aracı toplama gecikme süresi
Aracılar ve yönetim çözümleri, gecikme yi etkileyebilecek sanal bir makineden veri toplamak için farklı stratejiler kullanır. Bazı özel örnekler şunlardır:

- Windows olayları, syslog olayları ve performans ölçümleri hemen toplanır. Linux performans sayaçları 30 saniyelik aralıklarla yoklanır.
- IIS günlükleri ve özel günlükleri zaman damgası değiştikten sonra toplanır. IIS günlükleri için bu, [IIS'de yapılandırılan rollover zamanlamasından](data-sources-iis-logs.md)etkilenir. 
- Active Directory Replication çözümü değerlendirmesini her beş günde bir gerçekleştirirken, Active Directory Assessment çözümü Aktif Dizin altyapınızın haftalık değerlendirmesini gerçekleştirir. Aracı bu günlükleri yalnızca değerlendirme tamamlandığında toplar.

### <a name="agent-upload-frequency"></a>Aracı karşıya yükleme sıklığı
Log Analytics aracısının hafif olduğundan emin olmak için aracı günlükleri günlüğe kaydeder ve bunları düzenli aralıklarla Azure Monitor'a yükler. Yükleme sıklığı, veri türüne bağlı olarak 30 saniye ile 2 dakika arasında değişir. Çoğu veri 1 dakikadan kısa bir dakikada yüklenir. Ağ koşulları, Azure Monitor alım noktasına ulaşmak için bu verilerin gecikmesini olumsuz etkileyebilir.

### <a name="azure-activity-logs-resource-logs-and-metrics"></a>Azure etkinlik günlükleri, kaynak günlükleri ve ölçümleri
Azure verileri, işleme için Log Analytics alım noktasında kullanılabilir hale gelmek için ek süre ekler:

- Kaynak günlüklerinden gelen veriler, Azure hizmetine bağlı olarak 2-15 dakika sürer. Ortamınızdaki bu gecikme yi incelemek için [aşağıdaki sorguya](#checking-ingestion-time) bakın
- Azure platform ölçümlerinin Log Analytics giriş noktasına gönderilmesi 3 dakika sürer.
- Etkinlik günlüğü verilerinin Log Analytics alım noktasına gönderilmesi yaklaşık 10-15 dakika sürer.

Bir kez yutma noktasında kullanılabilir, veri sorguiçin kullanılabilir olması için ek 2-5 dakika sürer.

### <a name="management-solutions-collection"></a>Yönetim çözümleri koleksiyonu
Bazı çözümler verilerini bir aracıdan toplamaz ve ek gecikme gecikmesi getiren bir toplama yöntemi kullanabilir. Bazı çözümler, neredeyse gerçek zamanlı toplama girişiminde bulunmadan düzenli aralıklarla veri toplar. Belirli örnekler şunlardır:

- Office 365 çözüm anketleri, şu anda gerçek zamanlı gecikme garantisi sağlamayan Office 365 Yönetim Etkinliği API'sini kullanarak etkinlik günlükleri sunar.
- Windows Analytics çözümleri (Örneğin Uyumluluğu Güncelleştir) verileri çözüm tarafından günlük frekansta toplanır.

Toplama sıklığını belirlemek için her çözüm için belgelere bakın.

### <a name="pipeline-process-time"></a>Boru hattı işlem süresi
Günlük kayıtları Azure Monitor ardışık hattına alındıktan sonra [(_TimeReceived](log-standard-properties.md#_timereceived) özelliğinde tanımlandıkları gibi), kiracı yalıtımı sağlamak ve verilerin kaybolmadığından emin olmak için geçici depolama alanına yazılır. Bu işlem genellikle 5-15 saniye ekler. Bazı yönetim çözümleri, verileri toplamak ve veri akışı sağlarken öngörüler elde etmek için daha ağır algoritmalar uygular. Örneğin, Ağ Performansı İzleme, gelen verileri 3 dakikalık aralıklarla toplar ve etkili bir şekilde 3 dakikalık gecikme süreleri ekler. Gecikme süresini ekleyen bir diğer işlem de özel günlükleri işleyen işlemdir. Bazı durumlarda, bu işlem aracı tarafından dosyalardan toplanan günlüklere birkaç dakika gecikme ekleyebilir.

### <a name="new-custom-data-types-provisioning"></a>Yeni özel veri türleri sağlama
Özel bir [günlük](data-sources-custom-logs.md) veya [Veri Toplayıcı API'den](data-collector-api.md)yeni bir özel veri türü oluşturulduğunda, sistem özel bir depolama kapsayıcısı oluşturur. Bu, yalnızca bu veri türünün ilk görünümünde gerçekleşen bir kerelik genel merkezdir.

### <a name="surge-protection"></a>Dalgalanma koruması
Azure Monitor'un en önemli önceliği, müşteri verilerinin kaybolmadığından emin olmaktır, bu nedenle sistem veri dalgalanmaları için yerleşik koruma sağlar. Bu, büyük yük altında bile sistemin çalışmaya devam etmesini sağlamak için arabellekleri içerir. Normal yük altında, bu denetimler bir dakikadan daha az ekler, ancak aşırı koşullarda ve hatalarda verilerin güvenli olmasını sağlarken önemli zaman ekleyebilirsiniz.

### <a name="indexing-time"></a>Dizin oluşturma süresi
Verilere anında erişim sağlamak yerine analitik ve gelişmiş arama özellikleri sağlamak arasında her büyük veri platformu için yerleşik bir denge vardır. Azure Monitor, milyarlarca kayıtta güçlü sorgular çalıştırmanızı ve birkaç saniye içinde sonuç almanızı sağlar. Bu mümkün çünkü altyapı, verileri yutma sırasında önemli ölçüde dönüştürür ve benzersiz kompakt yapılarda saklar. Sistem, bu yapıları oluşturmak için yeterli sayıda kullanılabilir olana kadar verileri arabellekte. Günlük kaydının arama sonuçlarında görünmesi için bu işlem tamamlanmalıdır.

Bu işlem şu anda veri hacmi düşük ama daha yüksek veri hızlarında daha az zaman olduğunda yaklaşık 5 dakika sürer. Bu mantıksız görünüyor, ancak bu işlem yüksek hacimli üretim iş yükleri için gecikme en iyi duruma getirilmesini sağlar.



## <a name="checking-ingestion-time"></a>Yutma süresini kontrol etme
Yutma süresi farklı koşullar altında farklı kaynaklar için değişebilir. Ortamınızın belirli davranışlarını tanımlamak için günlük sorgularını kullanabilirsiniz. Aşağıdaki tablo, bir kayıt oluşturulduğunda ve Azure Monitor'a gönderilirken farklı zamanları nasıl belirleyebileceğinizi belirtir.

| Adım | Özellik veya İşlev | Yorumlar |
|:---|:---|:---|
| Veri kaynağında oluşturulan kayıt | [TimeGenerated](log-standard-properties.md#timegenerated-and-timestamp) <br>Veri kaynağı bu değeri ayarlamıyorsa, _TimeReceived olarak aynı zamana ayarlanır. |
| Azure Monitor bitiş noktası tarafından alınan kayıt | [_TimeReceived](log-standard-properties.md#_timereceived) | |
| Çalışma alanında depolanan ve sorgular için kullanılabilir kayıt | [ingestion_time()](/azure/kusto/query/ingestiontimefunction) | |

### <a name="ingestion-latency-delays"></a>Yutma gecikmesi gecikmeleri
[ingestion_time()](/azure/kusto/query/ingestiontimefunction) işlevinin sonucunu _TimeGenerated_ özelliğiyle karşılaştırarak belirli bir kaydın gecikme süresini ölçebilirsiniz. Bu veriler, yutma gecikmesi nasıl bir şekilde nasıl olduğunu bulmak için çeşitli toplamalarla kullanılabilir. Büyük miktarda veri için öngörüler elde etmek için yutma süresinin bazı yüzdelik dilimini inceleyin. 

Örneğin, aşağıdaki sorgu, önceki 8 saat içinde hangi bilgisayarların en yüksek alım süresine sahip olduğunu gösterir: 

``` Kusto
Heartbeat
| where TimeGenerated > ago(8h) 
| extend E2EIngestionLatency = ingestion_time() - TimeGenerated 
| extend AgentLatency = _TimeReceived - TimeGenerated 
| summarize percentiles(E2EIngestionLatency,50,95), percentiles(AgentLatency,50,95) by Computer 
| top 20 by percentile_E2EIngestionLatency_95 desc
```

Önceki yüzdelik çekler gecikme genel eğilimleri bulmak için iyidir. Gecikme süresinde kısa vadeli bir ani artış`max()`belirlemek için, maksimum kullanarak ( ) daha etkili olabilir.

Belirli bir bilgisayarın kullanım süresini belirli bir süre boyunca ayrıntıya kadar incelemek istiyorsanız, grafikte geçmiş güne ait verileri de görselleştiren aşağıdaki sorguyu kullanın: 


``` Kusto
Heartbeat 
| where TimeGenerated > ago(24h) //and Computer == "ContosoWeb2-Linux"  
| extend E2EIngestionLatencyMin = todouble(datetime_diff("Second",ingestion_time(),TimeGenerated))/60 
| extend AgentLatencyMin = todouble(datetime_diff("Second",_TimeReceived,TimeGenerated))/60 
| summarize percentiles(E2EIngestionLatencyMin,50,95), percentiles(AgentLatencyMin,50,95) by bin(TimeGenerated,30m) 
| render timechart
```
 
Ip adresine dayalı olarak bulundukları ülkeye/bölgeye göre bilgisayar alım süresini göstermek için aşağıdaki sorguyu kullanın: 

``` Kusto
Heartbeat 
| where TimeGenerated > ago(8h) 
| extend E2EIngestionLatency = ingestion_time() - TimeGenerated 
| extend AgentLatency = _TimeReceived - TimeGenerated 
| summarize percentiles(E2EIngestionLatency,50,95),percentiles(AgentLatency,50,95) by RemoteIPCountry 
```
 
Aracıdan kaynaklanan farklı veri türleri farklı aksama gecikme süresine sahip olabilir, bu nedenle önceki sorgular diğer türlerle kullanılabilir. Çeşitli Azure hizmetlerinin kullanım süresini incelemek için aşağıdaki sorguyu kullanın: 

``` Kusto
AzureDiagnostics 
| where TimeGenerated > ago(8h) 
| extend E2EIngestionLatency = ingestion_time() - TimeGenerated 
| extend AgentLatency = _TimeReceived - TimeGenerated 
| summarize percentiles(E2EIngestionLatency,50,95), percentiles(AgentLatency,50,95) by ResourceProvider
```

### <a name="resources-that-stop-responding"></a>Yanıt vermeyi durduran kaynaklar 
Bazı durumlarda, bir kaynak veri göndermeyi durdurabilir. Bir kaynağın veri gönderip göndermediğini anlamak için, standart _TimeGenerated_ alanı tarafından tanımlanabilecek en son kaydına bakın.  

Bir sinyal ataak tarafından dakikada bir kez gönderildiğinden, bir VM kullanılabilirliğini kontrol etmek için _Sinyal_ tablosunu kullanın. Son zamanlarda sinyal bildirmemiş etkin bilgisayarları listelemek için aşağıdaki sorguyu kullanın: 

``` Kusto
Heartbeat  
| where TimeGenerated > ago(1d) //show only VMs that were active in the last day 
| summarize NoHeartbeatPeriod = now() - max(TimeGenerated) by Computer  
| top 20 by NoHeartbeatPeriod desc 
```

## <a name="next-steps"></a>Sonraki adımlar
* Azure Monitor için [Hizmet Düzeyi Sözleşmesi'ni (SLA)](https://azure.microsoft.com/support/legal/sla/log-analytics/v1_1/) okuyun.

