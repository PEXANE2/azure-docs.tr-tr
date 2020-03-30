---
title: Azure Monitor ile kaynaklar arasında sorgu | Microsoft Dokümanlar
description: Bu makalede, aboneliğinizde birden çok çalışma alanından ve App Insights uygulamasından kaynaklara karşı nasıl sorgu yapabileceğiniz açıklanmaktadır.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 06/05/2019
ms.openlocfilehash: 4740034bd970f42833125fa43bfdf72f710ac147
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79249613"
---
# <a name="perform-cross-resource-log-queries-in-azure-monitor"></a>Azure Monitor'da kaynaklar arası günlük sorguları gerçekleştirme  

Azure Monitor ile önceden, yalnızca geçerli çalışma alanı içindeki verileri çözümleyebilirdin ve aboneliğinizde tanımlanan birden çok çalışma alanında sorgu yapma yeteneğinizi sınırlamıştır.  Ayrıca, yalnızca web tabanlı uygulamanızdan toplanan telemetri öğelerini doğrudan Application Insights veya Visual Studio'da Uygulama Öngörüleri ile arayabilirsiniz. Bu aynı zamanda operasyonel ve uygulama verilerini birlikte doğal olarak analiz etmeyi de zorlaştırdı.

Artık yalnızca birden çok Log Analytics çalışma alanında değil, aynı kaynak grubundaki belirli bir Uygulama Öngörüleri uygulamasından, başka bir kaynak grubundan veya başka bir abonelikten gelen verileri de sorgulayabilirsiniz. Bu, verilerinizin sistem genelinde bir görünümünü sağlar. Bu tür sorguları yalnızca [Log Analytics'te](portals.md)gerçekleştirebilirsiniz.

## <a name="cross-resource-query-limits"></a>Çapraz kaynak sorgu sınırları 

* Tek bir sorguya eklenebilen Application Insights kaynaklarının ve Log Analytics çalışma alanlarının sayısı 100 ile sınırlıdır.
* Kaynak karşıtması View Designer'da desteklenmez. Log Analytics'te bir sorgu yazabilir ve [günlük sorgusunun görselleştirilmesi](../learn/tutorial-logs-dashboards.md)için bunu Azure panosuna sabitleyebilirsiniz. 
* Günlük uyarıları çapraz kaynak sorgusu yeni [scheduledQueryRules API'de](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules)desteklenir. Varsayılan olarak, Azure Monitor, eski Log Uyarıları API'sinden geçiş yaptığınız sürece Azure portalından yeni günlük uyarı kuralları oluşturmak için [eski Log Analytics Alert API'sini](../platform/api-alerts.md) kullanır. [legacy Log Alerts API](../platform/alerts-log-api-switch.md#process-of-switching-from-legacy-log-alerts-api) Geçişten sonra, yeni API Azure portalında yeni uyarı kuralları için varsayılan olur ve kaynaklar arası sorgu günlüğü uyarıları kuralları oluşturmanıza olanak tanır. [ZamanlanmışQueryRules API için Azure Kaynak Yöneticisi şablonu](../platform/alerts-log.md#log-alert-with-cross-resource-query-using-azure-resource-template) kullanarak geçiş yapmadan kaynaklar arası sorgu günlüğü uyarı kuralları oluşturabilirsiniz – ancak bu uyarı kuralı Azure portalından değil, [planlanmış QueryRules API'si](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) olsa da yönetilebilir.


## <a name="querying-across-log-analytics-workspaces-and-from-application-insights"></a>Log Analytics çalışma alanlarında ve Application Insights'tan sorgulama
Sorgunuzdaki başka bir çalışma alanına başvurmak için [*çalışma alanı*](https://docs.microsoft.com/azure/log-analytics/query-language/workspace-expression) tanımlayıcısını kullanın ve Application Insights'tan bir uygulama için [*uygulama*](https://docs.microsoft.com/azure/log-analytics/query-language/app-expression) tanımlayıcısını kullanın.  

### <a name="identifying-workspace-resources"></a>Çalışma alanı kaynaklarını tanımlama
Aşağıdaki örnekler, *contosoretail-it*adlı bir çalışma alanında Güncelleştirme tablosundan özet günlük sayısını döndürmek için Log Analytics çalışma alanlarındaki sorguları gösterir. 

Çalışma alanını tanımlama birkaç yoldan biri gerçekleştirilebilir:

* Kaynak adı - çalışma alanının insan tarafından okunabilir bir *adıdır,* bazen bileşen adı olarak adlandırılır. 

    `workspace("contosoretail-it").Update | count`

* Nitelikli ad - bu formattaki abonelik adı, kaynak grubu ve bileşen adından oluşan çalışma alanının "tam adı"dır: *subscriptionName/resourceGroup/componentName*. 

    `workspace('contoso/contosoretail/contosoretail-it').Update | count`

    >[!NOTE]
    >Azure abonelik adları benzersiz olmadığından, bu tanımlayıcı belirsiz olabilir. 
    >

* Çalışma alanı kimliği - Çalışma alanı kimliği, genel olarak benzersiz bir tanımlayıcı (GUID) olarak temsil edilen her çalışma alanına atanan benzersiz, değişmez, tanımlayıcıdır.

    `workspace("b459b4u5-912x-46d5-9cb1-p43069212nb4").Update | count`

* Azure Kaynak Kimliği – çalışma alanının Azure tanımlı benzersiz kimliği. Kaynak adı belirsiz olduğunda Kaynak Kimliğini kullanırsınız.  Çalışma alanları için biçim şudur: */subscriptions/subscriptionId/resourcegroups/resourceGroup/providers/microsoft. OperationalInsights/çalışma alanları/componentName*.  

    Örnek:
    ``` 
    workspace("/subscriptions/e427519-5645-8x4e-1v67-3b84b59a1985/resourcegroups/ContosoAzureHQ/providers/Microsoft.OperationalInsights/workspaces/contosoretail-it").Update | count
    ```

### <a name="identifying-an-application"></a>Bir uygulamayı tanımlama
Aşağıdaki örnekler, Uygulama Öngörüleri'nde *fabrikamapp* adlı bir uygulamaya karşı yapılan isteklerin özet sayısını döndürer. 

Uygulama Öngörüleri'nde bir uygulamanın *tanımlanması, uygulama (Tanımlayıcı)* ifadesi ile gerçekleştirilebilir.  *Tanımlayıcı* bağımsız değişkeni aşağıdakilerden birini kullanarak uygulamayı belirtir:

* Kaynak adı - uygulamanın insan tarafından okunabilir bir adıdır, bazen *bileşen adı*olarak adlandırılır.  

    `app("fabrikamapp")`

    >[!NOTE]
    >Bir uygulamayı ada göre tanımlamak, erişilebilir tüm aboneliklerde benzersizlik varsayar. Belirtilen ada sahip birden çok uygulamanız varsa, sorgu belirsizlik nedeniyle başarısız olur. Bu durumda, diğer tanımlayıcılardan birini kullanmanız gerekir.

* Nitelikli ad - bu formattaki abonelik adı, kaynak grubu ve bileşen adından oluşan uygulamanın "tam adı"dır: *subscriptionName/resourceGroup/componentName*. 

    `app("AI-Prototype/Fabrikam/fabrikamapp").requests | count`

     >[!NOTE]
    >Azure abonelik adları benzersiz olmadığından, bu tanımlayıcı belirsiz olabilir. 
    >

* ID - uygulamanın GUID uygulaması.

    `app("b459b4f6-912x-46d5-9cb1-b43069212ab4").requests | count`

* Azure Kaynak Kimliği - uygulamanın Azure tanımlı benzersiz kimliği. Kaynak adı belirsiz olduğunda Kaynak Kimliğini kullanırsınız. Biçimi: */subscriptions/subscriptionId/resourcegroups/resourceGroup/providers/microsoft. OperationalInsights/components/componentName*.  

    Örnek:
    ```
    app("/subscriptions/b459b4f6-912x-46d5-9cb1-b43069212ab4/resourcegroups/Fabrikam/providers/microsoft.insights/components/fabrikamapp").requests | count
    ```

### <a name="performing-a-query-across-multiple-resources"></a>Birden çok kaynak arasında sorgu gerçekleştirme
Kaynak örneklerinizden birden çok kaynağı sorgulayabilirsiniz, bunlar çalışma alanları ve uygulamalar birleştirilmiş olabilir.
    
İki çalışma alanı boyunca sorgu örneği:    

```
union Update, workspace("contosoretail-it").Update, workspace("b459b4u5-912x-46d5-9cb1-p43069212nb4").Update
| where TimeGenerated >= ago(1h)
| where UpdateState == "Needed"
| summarize dcount(Computer) by Classification
```

## <a name="using-cross-resource-query-for-multiple-resources"></a>Birden çok kaynak için çapraz kaynak sorgusu kullanma
Birden çok Log Analytics çalışma alanlarından ve Application Insights kaynaklarından gelen verileri ilişkilendirmek için kaynaklar arası sorgular kullanılırken, sorgu karmaşık ve bakımı zor hale gelebilir. Sorgu mantığını sorgu yapısını basitleştiren sorgu kaynaklarının kapsamlarından ayırmak için [Azure Monitor günlük sorgularında işlevlerden](functions.md) yararlanmalısınız. Aşağıdaki örnek, birden çok Application Insights kaynaklarını nasıl izleyebilirsiniz ve başarısız isteklerin sayısını uygulama adına göre nasıl görselleştirebileceğinizi gösterir. 

Uygulama Öngörüleri kaynaklarının kapsamına başvuran aşağıdaki gibi bir sorgu oluşturun. Komut, `withsource= SourceApp` günlüğü gönderen uygulama adını belirten bir sütun ekler. Diğer ad _uygulamalarıSCoping_ile [işlev olarak sorgu kaydedin.](functions.md#create-a-function)

```Kusto
// crossResource function that scopes my Application Insights resources
union withsource= SourceApp
app('Contoso-app1').requests, 
app('Contoso-app2').requests,
app('Contoso-app3').requests,
app('Contoso-app4').requests,
app('Contoso-app5').requests
```



Artık bu işlevi aşağıdaki gibi bir çapraz kaynak sorgusunda [kullanabilirsiniz.](../../azure-monitor/log-query/functions.md#use-a-function) İşlev diğer adı _UygulamalarıScoping,_ tüm tanımlanan uygulamalardan istekler tablosunun birleşimini döndürür. Sorgu daha sonra başarısız istekleri filtreler ve eğilimleri uygulamaya göre görselleştirir. _Ayrıştırma_ işleci bu örnekte isteğe bağlıdır. Uygulama adını _SourceApp_ özelliğinden çıkarır.

```Kusto
applicationsScoping 
| where timestamp > ago(12h)
| where success == 'False'
| parse SourceApp with * '(' applicationName ')' * 
| summarize count() by applicationName, bin(timestamp, 1h) 
| render timechart
```

>[!NOTE]
>Çalışma alanları ve uygulamalar da dahil olmak üzere uyarı kuralı kaynaklarının erişim doğrulaması uyarı oluşturma zamanında gerçekleştirildığından, bu yöntem günlük uyarılarıyla kullanılamaz. Uyarı oluşturma desteklenmeden sonra işleve yeni kaynaklar ekleme. Günlük uyarılarında kaynak kapsamı için işlevi kullanmayı tercih ederseniz, kapsamlı kaynakları güncelleştirmek için portaldaki veya Kaynak Yöneticisi şablonundaki uyarı kuralını yeniden oluşturmanız gerekir. Alternatif olarak, günlük uyarı sorgusuna kaynak listesini ekleyebilirsiniz.


![Zaman Çizelgesi](media/cross-workspace-query/chart.png)

## <a name="next-steps"></a>Sonraki adımlar

- Günlük sorgularına ve Azure Monitor günlük verilerinin nasıl yapılandırıldığına genel bir bakış için [Azure Monitor'daki günlük verilerini analiz](log-query-overview.md) edin.
- Azure Monitor günlük sorgularının tüm kaynaklarını görüntülemek için [Azure Monitor günlük sorgularını](query-language.md) gözden geçirin.
