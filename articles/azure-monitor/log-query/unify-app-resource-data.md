---
title: Birden çok Azure Izleyici Application Insights kaynağını bütünleştirme | Microsoft Docs
description: Bu makalede, Azure Izleyici günlüklerinde bir işlevi kullanarak birden çok Application Insights kaynağını sorgulama ve bu verileri görselleştirme hakkında ayrıntılar verilmektedir.
author: bwren
ms.author: bwren
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/02/2020
ms.openlocfilehash: ce58aae3b1db1f0f338d353025d4f277aeb6944f
ms.sourcegitcommit: b95983c3735233d2163ef2a81d19a67376bfaf15
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77137489"
---
# <a name="unify-multiple-azure-monitor-application-insights-resources"></a>Birden çok Azure Izleyici Application Insights kaynağını bütünleştirme 
Bu makalede, Application Insights Bağlayıcısı kullanım dışı bırakma işleminin yerine, farklı Azure aboneliklerinde olsalar bile, tüm Application Insights günlük verilerinizi tek bir yerde sorgulama ve görüntüleme açıklanmaktadır. Tek bir sorguya dahil edebilirsiniz Application Insights kaynak sayısı 100 ile sınırlıdır.

## <a name="recommended-approach-to-query-multiple-application-insights-resources"></a>Birden çok Application Insights kaynağını sorgulamak için önerilen yaklaşım 
Bir sorgudaki birden çok Application Insights kaynağın listelenmesi, bakımını yapmak ve sürdürmek zor olabilir. Bunun yerine, uygulama kapsamı 'ndan sorgu mantığını ayırmak için işlevinden yararlanabilirsiniz.  

Bu örnek, birden çok Application Insights kaynağını nasıl izleyebileceğinizi ve başarısız isteklerin sayısını uygulama adına göre görselleştirmenizi gösterir.

Uygulama listesiyle birlikte UNION işlecini kullanarak bir işlev oluşturun, sonra sorguyu çalışma alanınıza, *Applicationsscoping*diğer adı ile işlev olarak kaydedin. 

Çalışma alanınızda sorgu Gezgini ' ne gidip, Düzenle ve sonra kaydetme işlevini seçerek veya `SavedSearch` PowerShell cmdlet 'ini kullanarak, listelenen uygulamaları portalda dilediğiniz zaman değiştirebilirsiniz. 

>[!NOTE]
>Çalışma alanları ve uygulamalar dahil olmak üzere uyarı kuralı kaynaklarının erişim doğrulaması uyarı oluşturma sırasında gerçekleştirildiğinden, bu yöntem günlük uyarıları ile kullanılamaz. Uyarı oluşturulduktan sonra işleve yeni kaynaklar eklemek desteklenmez. Günlük uyarılarında kaynak kapsamı için işlev kullanmayı tercih ederseniz, kapsamdaki kaynakları güncelleştirmek için portalda veya bir Kaynak Yöneticisi şablonuyla uyarı kuralını düzenlemeniz gerekir. Alternatif olarak, günlük uyarısı sorgusuna kaynak listesini de ekleyebilirsiniz.

`withsource= SourceApp` komutu, günlüğü gönderen uygulamayı atayan sonuçlara bir sütun ekler. Parse işleci Bu örnekte isteğe bağlıdır ve SourceApp özelliğinden uygulama adını ayıklamak için kullanır. 

```
union withsource=SourceApp 
app('Contoso-app1').requests,  
app('Contoso-app2').requests, 
app('Contoso-app3').requests, 
app('Contoso-app4').requests, 
app('Contoso-app5').requests 
| parse SourceApp with * "('" applicationName "')" *  
```

Artık çapraz kaynak sorgusunda applicationsScoping işlevini kullanmaya hazırsınız:  

```
applicationsScoping 
| where timestamp > ago(12h)
| where success == 'False'
| parse SourceApp with * '(' applicationName ')' * 
| summarize count() by applicationName, bin(timestamp, 1h) 
| render timechart
```

Sorgu Application Insights şeması kullanır, ancak applicationsScoping işlevi Application Insights veri yapısını döndürdüğünden sorgu çalışma alanında yürütülür. İşlev diğer adı, tüm tanımlanmış uygulamalardan gelen isteklerin birleşimini döndürür. Sorgu daha sonra başarısız isteklere filtre uygular ve eğilimleri uygulamaya göre görselleştirir.

![Çapraz sorgu sonuçları örneği](media/unify-app-resource-data/app-insights-query-results.png)

>[!NOTE]
>Log uyarılarındaki [çapraz kaynak sorgusu](../log-query/cross-workspace-query.md) , yeni [SCHEDULEDQUERYRULES API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules)'sinde desteklenir. Azure Izleyici, [eski günlük uyarıları API](../platform/alerts-log-api-switch.md#process-of-switching-from-legacy-log-alerts-api)'sinden geçiş yapmadığınız takdirde, varsayılan olarak, Azure Portal ' dan yeni günlük uyarı kuralları oluşturmak için [eskı Log Analytics uyarı API](../platform/api-alerts.md) 'sini kullanır. Anahtar sonrasında, yeni API Azure portal yeni uyarı kuralları için varsayılan olur ve çapraz kaynak sorgu günlüğü uyarı kuralları oluşturmanıza olanak sağlar. [Scheduledqueryrules API 'si Için ARM şablonunu](../platform/alerts-log.md#log-alert-with-cross-resource-query-using-azure-resource-template) kullanarak anahtarı yapmadan [çapraz kaynak sorgu](../log-query/cross-workspace-query.md) günlüğü uyarı kuralları oluşturabilirsiniz, ancak bu uyarı kuralı Azure Portal değil, [scheduledqueryrules API 'si](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) ile yönetilebilir.

## <a name="application-insights-and-log-analytics-workspace-schema-differences"></a>Application Insights ve Log Analytics çalışma alanı şema farklılıkları
Aşağıdaki tabloda Log Analytics ve Application Insights arasındaki şema farklılıkları gösterilmektedir.  

| Log Analytics çalışma alanı özellikleri| Application Insights kaynak özellikleri|
|------------|------------| 
| AnonUserId | user_id|
| ApplicationId | appId|
| ApplicationName | appName|
| ApplicationTypeVersion | application_Version |
| AvailabilityCount | itemCount |
| AvailabilityDuration | duration |
| AvailabilityMessage | message |
| AvailabilityRunLocation | location |
| AvailabilityTestId | id |
| AvailabilityTestName | ad |
| AvailabilityTimestamp | timestamp |
| Tarayıcı | client_browser |
| Şehir | client_city |
| ClientIP | client_IP |
| Bilgisayar | cloud_RoleInstance | 
| Ülke | client_CountryOrRegion | 
| CustomEventCount | itemCount | 
| CustomEventDimensions | customDimensions |
| CustomEventName | ad | 
| DeviceModel | client_Model | 
| DeviceType | client_Type | 
| ExceptionCount | itemCount | 
| ExceptionHandledAt | handledAt |
| ExceptionMessage | message | 
| ExceptionType | type |
| OperationID | operation_id |
| için abonelik sınırlarını aştıysanız Hizmet Azaltma gerçekleşir | operation_Name | 
| İşletim Sistemi | client_OS | 
| PageViewCount | itemCount |
| PageViewDuration | duration | 
| PageViewName | ad | 
| ParentOperationID | operation_Id | 
| RequestCount | itemCount | 
| RequestDuration | duration | 
| RequestId | id | 
| RequestName | ad | 
| RequestSuccess | başarılı | 
| ResponseCode | resultCode | 
| Rol | cloud_RoleName |
| Roleınstance | cloud_RoleInstance |
| oturum kimliği | session_Id | 
| SourceSystem | operation_SyntheticSource |
| TelemetryTYpe | type |
| URL'si | url |
| Useraccountıd | user_AccountId |

## <a name="next-steps"></a>Sonraki adımlar

Application Insights uygulamalarınızın ayrıntılı bilgilerini görüntülemek için [günlük araması](../../azure-monitor/log-query/log-query-overview.md) 'nı kullanın.
