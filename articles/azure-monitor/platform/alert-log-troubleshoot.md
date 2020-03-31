---
title: Azure Monitör'de günlük uyarıları | Microsoft Dokümanlar
description: Azure'da günlük uyarı kuralları için sık karşılaşılan sorunlar, hatalar ve çözümler.
author: yanivlavi
ms.author: yalavi
ms.topic: conceptual
ms.subservice: alerts
ms.date: 10/29/2018
ms.openlocfilehash: acb9784b745fa90fc9cd264162930020e6d64751
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79249041"
---
# <a name="troubleshoot-log-alerts-in-azure-monitor"></a>Azure Monitör'de günlük uyarılarını giderme  

Bu makalede, Azure Monitor'da günlük uyarıları ayarlarken ortaya çıkan sık karşılaşılan sorunları nasıl çözeceğiniz gösterilmektedir. Ayrıca, günlük uyarılarının işlevselliği veya yapılandırmasıyla ilgili sık karşılaşılan sorunlara çözümler de sağlar.

Günlük *uyarıları* terimi, [Azure Log Analytics çalışma alanında](../learn/tutorial-viewdata.md) veya Azure Uygulama [Öngörüleri'ndeki](../../azure-monitor/app/analytics.md)günlük sorgusuna dayalı olarak çalışan kuralları açıklar. [Azure Monitor'da Günlük uyarılarındaki](../platform/alerts-unified-log.md)işlevsellik, terminoloji ve türleri hakkında daha fazla bilgi edinin.

> [!NOTE]
> Bu makalede, Azure portalının tetiklenen bir uyarı kuralı gösterdiği ve bildirimin ilişkili bir eylem grubu tarafından gerçekleştirilmediği durumlar dikkate alınmaz. Bu gibi durumlarda, [Azure portalındaki eylem gruplarını oluştur ve yönet ayrıntılarına](../platform/action-groups.md)bakın.

## <a name="log-alert-didnt-fire"></a>Günlük uyarısı ateş etmedi

Azure Monitor'da yapılandırılmış günlük uyarı [kuralının](../platform/alerts-log.md) durumunun beklendiği gibi gösterilmemesinin bazı yaygın nedenleri aşağıda veda [edilmiştir. *fired* ](../platform/alerts-managing-alert-states.md)

### <a name="data-ingestion-time-for-logs"></a>Günlükler için veri alma süresi

Günlük uyarısı, sorgunuzu Günlük [Analizi](../learn/tutorial-viewdata.md) veya [Uygulama Öngörüleri'ni](../../azure-monitor/app/analytics.md)temel alınca düzenli aralıklarla çalıştırAr. Azure Monitor, dünyanın çeşitli kaynaklarından binlerce müşteriden gelen birçok terabaytlık veriyi işlediği için, hizmet değişen zaman gecikmelerine açıktır. Daha fazla bilgi için Azure [Monitor günlüklerinde Veri alma süresine](../platform/data-ingestion-time.md)bakın.

Gecikmeleri azaltmak için sistem, gerekli verilerin henüz yutulmadığını tespit ederse, uyarı sorgusunu birden çok kez bekler ve yeniden dener. Sistem katlanarak artan bir bekleme süresi kümesine sahiptir. Günlük uyarısı yalnızca veriler kullanılabilir olduktan sonra tetiklenir, bu nedenle gecikme günlük verilerinin yavaş alınmasından kaynaklanıyor olabilir.

### <a name="incorrect-time-period-configured"></a>Yanlış zaman dilimi yapılandırıldı

Günlük uyarıları için [terminoloji](../platform/alerts-unified-log.md#log-search-alert-rule---definition-and-types)makalesinde açıklandığı gibi, yapılandırmada belirtilen süre sorgu için zaman aralığını belirtir. Sorgu yalnızca bu aralık ta oluşturulan kayıtları döndürür.

Zaman dilimi kötüye kullanımı önlemek için bir günlük sorgusu için getirilen verileri kısıtlar ve bir günlük sorgusunda kullanılan herhangi bir zaman komutunu **(önceki**gibi) atlatıyor. Örneğin, zaman dilimi 60 dakika olarak ayarlanırsa ve sorgu 13:15'te çalıştırılırsa, günlük sorgusu için yalnızca 12:15 ile 13:15 arasında oluşturulan kayıtlar kullanılır. Günlük sorgusu **önce (1d)** gibi bir zaman komutu kullanıyorsa, zaman dilimi bu arayla ayarlandığı için sorgu yine de yalnızca 12:15 ile 13:15 arasında veri kullanır.

Yapılandırmadaki sürenin sorgunuzla eşleşin. Daha önce gösterilen örnekte, günlük sorgusu önce **(1d)** yeşil işaretçiyle kullanıyorsa, süre 24 saat veya 1.440 dakika (kırmızı ile gösterilir) olarak ayarlanmalıdır. Bu ayar, sorgunun beklendiği gibi çalıştırMasını sağlar.

![Süre](media/alert-log-troubleshoot/LogAlertTimePeriod.png)

### <a name="suppress-alerts-option-is-set"></a>Uyarıları Bastır seçeneği ayarlandı

[Azure portalında günlük uyarı kuralı oluşturma](../platform/alerts-log.md#managing-log-alerts-from-the-azure-portal)yla ilgili makalenin 8. **Suppress Alerts** Sonuç olarak, bir uyarının ateş etmediğini düşünebilirsiniz. Aslında, ateş etti ama bastırıldı.  

![Uyarıları bastırma](media/alert-log-troubleshoot/LogAlertSuppress.png)

### <a name="metric-measurement-alert-rule-is-incorrect"></a>Metrik ölçüm uyarı kuralı yanlış

*Metrik ölçüm günlüğü uyarıları,* özel özelliklere ve kısıtlı uyarı sorgusu sözdizimine sahip günlük uyarılarının bir alt türüdür. Metrik ölçüm günlüğü uyarısı için bir kural sorgu çıktısının bir metrik zaman serisi olmasını gerektirir. Diğer bir tarihte, çıktı, ilgili toplu değerlerle birlikte farklı, eşit boyutlandırılmış zaman dönemlerine sahip bir tablodur.

**Tabloda AggregatedValue**ile birlikte ek değişkenler olmasını seçebilirsiniz. Bu değişkenler tabloyu sıralamak için kullanılabilir.

Örneğin, bir metrik ölçüm günlüğü uyarısı için bir kural olarak yapılandırıldı varsayalım:

- Sorgu`search *| summarize AggregatedValue = count() by $table, bin(timestamp, 1h)`  
- 6 saatlik süre
- 50 eşik
- Art arda üç ihlalin uyarı mantığı
- **$table** **olarak** seçilen Toplama

Çünkü komut **özetiçerir ... tarafından** ve iki değişken **(zaman damgası** ve **$table)** sağlar, sistem **Üzerine Aggregate**için **$table** seçer. Sistem, aşağıdaki ekran görüntüsünde gösterildiği gibi sonuç tablosunu **$table** alanına göre sıralar. Daha sonra, art arda üç veya daha fazla ihlal olup olmadığını görmek için her tablo türü **(availabilityResults**gibi) için birden çok **AggregatedValue** örneğine bakar.

![Birden çok değerle metrik ölçüm sorgusu yürütme](media/alert-log-troubleshoot/LogMMQuery.png)

**Aggregate Upon** **$table**tanımlandığı için veriler **$table** sütununda (kırmızı ile gösterilir) sıralanır. Daha sonra grup ve **Toplam Upon** alan türleri arayın.

Örneğin, **$table**için, **kullanılabilirlik** için değerlerSonuçlar tek bir çizim/varlık olarak kabul edilir (turuncu renkte gösterilir). Bu değer çiziminde/varlığında, uyarı hizmeti art arda üç ihlali denetler (yeşil olarak gösterilir). İhlaller, tablo değeri **kullanılabilirliği Sonuçları**için bir uyarı tetikler.

Benzer şekilde, **$table**başka bir değeri için art arda üç ihlal gerçekleşirse, aynı şey için başka bir uyarı bildirimi tetiklenir. Uyarı hizmeti, değerleri otomatik olarak bir çizimdeki/varlıkta (turuncu renkle gösterilir) zamana göre sıralar.

Şimdi metrik ölçüm günlüğü uyarısı için kural değiştirildi ve `search *| summarize AggregatedValue = count() by bin(timestamp, 1h)`sorgu olduğunu varsayalım . Yapılandırmanın geri kalanı, art arda üç ihlal için uyarı mantığı da dahil olmak üzere öncekiyle aynı kaldı. Bu durumda **Aggregate Upon** seçeneği varsayılan olarak **zaman damgasI** olur. Özetlemek için sorguda yalnızca bir değer sağlanır **... tarafından** (yani, **zaman damgası).** Önceki örnekte olduğu gibi, yürütme sonundaki çıktı aşağıdaki gibi gösterilecek.

   ![Tekil değeri olan metrik ölçüm sorgusu yürütmesi](media/alert-log-troubleshoot/LogMMtimestamp.png)

**Aggregate Upon** zaman **damgasında**tanımlandığından, veriler **zaman damgası** sütununda sıralanır (kırmızı ile gösterilir). Sonra zaman **damgası**ile grup . Örneğin, değerler `2018-10-17T06:00:00Z` tek bir çizim/varlık olarak kabul edilir (turuncu ile gösterilir). Bu değer çiziminde/varlığında, uyarı hizmeti ardışık ihlaller bulamaz (çünkü her **zaman damgası** değeri yalnızca bir girişi vardır). Yani uyarı hiç tetiklenmez. Böyle bir durumda, kullanıcı aşağıdakilerden birini

- **Aggregate Upon** alanını kullanarak doğru bir şekilde sıralamak için bir kukla değişken veya varolan bir değişken **($table**gibi) ekleyin.
- Bunun yerine **toplam ihlale** dayalı uyarı mantığını kullanmak için uyarı kuralını yeniden yapılandırın.

## <a name="log-alert-fired-unnecessarily"></a>Günlük uyarısı gereksiz yere ateşlendi

[Azure Görünümü'nde](../platform/alerts-log.md) yapılandırılmış günlük uyarı kuralı, [Azure Uyarıları'nda](../platform/alerts-managing-alert-states.md)görüntülediğinizde beklenmedik bir şekilde tetiklenebilir. Aşağıdaki bölümlerde bazı yaygın nedenler açıklanmaktadır.

### <a name="alert-triggered-by-partial-data"></a>Kısmi veriler tarafından tetiklenen uyarı

Log Analytics ve Application Insights yutma gecikmelerine ve işleme tabi tutulur. Günlük uyarı sorgusu çalıştırdığınızda, hiçbir veri bulunmadığını veya yalnızca bazı verilerin kullanılabildiğini görebilirsiniz. Daha fazla bilgi için Azure [Monitor'da veri alma süresini](../platform/data-ingestion-time.md)günlüğüne bakın.

Uyarı kuralını nasıl yapılandırdığınıza bağlı olarak, uyarı yürütme sırasında günlüklerde veri veya kısmi veri yoksa yanlış ateşleme gerçekleşebilir. Bu gibi durumlarda, uyarı sorgusunu veya yapılandırmayı değiştirmenizi tavsiye ederiz.

Örneğin, bir analitiği sorgusundan elde edilen sonuç sayısı 5'ten az olduğunda tetiklenecek günlük uyarı kuralını yapılandırAcak şekilde yapılandırıyorsanız, veri (sıfır kayıt) veya kısmi sonuç (bir kayıt) olmadığında uyarı tetiklenir. Ancak veri alma gecikmesi sonrasında, tam veri ile aynı sorgu 10 kayıt bir sonucu sağlayabilir.

### <a name="alert-query-output-is-misunderstood"></a>Uyarı sorgusu çıktısı yanlış anlaşıldı

Bir analitik sorgusunda günlük uyarıları için mantığı sağlarsınız. Analitik sorgu çeşitli büyük veri ve matematiksel işlevleri kullanabilirsiniz. Uyarı hizmeti, sorgunuzu belirli bir süre için verilerle belirtilen aralıklarla çalıştırZ. Uyarı hizmeti, uyarı türüne bağlı olarak sorguda ince değişiklikler yapar. **Bu değişikliği, Yapılandırma sinyali mantığı** ekranında **yürütülecek Sorgu** bölümünde görüntüleyebilirsiniz:

![Yürütülecek sorgu](media/alert-log-troubleshoot/LogAlertPreview.png)

**Yürütülecek Sorgu** kutusu, günlük uyarı hizmetinin çalıştırılan durumudur. Uyarıyı oluşturmadan önce uyarı sorgusu çıktısının ne olabileceğini anlamak istiyorsanız, belirtilen sorguyu ve zaman lamasını [Analytics portalı](../log-query/portals.md) veya [Analytics API](https://docs.microsoft.com/rest/api/loganalytics/)üzerinden çalıştırabilirsiniz.

## <a name="log-alert-was-disabled"></a>Günlük uyarısı devre dışı bırakıldı

Aşağıdaki bölümlerde Azure Monitor'un [günlük uyarı kuralını](../platform/alerts-log.md)devre dışı kaçması için bazı nedenler listelenir.

### <a name="resource-where-the-alert-was-created-no-longer-exists"></a>Uyarının oluşturulduğu kaynak artık yok

Azure Monitor'da oluşturulan günlük uyarı kuralları, Azure Log Analytics çalışma alanı, Azure Uygulama Öngörüleri uygulaması ve Azure kaynağı gibi belirli bir kaynağı hedeflemektedir. Günlük uyarı hizmeti daha sonra belirtilen hedef için kuralda sağlanan bir analiz sorgusu çalıştırır. Ancak kural oluşturulduktan sonra, kullanıcılar genellikle Azure'dan silmeye veya günlük uyarısı kuralının hedefi olan Azure'da hareket etmeye devam edin. Uyarı kuralının hedefi artık geçerli olmadığından, kuralın yürütülmesi başarısız olur.

Bu gibi durumlarda Azure Monitor günlük uyarısını devre dışı kılabilir ve kural büyük çetin bir süre boyunca (bir hafta gibi) sürekli olarak çalıştırılamadığınızda gereksiz yere faturalandırılmamanızı sağlar. Azure Monitor'un günlük uyarısını Azure Etkinlik [Günlüğü](../../azure-resource-manager/management/view-activity-logs.md)üzerinden tam olarak ne zaman devre dışı bırakıldığını öğrenebilirsiniz. Azure Etkinlik Günlüğü'nde, Azure Monitörü günlük uyarı kuralını devre dışı ladığında bir olay eklenir.

Azure Etkinlik Günlüğü'ndeki aşağıdaki örnek olay, sürekli bir hata nedeniyle devre dışı bırakılmış bir uyarı kuralı içindir.

```json
{
    "caller": "Microsoft.Insights/ScheduledQueryRules",
    "channels": "Operation",
    "claims": {
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn": "Microsoft.Insights/ScheduledQueryRules"
    },
    "correlationId": "abcdefg-4d12-1234-4256-21233554aff",
    "description": "Alert: test-bad-alerts is disabled by the System due to : Alert has been failing consistently with the same exception for the past week",
    "eventDataId": "f123e07-bf45-1234-4565-123a123455b",
    "eventName": {
        "value": "",
        "localizedValue": ""
    },
    "category": {
        "value": "Administrative",
        "localizedValue": "Administrative"
    },
    "eventTimestamp": "2019-03-22T04:18:22.8569543Z",
    "id": "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<ResourceGroup>/PROVIDERS/MICROSOFT.INSIGHTS/SCHEDULEDQUERYRULES/TEST-BAD-ALERTS",
    "level": "Informational",
    "operationId": "",
    "operationName": {
        "value": "Microsoft.Insights/ScheduledQueryRules/disable/action",
        "localizedValue": "Microsoft.Insights/ScheduledQueryRules/disable/action"
    },
    "resourceGroupName": "<Resource Group>",
    "resourceProviderName": {
        "value": "MICROSOFT.INSIGHTS",
        "localizedValue": "Microsoft Insights"
    },
    "resourceType": {
        "value": "MICROSOFT.INSIGHTS/scheduledqueryrules",
        "localizedValue": "MICROSOFT.INSIGHTS/scheduledqueryrules"
    },
    "resourceId": "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<ResourceGroup>/PROVIDERS/MICROSOFT.INSIGHTS/SCHEDULEDQUERYRULES/TEST-BAD-ALERTS",
    "status": {
        "value": "Succeeded",
        "localizedValue": "Succeeded"
    },
    "subStatus": {
        "value": "",
        "localizedValue": ""
    },
    "submissionTimestamp": "2019-03-22T04:18:22.8569543Z",
    "subscriptionId": "<SubscriptionId>",
    "properties": {
        "resourceId": "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<ResourceGroup>/PROVIDERS/MICROSOFT.INSIGHTS/SCHEDULEDQUERYRULES/TEST-BAD-ALERTS",
        "subscriptionId": "<SubscriptionId>",
        "resourceGroup": "<ResourceGroup>",
        "eventDataId": "12e12345-12dd-1234-8e3e-12345b7a1234",
        "eventTimeStamp": "03/22/2019 04:18:22",
        "issueStartTime": "03/22/2019 04:18:22",
        "operationName": "Microsoft.Insights/ScheduledQueryRules/disable/action",
        "status": "Succeeded",
        "reason": "Alert has been failing consistently with the same exception for the past week"
    },
    "relatedEvents": []
}
```

### <a name="query-used-in-a-log-alert-is-not-valid"></a>Günlük uyarısında kullanılan sorgu geçerli değil

Yapılandırmasının bir parçası olarak Azure Monitor'da oluşturulan her günlük uyarı kuralı, uyarı hizmetinin düzenli olarak çalışacağı bir analiz sorgusu belirtmelidir. Analitik sorgusu, kural oluşturma veya güncelleştirme sırasında doğru sözdizimi olabilir. Ancak bazen, bir süre içinde, günlük uyarısı kuralında sağlanan sorgu sözdizimi sorunları geliştirebilir ve kural yürütmenin başarısız olmasına neden olabilir. Günlük uyarı kuralında sağlanan bir analiz sorgusunun hata lar geliştirmesinin yaygın nedenlerinden bazıları şunlardır:

- Sorgu, birden [çok kaynak arasında çalışacak](../log-query/cross-workspace-query.md)şekilde yazılır. Ve belirtilen kaynaklardan biri veya birkaçı artık yok.
- [Metrik ölçüm türü günlük uyarısı](../../azure-monitor/platform/alerts-unified-log.md#metric-measurement-alert-rules) yapılandırılan bir uyarı sorgusu sözdizimi normlarına uymaz
- Analiz platformuna veri akışı yapılmadı. Sağlanan sorgu için veri olmadığından [sorgu yürütme sayılsa hata verir.](https://dev.loganalytics.io/documentation/Using-the-API/Errors)
- Sorgu [dilindeki](https://docs.microsoft.com/azure/kusto/query/) değişiklikler, komutlar ve işlevler için gözden geçirilmiş bir biçim içerir. Bu nedenle, daha önce bir uyarı kuralında sağlanan sorgu artık geçerli değildir.

[Azure Danışmanı](../../advisor/advisor-overview.md) bu davranış hakkında sizi uyarır. Azure Advisor'daki belirli günlük uyarı kuralı için, orta etkili Yüksek Kullanılabilirlik kategorisi ve "İzlemeyi sağlamak için günlük uyarı kuralını onarın" açıklaması altında bir öneri eklenir. Azure Danışmanı yedi gün boyunca bir öneri sağladıktan sonra günlük uyarısı kuralındaki bir uyarı sorgusu düzeltilmezse, Azure Monitor günlük uyarısını devre dışı ve kural önemli bir süre boyunca sürekli olarak çalışmadığında gereksiz yere faturalandırılmadığınızı garanti edecektir ( bir hafta gibi).

[Azure Etkinlik Günlüğü'nde](../../azure-resource-manager/management/view-activity-logs.md)bir olay arayarak Azure Monitor'un günlük uyarı kuralını devre dışı bırakma saatini tam olarak bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure'daki günlük uyarıları](../platform/alerts-unified-log.md)hakkında bilgi edinin.
- [Uygulama Öngörüleri](../../azure-monitor/app/analytics.md)hakkında daha fazla bilgi edinin.
- [Günlük sorguları](../log-query/log-query-overview.md)hakkında daha fazla bilgi edinin.
