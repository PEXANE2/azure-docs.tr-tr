---
title: Azure Izleyici 'yi kullanarak günlük uyarıları oluşturma, görüntüleme ve yönetme | Microsoft Docs
description: Azure 'da günlük uyarı kurallarını yazmak, görüntülemek ve yönetmek için Azure Izleyicisini kullanın.
author: yanivlavi
ms.author: yalavi
ms.topic: conceptual
ms.date: 07/29/2019
ms.subservice: alerts
ms.openlocfilehash: 57cc3624a38fbec1e5bef7bb281363d34acef2b1
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86505610"
---
# <a name="create-view-and-manage-log-alerts-using-azure-monitor"></a>Azure Izleyici 'yi kullanarak günlük uyarıları oluşturma, görüntüleme ve yönetme

## <a name="overview"></a>Genel Bakış
Bu makalede, Azure portal içindeki uyarılar arabirimini kullanarak günlük uyarılarını oluşturma ve yönetme işlemlerinin nasıl yapılacağı gösterilir. Uyarı kuralları üç bileşen tarafından tanımlanır:
- Hedef: izlenecek belirli bir Azure kaynağı
- Ölçüt: Truth için değerlendirilecek bir koşul veya mantık. True ise, uyarı ateşlenir.  
- Eylem: bir bildirim alıcısına gönderilen belirli çağrı-e-posta, SMS, Web kancası vb.

**Günlük uyarısı** , [Log Analytics çalışma alanındaki](../log-query/get-started-portal.md) veya [Application Insights](../log-query/log-query-overview.md) bir günlük sorgusunun değerlendirildiği ve sonuç doğru ise tetiklenen bir uyarı tanımlar. Günlük uyarılarından işlevsellik, terminoloji ve türler hakkında daha fazla bilgi edinin [-genel bakış](alerts-unified-log.md).

> [!NOTE]
> [Bir Log Analytics çalışma alanındaki](../log-query/get-started-portal.md) günlük verileri de Azure izleyici ölçümleri veritabanına yönlendirilebilir. Ölçüm uyarıları [farklı davranışa](alerts-metric-overview.md)sahiptir ve bu işlem, çalıştığınız verilere bağlı olarak daha çok tercih edilebilir.   Günlükleri ölçümlere yönlendirme hakkında bilgi için bkz. [Günlükler Için ölçüm uyarısı](alerts-metric-logs.md).

## <a name="create-a-log-alert-rule-with-the-azure-portal"></a>Azure portal bir günlük uyarı kuralı oluşturun

1. [Portalda](https://portal.azure.com/), **İzle**' yi seçin. Bu bölümde **Uyarılar**' ı seçin.

    ![İzleme](media/alerts-log/AlertsPreviewMenu.png)

1. **Yeni uyarı kuralı**' na tıklayın. 

    ![Uyarı Ekle](media/alerts-log/AlertsPreviewOption.png)

1. **Uyarı oluştur** bölmesi görüntülenir. Dört bölümden oluşur: 
    - Uyarının uygulandığı kaynak
    - Denetlenecek koşul
    - Koşul doğru ise gerçekleştirilecek eylem
    - Uyarının adı ve açıklaması hakkında ayrıntılar. 

    ![Kural Oluştur](media/alerts-log/AlertsPreviewAdd.png)

1. **Kaynak Seç** bağlantısını kullanarak ve bir kaynak seçerek hedefi belirterek uyarı koşulunu tanımlayın. *Abonelik*, *kaynak türü*ve gerekli *kaynağı*seçerek filtre uygulayın. 

   ![Kaynak seçin](media/alerts-log/Alert-SelectResourceLog.png)

1. **Kaynak türünün** *Log Analytics* veya *Application Insights* gibi bir analiz kaynağı ve *günlük*olarak sinyal türü olduğundan emin olun. **Bitti**’ye tıklayın. Ardından, kaynak için kullanılabilen sinyal seçeneklerinin listesini görüntülemek için **Ölçüt Ekle** düğmesini kullanın. Günlük uyarılarınız için verilerin bulunduğu yere bağlı olarak, *Log Analytics* veya *Application Insights*için **özel günlük araması** seçeneğini bulun ve seçin.

   ![Kaynak seçin-özel günlük araması](media/alerts-log/AlertsPreviewResourceSelectionLog.png)

   > [!NOTE]
   > 
   > Uyarı listeleri, yukarıdaki çizimde görüldüğü gibi Analytics sorgusunu, sinyal türü- **günlük (kaydedilmiş sorgu)** olarak içeri aktarabilir. Böylece kullanıcılar, analizlerinizi sorgulayabilir ve daha sonra uyarılarda kullanılmak üzere kaydedebilir. Kayıtlı sorguları kullanma hakkında daha fazla bilgi için bkz. [Azure izleyici 'de günlük sorgusu kullanma](../log-query/log-query-overview.md) ve [Application Insights Analytics 'te paylaşılan sorgu](../app/app-insights-overview.md).

1. Seçildiğinde, **arama sorgusu** alanında uyarı sorgusu oluşturun. Sorgu söz dizimi yanlışsa, alan kırmızı renkte görüntülenir. 

1. Sorgu söz dizimi doğruysa sorgunun geçmiş verileri, son altı saatten geçen haftaya kadar olan zaman penceresini ince ayar seçeneği ile bir grafik olarak görüntülenir.

    ![Uyarı kuralını Yapılandır](media/alerts-log/AlertsPreviewAlertLog.png)

   Geçmiş veri görselleştirmesi yalnızca sorgu sonuçlarının zaman ayrıntıları varsa gösterilir. Sorgunuz özetlenmiş veriler veya belirli sütun değerleriyle sonuçlanırsa, ekranda tek bir çizim gösterilir.
  
   Application Insights veya [log ANALYTICS API](/rest/api/monitor/scheduledqueryrules)kullanan ölçüm ölçümleri Için, **toplama** seçeneğini kullanarak verileri hangi belirli değişkenin gruplandırarak belirtebilirsiniz; burada gösterildiği gibi: 
  
   ![toplama açık seçeneği](media/alerts-log/aggregate-on.png)



1. Ardından, **Uyarı mantığı** koşulunu, toplamayı ve eşiği seçin. 

1. Belirtilen koşulun değerlendilecek süreyi, **period** seçeneğini kullanarak seçin. 

1. Uyarının **Sıklık**olarak ne sıklıkta çalışacağını seçin. 

    **Günlük uyarıları** şunları temel alabilir:
    - [Kayıt sayısı](../../azure-monitor/platform/alerts-unified-log.md#number-of-results-alert-rules): sorgu tarafından döndürülen kayıt sayısı, belirtilen değerden büyükse veya bundan küçükse bir uyarı oluşturulur.
    - [Ölçüm ölçümü](../../azure-monitor/platform/alerts-unified-log.md#metric-measurement-alert-rules): sonuçlardaki her bir *toplama değeri* , belirtilen eşik değerini aşarsa ve seçilen değere *göre gruplandırılmışsa* bir uyarı oluşturulur. Bir uyarı için ihlal sayısı, eşiğin seçilen dönemde kaç kez aşıldığını sayısıdır. Sonuçlar kümesi veya ardışık ihlallerin her türlü birleşimi için, her türlü ihlal kombinasyonu için, binlerin ardışık örneklerde oluşması gerekir.


1. **Bitti**’ye tıklayın. 

1. Uyarı **kuralı adı** alanında, uyarı ve **önem derecesi** değerinin ayrıntılarını açıklayan bir **açıklamanın** yanı sıra, belirtilen seçeneklerden bir ad tanımlayın. Bu ayrıntılar, Azure Izleyici tarafından yapılan tüm uyarı e-postalarında, bildirimlerde veya Gönderimleriyle yeniden kullanılır. Ayrıca, oluşturma **sırasında kuralı etkinleştir**' e tıklayarak oluşturma sırasında uyarı kuralını hemen etkinleştirmeyi seçebilirsiniz.

1. Uyarıları bir süre **gizlemek** istiyorsanız seçin.  Uyarı kuralı için gizlemeyi açtığınızda, kuralın eylemleri, yeni bir uyarı oluşturulduktan sonra tanımlanan süre için devre dışı bırakılır. Kural hala çalışır ve ölçütler sağlandığında uyarı kayıtları oluşturur. Bu ayar, yinelenen eylemleri çalıştırmadan sorunu düzeltmenize zaman sağlar.

   ![Günlük uyarıları uyarılarını gösterme](media/alerts-log/AlertsPreviewSuppress.png)

    > [!TIP]
    > Bildirimlerin çakışma olmadan durdurulduğundan emin olmak için uyarı sıklığından daha büyük bir gizleme uyarı değeri belirtin

1. Üçüncü ve son adım olarak, uyarı koşulu karşılandığında uyarı kuralının bir veya daha fazla **Eylem grubunu** tetiklemeniz gerekip gerekmediğini belirtin. Var olan herhangi bir eylem grubunu seçebilir veya yeni bir tane oluşturabilirsiniz. Eylem gruplarıyla, e-posta gönder, SMS gönderme, Web kancaları çağırma, Azure runbook 'Ları kullanma, ıTSM aracınızı yükleme ve daha fazlası gibi çeşitli eylemler gerçekleştirebilirsiniz. [Eylem grupları](action-groups.md)hakkında daha fazla bilgi edinin.

    > [!NOTE]
    > Gerçekleştirilebilecek eylemlerdeki sınırlamalar için [Azure abonelik hizmeti sınırlarına](../../azure-resource-manager/management/azure-subscription-service-limits.md) bakın.  

    Varsayılan eylemleri geçersiz kılmak için bazı ek işlevler kullanılabilir:

    - **E-posta bildirimi**: eylem grubu aracılığıyla gönderilen e-postadaki *e-posta konusunu* geçersiz kılar. Postanın gövdesini değiştiremezsiniz ve bu alan e-posta adresi için **değil** .
    - **Özel JSON yükünü dahil et**: eylem grubunun bir Web kancası türü içerdiğini varsayarak, eylem grupları tarafından kullanılan Web kancası JSON 'ı geçersiz kılar. Web kancası biçimleri hakkında daha fazla bilgi için bkz. [günlük uyarıları için Web kancası eylemi](../../azure-monitor/platform/alerts-log-webhook.md). Örnek JSON verilerini kullanarak Biçimlendirmeyi denetlemek için Web kancasını görüntüle seçeneği sağlanır.

        ![Günlük uyarıları için eylem geçersiz kılmaları](media/alerts-log/AlertsPreviewOverrideLog.png)


1. Tüm alanlar geçerliyse ve yeşil onay işareti varsa **Uyarı kuralı oluştur** düğmesine tıklanmış ve Azure Izleyici-Uyarılar ' ta bir uyarı oluşturulur. Tüm uyarılar uyarılar panosundan görüntülenebilir.

     ![Kural oluşturma](media/alerts-log/AlertsPreviewCreate.png)

     Birkaç dakika içinde, uyarı etkin ve daha önce açıklandığı gibi tetikler.

Ayrıca kullanıcılar, [Log Analytics](../log-query/log-query-overview.md) 'te analiz sorgusunu sonlandırabilir ve sonra ' uyarı ayarla ' düğmesine bir uyarı oluşturmak için bunu gönderebilir ve yukarıdaki öğreticide adım 6 ' dan sonraki yönergeleri izleyin.

 ![Log Analytics-uyarı ayarla](media/alerts-log/AlertsAnalyticsCreate.png)

### <a name="view--manage-log-alerts-in-azure-portal"></a>Azure portal 'da günlük uyarılarını görüntüleme & yönetme

1. [Portalda](https://portal.azure.com/) **izleyici** ' yi seçin ve izleyici bölümünde **Uyarılar**' ı seçin.

1. **Uyarılar panosu** görüntülenir; tüm Azure uyarıları (günlük uyarıları dahil) tekil bir panoda görüntülenir; günlük uyarı kuralınız tetiklendiğinde her örneğini dahil etme. Daha fazla bilgi için bkz. [uyarı yönetimi](https://aka.ms/managealertinstances).
    > [!NOTE]
    > Günlük uyarı kuralları, kullanıcılar tarafından belirtilen özel sorgu tabanlı mantığın ve bu nedenle çözümlenmiş bir durum olmadan oluşur. Günlük uyarı kuralında belirtilen koşulların her karşılanması nedeniyle, bu durum tetiklenir.

1. Kural yönetimi bölümüne gitmek için üst taraftaki çubukta **Kuralları Yönet** düğmesini seçin-tüm uyarı kurallarının listelendiği yer. devre dışı bırakılmış uyarılar dahil.
    ![Uyarı kurallarını yönet](media/alerts-log/manage-alert-rules.png)

## <a name="managing-log-alerts-using-azure-resource-template"></a>Azure kaynak şablonu kullanarak günlük uyarılarını yönetme

Azure Izleyici 'deki günlük uyarıları kaynak türüyle ilişkilendirilir `Microsoft.Insights/scheduledQueryRules/` . Bu kaynak türü hakkında daha fazla bilgi için bkz. [Azure izleyici-zamanlanmış sorgu KURALLARı API başvurusu](/rest/api/monitor/scheduledqueryrules/). Application Insights veya Log Analytics için günlük uyarıları, [Zamanlanmış sorgu kuralları API 'si](/rest/api/monitor/scheduledqueryrules/)kullanılarak oluşturulabilir.

> [!NOTE]
> Log Analytics için günlük uyarıları, eski [Log Analytics uyarı API 'si](api-alerts.md) ve [Log Analytics kayıtlı aramaların ve uyarıların](../insights/solutions.md) eski şablonları kullanılarak da yönetilebilir. Burada ayrıntılı olarak açıklanan yeni ScheduledQueryRules API 'sini kullanma hakkında daha fazla bilgi için, bkz. [Log Analytics uyarılar için yenı API 'ye geçme](alerts-log-api-switch.md).


### <a name="sample-log-alert-creation-using-azure-resource-template"></a>Azure kaynak şablonu kullanarak örnek günlük uyarısı oluşturma

Aşağıda, örnek verileri değişken olarak ayarlanmış şekilde, [sonuç türü günlük uyarısı](alerts-unified-log.md#number-of-results-alert-rules)için standart günlük arama sorgusu kullanılarak [zamanlanan sorgu kuralları oluşturma](/rest/api/monitor/scheduledqueryrules/createorupdate) tabanlı kaynak şablonunun yapısı verilmiştir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
    },
    "variables": {
        "alertLocation": "southcentralus",
        "alertName": "samplelogalert",
        "alertDescription": "Sample log search alert",
        "alertStatus": "true",
        "alertSource":{
            "Query":"requests",
            "SourceId": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/myRG/providers/microsoft.insights/components/sampleAIapplication",
            "Type":"ResultCount"
        },
        "alertSchedule":{
            "Frequency": 15,
            "Time": 60
        },
        "alertActions":{
            "SeverityLevel": "4"
        },
        "alertTrigger":{
            "Operator":"GreaterThan",
            "Threshold":"1"
        },
        "actionGrp":{
            "ActionGroup": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/myRG/providers/microsoft.insights/actiongroups/sampleAG",
            "Subject": "Customized Email Header",
            "Webhook": "{ \"alertname\":\"#alertrulename\", \"IncludeSearchResults\":true }"
        }
    },
    "resources":[ {
        "name":"[variables('alertName')]",
        "type":"Microsoft.Insights/scheduledQueryRules",
        "apiVersion": "2018-04-16",
        "location": "[variables('alertLocation')]",
        "properties":{
            "description": "[variables('alertDescription')]",
            "enabled": "[variables('alertStatus')]",
            "source": {
                "query": "[variables('alertSource').Query]",
                "dataSourceId": "[variables('alertSource').SourceId]",
                "queryType":"[variables('alertSource').Type]"
            },
            "schedule":{
                "frequencyInMinutes": "[variables('alertSchedule').Frequency]",
                "timeWindowInMinutes": "[variables('alertSchedule').Time]"
            },
            "action":{
                "odata.type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.Microsoft.AppInsights.Nexus.DataContracts.Resources.ScheduledQueryRules.AlertingAction",
                "severity":"[variables('alertActions').SeverityLevel]",
                "aznsAction":{
                    "actionGroup":"[array(variables('actionGrp').ActionGroup)]",
                    "emailSubject":"[variables('actionGrp').Subject]",
                    "customWebhookPayload":"[variables('actionGrp').Webhook]"
                },
                "trigger":{
                    "thresholdOperator":"[variables('alertTrigger').Operator]",
                    "threshold":"[variables('alertTrigger').Threshold]"
                }
            }
        }
    } ]
}

```

Yukarıdaki örnek JSON (deyin), Bu izlenecek yol için sampleScheduledQueryRule.jsolarak kaydedilebilir ve [Azure portal Azure Resource Manager](../../azure-resource-manager/templates/deploy-portal.md#deploy-resources-from-custom-template)kullanılarak dağıtılabilir.


### <a name="log-alert-with-cross-resource-query-using-azure-resource-template"></a>Azure kaynak şablonu kullanarak çapraz kaynak sorgusuyla günlük uyarısı

Aşağıda, değişken olarak ayarlanan örnek verilerle birlikte, [Ölçüm Ölçüm türü günlük uyarısı](../../azure-monitor/platform/alerts-unified-log.md#metric-measurement-alert-rules)' nın [çapraz kaynak günlük arama sorgusu](../../azure-monitor/log-query/cross-workspace-query.md) kullanılarak [zamanlanan sorgu kuralları oluşturma](/rest/api/monitor/scheduledqueryrules/createorupdate) tabanlı kaynak şablonunun yapısı verilmiştir.

```json

{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
    },
    "variables": {
        "alertLocation": "Region Name for your Application Insights App or Log Analytics Workspace",
        "alertName": "sample log alert",
        "alertDescr": "Sample log search alert",
        "alertStatus": "true",
        "alertSource":{
            "Query":"union workspace(\"servicews\").Update, app('serviceapp').requests | summarize AggregatedValue = count() by bin(TimeGenerated,1h), Classification",
            "Resource1": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/contosoRG/providers/microsoft.OperationalInsights/workspaces/servicews",
            "Resource2": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/contosoRG/providers/microsoft.insights/components/serviceapp",
            "SourceId": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/contosoRG/providers/microsoft.OperationalInsights/workspaces/servicews",
            "Type":"ResultCount"
        },
        "alertSchedule":{
            "Frequency": 15,
            "Time": 60
        },
        "alertActions":{
            "SeverityLevel": "4",
            "SuppressTimeinMin": 20
        },
        "alertTrigger":{
            "Operator":"GreaterThan",
            "Threshold":"1"
        },
        "metricMeasurement": {
            "thresholdOperator": "Equal",
            "threshold": "1",
            "metricTriggerType": "Consecutive",
            "metricColumn": "Classification"
        },
        "actionGrp":{
            "ActionGroup": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/contosoRG/providers/microsoft.insights/actiongroups/sampleAG",
            "Subject": "Customized Email Header",
            "Webhook": "{ \"alertname\":\"#alertrulename\", \"IncludeSearchResults\":true }"
        }
    },
    "resources":[ {
        "name":"[variables('alertName')]",
        "type":"Microsoft.Insights/scheduledQueryRules",
        "apiVersion": "2018-04-16",
        "location": "[variables('alertLocation')]",
        "properties":{
            "description": "[variables('alertDescr')]",
            "enabled": "[variables('alertStatus')]",
            "source": {
                "query": "[variables('alertSource').Query]",
                "authorizedResources": "[concat(array(variables('alertSource').Resource1), array(variables('alertSource').Resource2))]",
                "dataSourceId": "[variables('alertSource').SourceId]",
                "queryType":"[variables('alertSource').Type]"
            },
            "schedule":{
                "frequencyInMinutes": "[variables('alertSchedule').Frequency]",
                "timeWindowInMinutes": "[variables('alertSchedule').Time]"
            },
            "action":{
                "odata.type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.Microsoft.AppInsights.Nexus.DataContracts.Resources.ScheduledQueryRules.AlertingAction",
                "severity":"[variables('alertActions').SeverityLevel]",
                "throttlingInMin": "[variables('alertActions').SuppressTimeinMin]",
                "aznsAction":{
                    "actionGroup": "[array(variables('actionGrp').ActionGroup)]",
                    "emailSubject":"[variables('actionGrp').Subject]",
                    "customWebhookPayload":"[variables('actionGrp').Webhook]"
                },
                "trigger":{
                    "thresholdOperator":"[variables('alertTrigger').Operator]",
                    "threshold":"[variables('alertTrigger').Threshold]",
                    "metricTrigger":{
                        "thresholdOperator": "[variables('metricMeasurement').thresholdOperator]",
                        "threshold": "[variables('metricMeasurement').threshold]",
                        "metricColumn": "[variables('metricMeasurement').metricColumn]",
                        "metricTriggerType": "[variables('metricMeasurement').metricTriggerType]"
                    }
                }
            }
        }
    } ]
}

```

> [!IMPORTANT]
> Günlük uyarısında çapraz kaynak sorgusu kullanılırken, [Authorizedresources](/rest/api/monitor/scheduledqueryrules/createorupdate#source) kullanımı zorunludur ve kullanıcının belirtilen kaynak listesine erişimi olması gerekir

Yukarıdaki örnek JSON (deyin), Bu izlenecek yol için sampleScheduledQueryRule.jsolarak kaydedilebilir ve [Azure portal Azure Resource Manager](../../azure-resource-manager/templates/deploy-portal.md#deploy-resources-from-custom-template)kullanılarak dağıtılabilir.

## <a name="managing-log-alerts-using-powershell"></a>PowerShell kullanarak günlük uyarılarını yönetme

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure Izleyici- [Zamanlanmış sorgu kuralları API 'si](/rest/api/monitor/scheduledqueryrules/) bir REST API ve Azure Resource Manager REST API ile tamamen uyumludur. Ve aşağıda listelenen PowerShell cmdlet 'leri, [Zamanlanmış sorgu KURALLARı API](/rest/api/monitor/scheduledqueryrules/)'sini kullanabilir.

- New [-AzScheduledQueryRule](/powershell/module/az.monitor/new-azscheduledqueryrule) : PowerShell cmdlet 'i yeni bir günlük uyarı kuralı oluşturmak için.
- [Set-AzScheduledQueryRule](/powershell/module/az.monitor/set-azscheduledqueryrule) : PowerShell cmdlet 'i var olan bir günlük uyarısı kuralını güncelleştirmek için.
- [New-AzScheduledQueryRuleSource](/powershell/module/az.monitor/new-azscheduledqueryrulesource) : bir günlük uyarısı için kaynak parametrelerini belirten nesne oluşturmak veya güncelleştirmek için PowerShell cmdlet 'i. [New-azscheduledqueryrule](/powershell/module/az.monitor/new-azscheduledqueryrule) ve [set-azscheduledqueryrule](/powershell/module/az.monitor/set-azscheduledqueryrule) cmdlet 'i tarafından giriş olarak kullanılır.
- [New-AzScheduledQueryRuleSchedule](/powershell/module/az.monitor/new-azscheduledqueryruleschedule): bir günlük uyarısı için zamanlama parametrelerini belirten nesne oluşturmak veya güncelleştirmek için PowerShell cmdlet 'i. [New-azscheduledqueryrule](/powershell/module/az.monitor/new-azscheduledqueryrule) ve [set-azscheduledqueryrule](/powershell/module/az.monitor/set-azscheduledqueryrule) cmdlet 'i tarafından giriş olarak kullanılır.
- [New-AzScheduledQueryRuleAlertingAction](/powershell/module/az.monitor/new-azscheduledqueryrulealertingaction) : bir günlük uyarısı için eylem parametreleri belirten nesne oluşturmak veya güncelleştirmek için PowerShell cmdlet 'i. [New-azscheduledqueryrule](/powershell/module/az.monitor/new-azscheduledqueryrule) ve [set-azscheduledqueryrule](/powershell/module/az.monitor/set-azscheduledqueryrule) cmdlet 'i tarafından giriş olarak kullanılır.
- [New-AzScheduledQueryRuleAznsActionGroup](/powershell/module/az.monitor/new-azscheduledqueryruleaznsactiongroup) : bir günlük uyarısı için eylem grupları parametrelerini belirten nesne oluşturmak veya güncelleştirmek için PowerShell cmdlet 'i. [New-AzScheduledQueryRuleAlertingAction](/powershell/module/az.monitor/new-azscheduledqueryrulealertingaction) cmdlet 'i tarafından giriş olarak kullanılır.
- [New-AzScheduledQueryRuleTriggerCondition](/powershell/module/az.monitor/new-azscheduledqueryruletriggercondition) : günlük uyarısı için tetikleyici koşulu parametrelerini belirten nesne oluşturmak veya güncelleştirmek için PowerShell cmdlet 'i. [New-AzScheduledQueryRuleAlertingAction](/powershell/module/az.monitor/new-azscheduledqueryrulealertingaction) cmdlet 'i tarafından giriş olarak kullanılır.
- [New-AzScheduledQueryRuleLogMetricTrigger](/powershell/module/az.monitor/new-azscheduledqueryrulelogmetrictrigger) : ölçüm [Ölçüm türü günlük uyarısı](../../azure-monitor/platform/alerts-unified-log.md#metric-measurement-alert-rules)için ölçüm tetikleyicisi koşul parametrelerini belirten nesne oluşturmak veya güncelleştirmek için PowerShell cmdlet 'i. [New-AzScheduledQueryRuleTriggerCondition](/powershell/module/az.monitor/new-azscheduledqueryruletriggercondition) cmdlet 'i tarafından giriş olarak kullanılır.
- [Get-AzScheduledQueryRule](/powershell/module/az.monitor/get-azscheduledqueryrule) : var olan günlük uyarısı kurallarını veya belirli bir günlük uyarısı kuralını listelemek için PowerShell cmdlet 'i
- [Update-AzScheduledQueryRule](/powershell/module/az.monitor/update-azscheduledqueryrule) : PowerShell cmdlet 'i, günlük uyarı kuralını etkinleştirmek veya devre dışı bırakmak için
- [Remove-AzScheduledQueryRule](/powershell/module/az.monitor/remove-azscheduledqueryrule): PowerShell cmdlet 'i var olan bir günlük uyarı kuralını siler

> [!NOTE]
> ScheduledQueryRules PowerShell cmdlet 'leri yalnızca oluşturulan cmdlet 'lerin kendisini yönetebilir veya Azure Izleyici ile [Zamanlanmış sorgu kuralları API 'sini](/rest/api/monitor/scheduledqueryrules/)kullanabilir. Eski [Log Analytics uyarı API 'si](api-alerts.md) kullanılarak oluşturulan günlük uyarı kuralları ve [Log Analytics kayıtlı aramaların ve uyarıların](../insights/solutions.md) eski şablonları, yalnızca Kullanıcı [Log Analytics uyarıları için API tercihine](alerts-log-api-switch.md)geçtikten sonra scheduledqueryrules PowerShell cmdlet 'leri kullanılarak yönetilebilir.

Sonraki adımda, scheduledQueryRules PowerShell cmdlet 'lerini kullanarak örnek günlük uyarısı kuralı oluşturma adımları gösterilmektedir.

```powershell
$source = New-AzScheduledQueryRuleSource -Query 'Heartbeat | summarize AggregatedValue = count() by bin(TimeGenerated, 5m), _ResourceId' -DataSourceId "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/contosoRG/providers/microsoft.OperationalInsights/workspaces/servicews"

$schedule = New-AzScheduledQueryRuleSchedule -FrequencyInMinutes 15 -TimeWindowInMinutes 30

$metricTrigger = New-AzScheduledQueryRuleLogMetricTrigger -ThresholdOperator "GreaterThan" -Threshold 2 -MetricTriggerType "Consecutive" -MetricColumn "_ResourceId"

$triggerCondition = New-AzScheduledQueryRuleTriggerCondition -ThresholdOperator "LessThan" -Threshold 5 -MetricTrigger $metricTrigger

$aznsActionGroup = New-AzScheduledQueryRuleAznsActionGroup -ActionGroup "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/contosoRG/providers/microsoft.insights/actiongroups/sampleAG" -EmailSubject "Custom email subject" -CustomWebhookPayload "{ `"alert`":`"#alertrulename`", `"IncludeSearchResults`":true }"

$alertingAction = New-AzScheduledQueryRuleAlertingAction -AznsAction $aznsActionGroup -Severity "3" -Trigger $triggerCondition

New-AzScheduledQueryRule -ResourceGroupName "contosoRG" -Location "Region Name for your Application Insights App or Log Analytics Workspace" -Action $alertingAction -Enabled $true -Description "Alert description" -Schedule $schedule -Source $source -Name "Alert Name"
```

## <a name="managing-log-alerts-using-cli-or-api"></a>CLı veya API kullanarak günlük uyarılarını yönetme

Azure Izleyici- [Zamanlanmış sorgu kuralları API 'si](/rest/api/monitor/scheduledqueryrules/) bir REST API ve Azure Resource Manager REST API ile tamamen uyumludur. Bu nedenle, Azure CLı için Kaynak Yöneticisi komutları kullanılarak PowerShell aracılığıyla kullanılabilir.


> [!NOTE]
> Log Analytics için günlük uyarıları, eski [Log Analytics uyarı API 'si](api-alerts.md) ve [Log Analytics kayıtlı aramaların ve uyarıların](../insights/solutions.md) eski şablonları kullanılarak da yönetilebilir. Burada ayrıntılı olarak açıklanan yeni ScheduledQueryRules API 'sini kullanma hakkında daha fazla bilgi için, bkz. [Log Analytics uyarılar için yenı API 'ye geçme](alerts-log-api-switch.md).

Günlük uyarıları Şu anda adanmış CLı komutlarına sahip değil; aşağıda gösterildiği gibi, kaynak şablonu bölümünde daha önce (sampleScheduledQueryRule.jsaçık) gösterilen örnek kaynak şablonu için Azure Resource Manager CLı komutu aracılığıyla kullanılabilir:

```azurecli
az group deployment create --resource-group contosoRG --template-file sampleScheduledQueryRule.json
```

Başarılı bir işlem olduğunda, 201 yeni uyarı kuralı oluşturma durumuna döndürülecek veya mevcut bir uyarı kuralı değiştirilirse 200 döndürülür.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure uyarılarında günlük uyarıları](../../azure-monitor/platform/alerts-unified-log.md) hakkında bilgi edinin
* [Günlük uyarıları Için Web kancası eylemlerini](../../azure-monitor/platform/alerts-log-webhook.md) anlama
* [Application Insights](../log-query/log-query-overview.md) hakkında daha fazla bilgi edinin
* [Günlük sorguları](../log-query/log-query-overview.md)hakkında daha fazla bilgi edinin.
