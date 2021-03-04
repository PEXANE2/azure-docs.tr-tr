---
title: Azure Izleyici 'de günlük uyarıları sorunlarını giderme | Microsoft Docs
description: Azure 'da günlük uyarısı kuralları için genel sorunlar, hatalar ve çözümler.
author: yanivlavi
ms.author: yalavi
ms.topic: conceptual
ms.date: 09/22/2020
ms.openlocfilehash: 6b495e8e700f146f5e6c117413456311ac0cdd5a
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102037889"
---
# <a name="troubleshoot-log-alerts-in-azure-monitor"></a>Azure Izleyici 'de günlük uyarıları sorunlarını giderme  

Bu makalede, Azure Izleyici 'de günlük uyarılarla ilgili yaygın sorunları nasıl giderecağınızı gösterilmektedir. Ayrıca, günlük uyarılarının işlevleri ve yapılandırmasıyla ilgili sık karşılaşılan sorunlara çözümler sağlar.

Günlük uyarıları, kullanıcıların kaynak günlüklerini her ayarlama sıklığını değerlendirmek ve sonuçlara göre bir uyarı tetiklemesi için [Log Analytics](../logs/log-analytics-tutorial.md) bir sorgu kullanmasına izin verir. Kurallar, [eylem gruplarını](./action-groups.md)kullanarak bir veya daha fazla eylemi tetikleyebilir. [Günlük uyarılarının işlevselliği ve terminolojisi hakkında daha fazla bilgi edinin](alerts-unified-log.md).

> [!NOTE]
> Bu makalede, Azure portal tetiklenen bir uyarı kuralı gösterdiği ve ilişkili bir eylem grubu tarafından bir bildirim gerçekleştirilmediği durumlar dikkate almaz. Bu gibi durumlarda [, sorun giderme](./alerts-troubleshoot.md#action-or-notification-on-my-alert-did-not-work-as-expected)hakkındaki ayrıntılara bakın.

## <a name="log-alert-didnt-fire"></a>Günlük uyarısı tetikmedi

### <a name="data-ingestion-time-for-logs"></a>Günlükler için veri alımı süresi

Azure Izleyici, [günlük alma gecikmesi](../logs/data-ingestion-time.md)oluşmasına yol açabilecek şekilde dünya genelindeki terabaytlık müşterilerin günlüklerini işler.

Günlüklerde yarı yapılandırılmış veriler ve doğal olarak ölçülerden daha fazla bilgi verilir. Tetiklenen uyarılarda 4 dakikalık bir gecikme yaşıyorsanız, [ölçüm uyarılarını](alerts-metric-overview.md)kullanmayı göz önünde bulundurmanız gerekir. [Günlükler için ölçüm uyarılarını](alerts-metric-logs.md)kullanarak günlüklerden ölçüm deposuna veri gönderebilirsiniz.

Sistem, gecikme süresini azaltmak için uyarı değerlendirmesini birden çok kez yeniden dener. Veriler ulaştıktan sonra uyarı ateşlenir, bu durum çoğu durumda günlük kaydı zamanına eşit değildir.

### <a name="incorrect-query-time-range-configured"></a>Yanlış sorgu zaman aralığı yapılandırıldı

Sorgu zaman aralığı, kural koşulu tanımında ayarlanır. Bu alana, çalışma alanları ve Application Insights için **süre** ve diğer tüm kaynak türleri Için **geçersiz kılma sorgu zaman aralığı** adı verilir. Log Analytics 'te olduğu gibi, zaman aralığı sorgu verilerini belirtilen döneme göre sınırlandırır. Sorguda kullanım **dışı komut kullanılsa** bile, zaman aralığı uygulanır. 

Örneğin, bir sorgu, zaman aralığı 60 dakika olduğunda ve metin **önce (1D)** olsa bile, bir sorgu 60 dakika tarar. Zaman aralığı ve sorgu zaman filtrelemenin eşleşmesi gerekir. Örnek örnekte, **Dönem**  /  **geçersiz kılma sorgu zaman aralığını** bir güne değiştirmek beklenen şekilde çalışır.

![Süre](media/alerts-troubleshoot-log/LogAlertTimePeriod.png)

### <a name="actions-are-muted-in-the-alert-rule"></a>Uyarı kuralında eylemler kapalı

Günlük uyarıları, ayarlanan bir süre boyunca tetiklenen uyarı eylemlerini sessize etmek için bir seçenek sağlar. Bu alan, çalışma alanlarında ve Application Insights **uyarıları gösterme** olarak adlandırılır. Diğer tüm kaynak türlerinde, bu, **sessiz eylemler** olarak adlandırılır. 

Yaygın bir sorun, bir hizmet sorunu nedeniyle uyarının eylemleri tetikleyemedi olduğunu düşündüğünüzden oluşur. Bu, kural yapılandırması tarafından sessize bırakılmıştı.

![Uyarıları bastır](media/alerts-troubleshoot-log/LogAlertSuppress.png)

### <a name="metric-measurement-alert-rule-with-splitting-using-the-legacy-log-analytics-api"></a>Eski Log Analytics API 'sini kullanarak bölme ile ölçüm ölçümü uyarı kuralı

[Ölçüm ölçümü](alerts-unified-log.md#calculation-of-measure-based-on-a-numeric-column-such-as-cpu-counter-value) , özetlenen zaman serisi sonuçlarına dayalı bir günlük uyarısı türüdür. Bu kurallar, [uyarıların bölünmesi](alerts-unified-log.md#split-by-alert-dimensions)için sütunlara göre gruplandırılmasına olanak sağlar. Eski Log Analytics API kullanıyorsanız, bölme beklendiği gibi çalışmaz. Eski API 'de gruplamayı seçme desteklenmez.

Geçerli ScheduledQueryRules API 'SI, beklenen şekilde çalışacak şekilde [ölçüm ölçüm](alerts-unified-log.md#calculation-of-measure-based-on-a-numeric-column-such-as-cpu-counter-value) kurallarında **toplu** olarak ayarlamanıza olanak tanır. [Geçerli ScheduledQueryRules API 'sine geçiş hakkında daha fazla bilgi edinin](../alerts/alerts-log-api-switch.md).

## <a name="log-alert-fired-unnecessarily"></a>Günlük uyarısı gereksiz şekilde tetiklendi

[Azure izleyici 'de yapılandırılmış bir günlük uyarısı kuralı](./alerts-log.md) beklenmedik şekilde tetiklenebilir. Aşağıdaki bölümlerde bazı yaygın nedenler açıklanmaktadır.

### <a name="alert-triggered-by-partial-data"></a>Kısmi verilerin tetiklediği uyarı

Azure Izleyici, [günlük alma gecikmesi](../logs/data-ingestion-time.md)oluşmasına yol açabilecek şekilde dünya genelindeki terabaytlık müşterilerin günlüklerini işler.

Günlüklerde yarı yapılandırılmış veriler ve doğal olarak ölçülerden daha fazla bilgi verilir. Tetiklenen uyarılarda çok sayıda hatalı ateşlenir, [ölçüm uyarılarını](alerts-metric-overview.md)kullanmayı göz önünde bulundurmanız gerekir. [Günlükler için ölçüm uyarılarını](alerts-metric-logs.md)kullanarak günlüklerden ölçüm deposuna veri gönderebilirsiniz.

Günlük uyarıları, günlüklerdeki verileri algılamaya çalıştığınızda en iyi şekilde çalışır. Günlüklerde veri eksikliklerini algılamaya çalıştığınızda daha az iyi çalışır. Örneğin, sanal makine sinyalde uyarma. 

Yanlış uyarıları önlemeye yönelik yerleşik bir özellik olsa da, bunlar (yaklaşık 30 dakika içinde) ve gecikme süreleri olan veriler üzerinde yine de gerçekleşebilir.

### <a name="query-optimization-issues"></a>Sorgu iyileştirme sorunları

Uyarı hizmeti, daha düşük yük ve uyarı gecikmesini iyileştirmek için sorgunuzu değiştirir. Uyarı akışı, sorunu belirten sonuçları bir uyarıya dönüştürmek için oluşturulmuştur. Örneğin, şöyle bir sorgu söz konusu olduğunda:

``` Kusto
SecurityEvent
| where EventID == 4624
```

Kullanıcının amacı uyarılardaysa, bu olay türü gerçekleştiğinde, uyarı mantığı `count` sorguya ekler. Çalıştırılacak sorgu şu şekilde olacaktır:

``` Kusto
SecurityEvent
| where EventID == 4624
| count
```

Sorguya uyarı mantığı eklemeniz ve bu durum bile sorunlara neden olabilir. Yukarıdaki örnekte, sorgunuza eklerseniz, `count` Uyarı hizmeti ' nin yapamasından bu yana her zaman değeri 1 ' de olur `count` `count` .

En iyi duruma getirilmiş sorgu, günlük uyarı hizmetinin çalıştığı şeydir. Değiştirilen sorguyu Log Analytics [portalında](../logs/log-query-overview.md) veya [API](/rest/api/loganalytics/)'de çalıştırabilirsiniz.

Çalışma alanları ve Application Insights için, koşul bölmesinde **yürütülecek sorgu** olarak adlandırılır. Diğer tüm kaynak türlerinde, koşul sekmesinde **son uyarı sorgusunu göster** ' i seçin.

![Yürütülecek sorgu](media/alerts-troubleshoot-log/LogAlertPreview.png)

## <a name="log-alert-was-disabled"></a>Günlük uyarısı devre dışı bırakıldı

Aşağıdaki bölümlerde, Azure Izleyici 'nin bir günlük uyarı kuralını devre dışı bırakabileceği bazı nedenler listelenmektedir. Ayrıca [, bir kural devre dışı bırakıldığında gönderilen etkinlik günlüğüne bir örnek](#activity-log-example-when-rule-is-disabled)de sunuyoruz.

### <a name="alert-scope-no-longer-exists-or-was-moved"></a>Uyarı kapsamı artık yok veya taşındı

Bir uyarı kuralının kapsam kaynakları artık geçerli olmadığında, kuralın yürütülmesi başarısız olur. Bu durumda, faturalandırma de duraklar.

Azure Izleyici sürekli olarak başarısız olursa bir hafta sonra günlük uyarısını devre dışı bırakır.

### <a name="query-used-in-a-log-alert-isnt-valid"></a>Günlük uyarısıyla kullanılan sorgu geçerli değil

Bir günlük uyarı kuralı oluşturulduğunda, sorgu doğru sözdizimi için onaylanır. Ancak bazen, günlük uyarı kuralında belirtilen sorgu başarısız olabilir. Bazı yaygın nedenler şunlardır:

- Kurallar API aracılığıyla oluşturuldu ve Kullanıcı tarafından doğrulama atlandı.
- Sorgu [birden fazla kaynak üzerinde çalışır](../logs/cross-workspace-query.md) ve bir veya daha fazla kaynak silinmiş veya taşınmış.
- Sorgu şu nedenle [başarısız olur](https://dev.loganalytics.io/documentation/Using-the-API/Errors) :
    - Günlüğe kaydetme çözümü [çalışma alanına dağıtılmadı](../insights/solutions.md#install-a-monitoring-solution), bu nedenle tablolar oluşturulmadı.
    - Veriler 30 gün boyunca sorgudaki bir tabloya akan şekilde durdu.
    - Veri akışı başlamadığından [özel günlük tabloları](../agents/data-sources-custom-logs.md) henüz oluşturulmaz.
- [Sorgu dilindeki](/azure/kusto/query/) değişiklikler, komutlar ve işlevler için düzeltilmiş bir biçim içerir. Bu nedenle, daha önce sağlanmış olan sorgu artık geçerli değildir.

[Azure Advisor](../../advisor/advisor-overview.md) bu davranışla ilgili olarak sizi uyarır. Etkilenen günlük uyarısı kuralı hakkında bir öneri ekler. Kullanılan kategori orta etkisi olan ' yüksek kullanılabilirlik ' ve ' izlemeyi sağlamak için günlük uyarı kuralınızı onarma ' açıklamasına sahip.

## <a name="alert-rule-quota-was-reached"></a>Uyarı kuralı kotasına ulaşıldı

Abonelik ve kaynak başına günlük araması uyarı kuralı sayısı [burada](../service-limits.md) açıklanan kota sınırlarına tabidir.

### <a name="recommended-steps"></a>Önerilen Adımlar
    
Kota sınırına ulaştıysanız, aşağıdaki adımlar sorunu çözmeye yardımcı olabilir.

1. Artık kullanılmayan günlük arama uyarısı kurallarını silmeyi veya devre dışı bırakmayı deneyin.
1. Kural sayısını azaltmak için [uyarıların boyutlara göre bölünmesini](alerts-unified-log.md#split-by-alert-dimensions) kullanmayı deneyin. Bu kurallar birçok kaynak ve algılama durumunu izleyebilir.
1. Kota sınırının arttırılabilmeniz gerekiyorsa, bir destek isteği açmaya devam edin ve aşağıdaki bilgileri sağlayın:

    - Kota sınırının artırılması gereken abonelik kimlikleri ve kaynak kimlikleri.
    - Kota artışı nedeni.
    - Kota artışı için kaynak türü: **Log Analytics**, **Application Insights**, vb.
    - İstenen kota sınırı.


### <a name="to-check-the-current-usage-of-new-log-alert-rules"></a>Yeni günlük uyarı kurallarının geçerli kullanımını denetlemek için
    
#### <a name="from-the-azure-portal"></a>Azure portalından

1. *Uyarılar* ekranını açın ve *Uyarı kurallarını yönet* ' i seçin.
2. *Abonelik* açılan menüsünü kullanarak ilgili aboneliği filtreleyin
3. Belirli bir kaynak grubuna, kaynak türüne veya kaynağa filtre uyguladığınızdan emin olun
4. *Sinyal türü* açılan menü denetiminde 'Günlük Araması'nı seçin
5. *Durum* açılan menüsünün "Etkin" olduğunu doğrulayın
6. Toplam günlük araması uyarı kuralı sayısı, kural listesinin üzerinde görüntülenir

#### <a name="from-api"></a>API'den

- PowerShell- [Get-AzScheduledQueryRule](/powershell/module/az.monitor/get-azscheduledqueryrule)
- REST API - [Aboneliğe göre listeleme](/rest/api/monitor/scheduledqueryrules/listbysubscription)

## <a name="activity-log-example-when-rule-is-disabled"></a>Kural devre dışı bırakıldığında etkinlik günlüğü örneği

Sorgu yedi gün boyunca sürekli başarısız olursa, Azure Izleyici günlük uyarısını devre dışı bırakır ve kuralın faturalandırmasını durdurur. Azure Izleyici 'nin [Azure etkinlik günlüğündeki](../../azure-resource-manager/management/view-activity-logs.md)günlük uyarısını devre dışı bırakıldığını tam olarak görebilirsiniz. Aşağıdaki örneğe bakın:

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

## <a name="next-steps"></a>Sonraki adımlar

- [Azure 'da günlük uyarıları](./alerts-unified-log.md)hakkında bilgi edinin.
- [Günlük uyarılarını yapılandırma](../logs/log-query-overview.md)hakkında daha fazla bilgi edinin.
- [Günlük sorguları](../logs/log-query-overview.md)hakkında daha fazla bilgi edinin.