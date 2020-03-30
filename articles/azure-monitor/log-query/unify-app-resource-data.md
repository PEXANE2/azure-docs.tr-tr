---
title: Birden çok Azure Monitörü Uygulama Öngörüleri kaynaklarını birliyi birle | Microsoft Dokümanlar
description: Bu makalede, birden çok Application Insights kaynaklarını sorgulamak ve bu verileri görselleştirmek için Azure Monitor Günlükleri'nde bir işlevin nasıl kullanılacağı hakkında ayrıntılı bilgi verilmektedir.
author: bwren
ms.author: bwren
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/02/2020
ms.openlocfilehash: ce58aae3b1db1f0f338d353025d4f277aeb6944f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77137489"
---
# <a name="unify-multiple-azure-monitor-application-insights-resources"></a>Birden çok Azure Monitörü Uygulama Öngörüleri kaynaklarını birlikin 
Bu makalede, Uygulama Öngörüleri Bağlayıcısı'nın amortismanının yerine, farklı Azure aboneliklerinde olsalar bile tüm Uygulama Öngörüleriniz günlük verilerinizi tek bir yerde nasıl sorgulayıp görüntüleyeniz açıklanmaktadır. Tek bir sorguya eklediğiniz Application Insights kaynaklarının sayısı 100 ile sınırlıdır.

## <a name="recommended-approach-to-query-multiple-application-insights-resources"></a>Birden çok Application Insights kaynaklarını sorgulamak için önerilen yaklaşım 
Bir sorguda birden çok Application Insights kaynağının listelenmesi hantal ve bakımı zor olabilir. Bunun yerine, sorgu mantığını kapsam adedinden ayırmak için işlevden yararlanabilirsiniz.  

Bu örnek, birden çok Application Insights kaynaklarını nasıl izleyebilirsiniz ve başarısız isteklerin sayısını uygulama adına göre nasıl görselleştirebileceğinizi gösterir.

Uygulama listesi ile birliktelik işleci kullanarak bir işlev oluşturun, sonra diğer uygulama *scoping*ile işlev olarak çalışma alanınızda sorgu kaydedin. 

Portalda listelenen uygulamaları, çalışma alanınızda Sorgu gezgini'ne yönlendirerek ve düzenleme ve ardından kaydetmek veya PowerShell `SavedSearch` cmdlet'i kullanarak işlevi seçerek istediğiniz zaman değiştirebilirsiniz. 

>[!NOTE]
>Çalışma alanları ve uygulamalar da dahil olmak üzere uyarı kuralı kaynaklarının erişim doğrulaması uyarı oluşturma zamanında gerçekleştirildığından, bu yöntem günlük uyarılarıyla kullanılamaz. Uyarı oluşturma desteklenmeden sonra işleve yeni kaynaklar ekleme. Günlük uyarılarında kaynak kapsamı için işlevi kullanmayı tercih ederseniz, kapsamlı kaynakları güncelleştirmek için portaldaki veya Kaynak Yöneticisi şablonundaki uyarı kuralını yeniden oluşturmanız gerekir. Alternatif olarak, günlük uyarı sorgusuna kaynak listesini ekleyebilirsiniz.

Komut, `withsource= SourceApp` günlüğü gönderen uygulamayı belirleyen sonuçlara bir sütun ekler. Ayrıştırma işleci bu örnekte isteğe bağlıdır ve uygulama adını SourceApp özelliğinden ayıklamak için kullanır. 

```
union withsource=SourceApp 
app('Contoso-app1').requests,  
app('Contoso-app2').requests, 
app('Contoso-app3').requests, 
app('Contoso-app4').requests, 
app('Contoso-app5').requests 
| parse SourceApp with * "('" applicationName "')" *  
```

Artık çapraz kaynak sorgusunda uygulamaları Scoping işlevini kullanmaya hazırsınız:  

```
applicationsScoping 
| where timestamp > ago(12h)
| where success == 'False'
| parse SourceApp with * '(' applicationName ')' * 
| summarize count() by applicationName, bin(timestamp, 1h) 
| render timechart
```

UygulamaScoping işlevi Uygulama Öngörüleri veri yapısını döndürdükten sonra sorgu çalışma alanında yürütülse de, sorgu Application Insights şemasını kullanır. İşlev diğer adı, tanımlanan tüm uygulamalardan gelen isteklerin birleşimini döndürür. Sorgu daha sonra başarısız istekleri filtreler ve eğilimleri uygulamaya göre görselleştirir.

![Çapraz sorgu sonuçları örneği](media/unify-app-resource-data/app-insights-query-results.png)

>[!NOTE]
>Günlük uyarıları çapraz [kaynak sorgusu](../log-query/cross-workspace-query.md) yeni [scheduledQueryRules API'de](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules)desteklenir. Varsayılan olarak, Azure Monitor, eski Log Uyarıları API'sinden geçiş yaptığınız sürece Azure portalından yeni günlük uyarı kuralları oluşturmak için [eski Log Analytics Alert API'sini](../platform/api-alerts.md) kullanır. [legacy Log Alerts API](../platform/alerts-log-api-switch.md#process-of-switching-from-legacy-log-alerts-api) Geçişten sonra, yeni API Azure portalında yeni uyarı kuralları için varsayılan olur ve kaynaklar arası sorgu günlüğü uyarıları kuralları oluşturmanıza olanak tanır. [ScheduledQueryRules API için ARM şablonu](../platform/alerts-log.md#log-alert-with-cross-resource-query-using-azure-resource-template) kullanarak geçiş yapmadan [kaynaklar arası sorgu](../log-query/cross-workspace-query.md) günlüğü uyarı kuralları oluşturabilirsiniz – ancak bu uyarı kuralı, Azure portalından değil, [planlanmışQueryRules API'si](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) olsa da yönetilebilir.

## <a name="application-insights-and-log-analytics-workspace-schema-differences"></a>Uygulama Öngörüleri ve Log Analytics çalışma alanı şema farklılıkları
Aşağıdaki tablo, Log Analytics ve Application Insights arasındaki şema farklılıklarını göstermektedir.  

| Günlük Analytics çalışma alanı özellikleri| Uygulama Öngörüleri kaynak özellikleri|
|------------|------------| 
| AnonUserId | user_id|
| ApplicationID | appId|
| ApplicationName | Uygadı|
| UygulamaTypeVersion | application_Version |
| Kullanılabilirlik Sayısı | ıtemcount |
| KullanılabilirlikSüresi | süre |
| Kullanılabilirlik Mesajı | message |
| AvailabilityRunLocation | location |
| AvailabilityTestid | id |
| AvailabilityTestName | ad |
| KullanılabilirlikZaman Damgası | timestamp |
| Tarayıcı | client_browser |
| Şehir | client_city |
| Müşteri IP | client_IP |
| Bilgisayar | cloud_RoleInstance | 
| Ülke | client_CountryOrRegion | 
| CustomEventCount | ıtemcount | 
| ÖzelEventDimensions | özelBoyutlar |
| CustomEventName | ad | 
| DeviceModel | client_Model | 
| DeviceType | client_Type | 
| Özel Durum Sayısı | ıtemcount | 
| Özel durumele | handledAt |
| Özel Durum Mesajı | message | 
| Özel Durum Türü | type |
| OperationID | operation_id |
| ThrottledRequests | operation_Name | 
| İşletim Sistemi | client_OS | 
| PageViewCount | ıtemcount |
| Sayfa Görüntüleme Süresi | süre | 
| PageViewName | ad | 
| ParentOperationID | operation_Id | 
| İstek Sayısı | ıtemcount | 
| İstekSüresi | süre | 
| RequestID | id | 
| İstek Adı | ad | 
| İstekBaşarı | başarılı | 
| Yanıt Kodu | Resultcode | 
| Rol | cloud_RoleName |
| RoleInstance | cloud_RoleInstance |
| SessionId | Session_id | 
| SourceSystem | operation_SyntheticSource |
| TelemetriTYpe | type |
| URL'si | url |
| UserAccountId | user_AccountId |

## <a name="next-steps"></a>Sonraki adımlar

Uygulama Öngörüleri uygulamalarınız için ayrıntılı bilgileri görüntülemek için [Günlük Arama'yı](../../azure-monitor/log-query/log-query-overview.md) kullanın.
