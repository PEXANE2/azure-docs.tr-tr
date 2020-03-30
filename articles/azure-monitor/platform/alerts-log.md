---
title: Azure Monitörünü Kullanarak günlük uyarıları oluşturma, görüntüleme ve yönetme | Microsoft Dokümanlar
description: Azure'da günlük uyarı kurallarını yazar, görüntüleyebilir ve yönetin için Azure Monitörünü kullanın.
author: yanivlavi
ms.author: yalavi
ms.topic: conceptual
ms.date: 07/29/2019
ms.subservice: alerts
ms.openlocfilehash: 96b1bd86576f8cf34428eb60e2d3f476312311c1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79249431"
---
# <a name="create-view-and-manage-log-alerts-using-azure-monitor"></a>Azure Monitor'u kullanarak günlük uyarıları oluşturma, görüntüleme ve yönetme

## <a name="overview"></a>Genel Bakış
Bu makalede, Azure portalı içindeki uyarılar arabirimini kullanarak günlük uyarılarının nasıl ayarlır? Uyarı kuralının tanımı üç bölümden oluşur:
- Hedef: İzlenecek belirli Azure kaynağı
- Kriterler: Signal'de görüldüğünde eylemi tetiklemesi gereken özel durum veya mantık
- Eylem: Bir bildirimin alıcısına gönderilen özel arama - e-posta, SMS, webhook vb.

Günlük [Analizi çalışma alanında](../learn/tutorial-viewdata.md) veya Uygulama [Öngörülerinde](../app/analytics.md)sinyalin günlük sorgusu olduğu uyarıları açıklamak için **Günlük Uyarıları** terimi. [Günlük uyarılarının](alerts-unified-log.md)işlevselliği, terminolojisi ve türleri hakkında daha fazla bilgi edinin - Genel Bakış .

> [!NOTE]
> [Log Analytics çalışma alanından](../../azure-monitor/learn/tutorial-viewdata.md) popüler günlük verileri artık Azure Monitor'daki metrik platformda da kullanılabilir. Ayrıntılar için görünüm, [Günlükler için Metrik Uyarı](alerts-metric-logs.md)

## <a name="managing-log-alerts-from-the-azure-portal"></a>Azure portalından günlük uyarıları yönetme

Ayrıntılı sonraki Azure portal arabirimini kullanarak günlük uyarıları kullanarak adım adım kılavuzdur.

### <a name="create-a-log-alert-rule-with-the-azure-portal"></a>Azure portalıile günlük uyarı kuralı oluşturma

1. [Portalda,](https://portal.azure.com/) **Monitör'ü** seçin ve MONITOR bölümünün altında **Uyarılar'ı**seçin.

    ![İzleme](media/alerts-log/AlertsPreviewMenu.png)

1. Azure'da yeni bir uyarı oluşturmak için **Yeni Uyarı Kuralı** düğmesini seçin.

    ![Uyarı Ekle](media/alerts-log/AlertsPreviewOption.png)

1. Uyarı Oluştur bölümü üç bölümden oluşan gösterilir: *Uyarı koşulunu tanımlayın,* *uyarı ayrıntılarını tanımla*ve *eylem grubunu tanımla.*

    ![Kural oluşturma](media/alerts-log/AlertsPreviewAdd.png)

1. **Kaynak Seç** bağlantısını kullanarak ve bir kaynak seçerek hedefi belirterek uyarı koşulunu tanımlayın. _Abonelik,_ _Kaynak Türü_ve gerekli _Kaynağı_seçerek filtre uygulayın.

   > [!NOTE]
   > Günlük uyarısı oluşturmak için, devam etmeden önce günlük **sinyalinin** seçili kaynak için kullanılabilir olduğunu doğrulayın.
   >  ![Kaynak seçin](media/alerts-log/Alert-SelectResourceLog.png)

1. *Günlük Uyarıları*: **Kaynak Türü'n** *Log Analytics* veya *Application Insights* gibi bir analiz kaynağı olduğundan emin olun ve **Log**olarak sinyal türü , uygun **kaynak** seçildikten sonra *Bitti'yi*tıklatın. Sonraki kaynak için kullanılabilir sinyal seçenekleri listesini görüntülemek için **ölçütekle** düğmesini kullanın ve sinyal listesinden *Log Analytics* veya *Application Insights*gibi seçilen günlük monitör hizmeti için **özel günlük arama** seçeneği .

   ![Kaynak seçin - özel günlük arama](media/alerts-log/AlertsPreviewResourceSelectionLog.png)

   > [!NOTE]
   > 
   > Uyarı listeleri, yukarıdaki resimde görüldüğü gibi, analiz sorgusu - **Log (Kayıtlı Sorgu)** olarak içe aktarılabilir. Böylece kullanıcılar sorgunuzu Analytics'te mükemmelleştirebilir ve bunları gelecekte uyarılarda kullanılmak üzere kaydedebilir - Azure Monitor'da günlük sorgusu nun [kullanımında](../log-query/log-query-overview.md) kullanılabilir kaydetme sorgusu nun kullanımı veya [uygulama öngörüleri analizinde paylaşılan sorgu](../app/app-insights-overview.md)nun kullanılması yla ilgili daha fazla ayrıntı .

1. *Günlük Uyarıları*: Seçildikten sonra, **arama sorgusu** alanında uyarı sorgusu belirtilebilir; sorgu sözdizimi yanlışsa alan RED'de hata görüntüler. Sorgu sözdizimi doğruysa - Başvuru için belirtilen sorgunun tarihi verileri, son altı saatten geçen haftaya kadar zaman penceresinde değişiklik yapmak için seçeneği olan bir grafik olarak gösterilir.

    ![Uyarı kuralını yapılandırma](media/alerts-log/AlertsPreviewAlertLog.png)

   > [!NOTE]
   > 
   > Geçmiş veri görselleştirmesi yalnızca sorgu sonuçlarının zaman ayrıntıları varsa gösterilebilir. Sorgunuz özetlenmiş verilerle veya belirli sütun değerlerinde sonuçlanırsa, aynı tekil çizim olarak gösterilir.
   > Uygulama Öngörülerini kullanan veya [yeni API'ye geçen](alerts-log-api-switch.md)Log Alert'lerin Metrik Ölçüm türü için, **Toplu** seçeneğini kullanarak verileri gruplandıracak belirli bir değişken belirtebilirsiniz; aşağıda gösterildiği gibi:
   > 
   > ![seçeneğinde toplam](media/alerts-log/aggregate-on.png)

1. *Günlük Uyarıları*: Görselleştirme yerinde olduğu **için, Durum,** Toplama ve son olarak Eşik seçeneklerinden Uyarı Mantığı seçilebilir. Son olarak mantık, belirtilen koşul için değerlendirmek için zaman, **Dönem** seçeneğini kullanarak belirtin. **Sıklık**seçerek Uyarı'nın ne sıklıkta çalışması gerektiği ile birlikte. **Günlük Uyarıları** şu temele dayanıyor olabilir:
    - [Kayıt Sayısı](../../azure-monitor/platform/alerts-unified-log.md#number-of-results-alert-rules): Sorgu tarafından döndürülen kayıt sayısı sağlanan değerden büyük veya daha azsa bir uyarı oluşturulur.
    - [Metrik Ölçüm](../../azure-monitor/platform/alerts-unified-log.md#metric-measurement-alert-rules): Sonuçlardaki her *toplam değer* sağlanan eşik değeri aşıyorsa ve seçilen değere *göre gruplandırılırsa* bir uyarı oluşturulur. Bir uyarı için ihlal sayısı, seçilen zaman diliminde eşiğin kaç kez aşıldığıdır. Sonuçlar kümesindeki ihlallerin veya Ardışık ihlallerin birleşimi için, ihlallerin ardışık örneklerde oluşmasını gerektirecek şekilde Toplam ihlalleri belirtebilirsiniz.


1. İkinci adım olarak, **Uyarı kuralı adı** alanında uyarınızın bir adını ve sağlanan seçeneklerden uyarı ve Önem **Derecesi** değerinin ayrıntılarını açıklayan bir **Açıklama** tanımlayın. Bu ayrıntılar, Azure Monitor tarafından yapılan tüm uyarı e-postalarında, bildirimlerinde veya anında netmede yeniden kullanılır. Ayrıca, kullanıcı oluşturma seçeneği üzerine **etkinleştir mekuralını** uygun şekilde değiştirerek oluşturma yla ilgili uyarı kuralını hemen etkinleştirmeyi seçebilir.

    Yalnızca Günlük Uyarıları için, Uyarı ayrıntılarında bazı ek **işlevler** mevcuttur:

    - **Uyarıları Bastırma**: Uyarı kuralı için bastırmayı açtığınızda, kural için eylemler yeni bir uyarı oluşturduktan sonra belirli bir süre için devre dışı bırakılır. Kural hala çalışıyor ve ölçütlerin karşılanması koşuluyla uyarı kayıtları oluşturur. Yinelenen eylemleri çalıştırmadan sorunu düzeltmeniz için size zaman sağlar.

        ![Günlük Uyarıları için Bastırma Uyarıları](media/alerts-log/AlertsPreviewSuppress.png)

        > [!TIP]
        > Bildirimlerin çakışmadan durdurulduğundan emin olmak için uyarı sıklığından daha büyük bir bastırma uyarı değeri belirtin

1. Üçüncü ve son adım olarak, uyarı koşulu karşılandığında uyarı kuralı için herhangi bir **Eylem Grubu'nun** tetiklenmesi gerekip gerekip gerekmediğinde belirtin. Uyarı yla varolan herhangi bir Eylem Grubu'nı seçebilir veya yeni bir Eylem Grubu oluşturabilirsiniz. Seçili Eylem Grubuna göre, uyarı tetiklendiğinde Azure şunları yapacak: e-posta(lar gönderin), SMS(lar gönderin), Webhook(lar'ı arayın), Azure Runbook'larını kullanarak düzeltin, ITSM aracınıza itin, vb. [Eylem Grupları](action-groups.md)hakkında daha fazla bilgi edinin.

    > [!NOTE]
    > Azure eylem grupları aracılığıyla günlük uyarıları için tetiklenen Runbook yüklerinde sınırlamalar için [Azure abonelik hizmeti sınırlarına](../../azure-resource-manager/management/azure-subscription-service-limits.md) bakın

    Günlük Uyarıları için varsayılan Eylemleri geçersiz kılmak için bazı ek **işlevler** kullanılabilir:

    - **E-posta Bildirimi**: Eylem Grubu aracılığıyla gönderilen *e-postadaki e-posta konusunu* geçersiz kılar; bu Eylem Grubunda bir veya daha fazla e-posta eylemi varsa. Postanın gövdesini değiştiremezsiniz ve bu alan e-posta adresi için **değildir.**
    - **Özel Json yükünü dahil et:** Eylem Grupları tarafından kullanılan webhook JSON'ı geçersiz kılar; bir veya daha fazla webhook eylemleri bu Eylem Grubu varsa. Kullanıcı, ilişkili Eylem Grubu'nda yapılandırılan tüm webhook'lar için kullanılacak JSON biçimini belirtebilir; webhook biçimleri hakkında daha fazla bilgi için, [Günlük Uyarıları için webhook eylem](../../azure-monitor/platform/alerts-log-webhook.md)bakın. Örnek JSON verilerini kullanarak biçimi denetlemek için Webhook'u görüntüle seçeneği sağlanır.

        ![Günlük Uyarıları için Eylem Geçersiz Kılar](media/alerts-log/AlertsPreviewOverrideLog.png)


1. Tüm alanlar geçerliyse ve yeşil onayla birlikte **uyarı kuralı oluştur** düğmesine tıklanabilir ve Azure Monitor - Alerts'te bir uyarı oluşturulur. Tüm uyarılar uyarılar Panosu'ndan görüntülenebilir.

     ![Kural Oluşturma](media/alerts-log/AlertsPreviewCreate.png)

     Birkaç dakika içinde, uyarı etkin dir ve daha önce açıklandığı gibi tetikler.

Kullanıcılar ayrıca [günlük analitiğinde](../log-query/portals.md) analiz sorgularını sonuçlandırabilir ve ardından yukarıdaki öğreticide 6.

 ![Günlük Analizi - Uyarı Ayarla](media/alerts-log/AlertsAnalyticsCreate.png)

### <a name="view--manage-log-alerts-in-azure-portal"></a>Azure portalında günlük uyarılarını görüntüleme & yönetme

1. [Portalda,](https://portal.azure.com/) **Monitör'ü** seçin ve MONITOR bölümünün altında **Uyarılar'ı**seçin.

1. **Uyarılar Panosu** görüntülenir ve tüm Azure Uyarıları (günlük uyarıları dahil) tekil bir panoda görüntülenir; günlük uyarı kuralınızın ne zaman ateşlediğinin her örneği de dahil olmak üzere. Daha fazla bilgi için [Uyarı Yönetimi'ne](https://aka.ms/managealertinstances)bakın.
    > [!NOTE]
    > Günlük uyarı kuralları, kullanıcılar tarafından sağlanan ve bu nedenle çözülmüş bir durum olmadan özel sorgu tabanlı mantık oluşur. Günlük uyarı kuralında belirtilen koşullar her karşılandığı nda, bu durum ateşlenir.

1. Oluşturulan tüm uyarı kurallarının listelendiği kural yönetimi bölümüne gitmek için üst çubuktaki **Kuralları Yönet** düğmesini seçin; devre dışı bırakılmış uyarılar da dahil olmak üzere.
    ![uyarı kurallarını yönetme](media/alerts-log/manage-alert-rules.png)

## <a name="managing-log-alerts-using-azure-resource-template"></a>Azure Kaynak Şablonu'nunu kullanarak günlük uyarılarını yönetme

Azure Monitor'daki günlük uyarıları kaynak `Microsoft.Insights/scheduledQueryRules/`türüyle ilişkilidir. Bu kaynak türü hakkında daha fazla bilgi için [Azure Monitor - Zamanlanmış Sorgu Kuralları API başvurusuna](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/)bakın. Uygulama Öngörüleri veya Günlük Analizi için günlük uyarıları, [Zamanlanmış Sorgu Kuralları API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/)kullanılarak oluşturulabilir.

> [!NOTE]
> Log Analytics için günlük uyarıları, eski [Log Analytics Alert API'sı](api-alerts.md) ve [Log Analytics kaydedilmiş arama ve uyarıların](../insights/solutions-resources-searches-alerts.md) eski şablonları kullanılarak da yönetilebilir. Varsayılan olarak burada ayrıntılı olarak ayrıntılı olarak yeni ScheduledQueryRules API'sini kullanma hakkında daha fazla bilgi [için, Log Analytics Uyarıları için yeni API'ye geçiş bölümüne](alerts-log-api-switch.md)bakın.


### <a name="sample-log-alert-creation-using-azure-resource-template"></a>Azure Kaynak Şablonu'nunu kullanarak Örnek Günlük uyarısı oluşturma

Aşağıda, örnek veri değişkenolarak ayarlanmış sonuç [türü günlük uyarısı sayısının](alerts-unified-log.md#number-of-results-alert-rules)standart günlük arama sorgusunu kullanarak [Zamanlanmış Sorgu Kuralları oluşturma](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/createorupdate) tabanlı kaynak şablonu yapısı verilmiştir.

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

Yukarıdaki örnek json bu yürüyüş amacıyla (diyelim) örnekScheduledQueryRule.json olarak kaydedilebilir ve [Azure portalında Azure Kaynak Yöneticisi](../../azure-resource-manager/templates/deploy-portal.md#deploy-resources-from-custom-template)kullanılarak dağıtılabilir.


### <a name="log-alert-with-cross-resource-query-using-azure-resource-template"></a>Azure Kaynak Şablonu'nunu kullanarak çapraz kaynak sorgusuyla uyarı yı günlüğe kaydetme

Aşağıda, örnek veri değişkenolarak ayarlanmış [metrik ölçüm türü günlük uyarısı](../../azure-monitor/platform/alerts-unified-log.md#metric-measurement-alert-rules)çapraz kaynak günlüğü arama [sorgusu](../../azure-monitor/log-query/cross-workspace-query.md) nu kullanarak [Zamanlanmış Sorgu Kuralları oluşturma](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/createorupdate) tabanlı kaynak şablonu yapısı verilmiştir.

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
> Günlük uyarısında kaynaklar arası sorgu kullanırken, [yetkili Kaynakların](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/createorupdate#source) kullanımı zorunludur ve kullanıcı belirtilen kaynaklar listesine erişebilmeli

Yukarıdaki örnek json bu yürüyüş amacıyla (diyelim) örnekScheduledQueryRule.json olarak kaydedilebilir ve [Azure portalında Azure Kaynak Yöneticisi](../../azure-resource-manager/templates/deploy-portal.md#deploy-resources-from-custom-template)kullanılarak dağıtılabilir.

## <a name="managing-log-alerts-using-powershell"></a>PowerShell kullanarak günlük uyarılarını yönetme

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure Monitor - [Zamanlanmış Sorgu Kuralları API'si](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/) bir REST API'sidir ve Azure Kaynak Yöneticisi REST API ile tam uyumludur. Ve Aşağıda listelenen PowerShell cmdlets [Zamanlanmış Sorgu Kuralları API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/)kaldıraç için kullanılabilir.

1. [New-AzScheduledQueryRule](https://docs.microsoft.com/powershell/module/az.monitor/new-azscheduledqueryrule) : Powershell cmdlet yeni bir günlük uyarı kuralı oluşturmak için.
1. [Set-AzScheduledQueryRule](https://docs.microsoft.com/powershell/module/az.monitor/set-azscheduledqueryrule) : Varolan bir günlük uyarı kuralını güncelleştirmek için powershell cmdlet.
1. [New-AzScheduledQueryRuleSource](https://docs.microsoft.com/powershell/module/az.monitor/new-azscheduledqueryrulesource) : Powershell cmdlet oluşturmak veya bir günlük uyarısı için kaynak parametreleri belirten nesne güncelleştirmek için. [New-AzScheduledQueryRule](https://docs.microsoft.com/powershell/module/az.monitor/new-azscheduledqueryrule) ve [Set-AzScheduledQueryRule](https://docs.microsoft.com/powershell/module/az.monitor/set-azscheduledqueryrule) cmdlet tarafından giriş olarak kullanılır.
1. [New-AzScheduledQueryRuleSchedule](https://docs.microsoft.com/powershell/module/az.monitor/New-AzScheduledQueryRuleSchedule): Günlük uyarısı için zamanlama parametrelerini belirten nesne oluşturmak veya güncelleştirmek için powershell cmdlet. [New-AzScheduledQueryRule](https://docs.microsoft.com/powershell/module/az.monitor/new-azscheduledqueryrule) ve [Set-AzScheduledQueryRule](https://docs.microsoft.com/powershell/module/az.monitor/set-azscheduledqueryrule) cmdlet tarafından giriş olarak kullanılır.
1. [New-AzScheduledQueryRuleAlertingAction](https://docs.microsoft.com/powershell/module/az.monitor/New-AzScheduledQueryRuleAlertingAction) : Powershell cmdlet oluşturmak veya bir günlük uyarısı için eylem parametreleri belirten nesne güncelleştirmek için. [New-AzScheduledQueryRule](https://docs.microsoft.com/powershell/module/az.monitor/new-azscheduledqueryrule) ve [Set-AzScheduledQueryRule](https://docs.microsoft.com/powershell/module/az.monitor/set-azscheduledqueryrule) cmdlet tarafından giriş olarak kullanılır.
1. [New-AzScheduledQueryRuleAznsActionGroup](https://docs.microsoft.com/powershell/module/az.monitor/new-azscheduledqueryruleaznsactiongroup) : Powershell cmdlet oluşturmak veya bir günlük uyarısı için eylem grupları parametreleri belirten nesne güncelleştirmek için. [New-AzScheduledQueryRuleAlertingAction](https://docs.microsoft.com/powershell/module/az.monitor/New-AzScheduledQueryRuleAlertingAction) cmdlet tarafından giriş olarak kullanılır.
1. [New-AzScheduledQueryRuleTriggerCondition](https://docs.microsoft.com/powershell/module/az.monitor/new-azscheduledqueryruletriggercondition) : Günlük uyarısı için tetikleyici durum parametrelerini belirten nesne oluşturmak veya güncelleştirmek için powershell cmdlet. [New-AzScheduledQueryRuleAlertingAction](https://docs.microsoft.com/powershell/module/az.monitor/New-AzScheduledQueryRuleAlertingAction) cmdlet tarafından giriş olarak kullanılır.
1. [New-AzScheduledQueryRuleLogMetricTrigger](https://docs.microsoft.com/powershell/module/az.monitor/new-azscheduledqueryrulelogmetrictrigger) : Powershell cmdlet oluşturmak veya metrik ölçüm [türü günlük uyarısı](../../azure-monitor/platform/alerts-unified-log.md#metric-measurement-alert-rules)için metrik tetik durum parametreleri belirten nesne güncellemek. [New-AzScheduledQueryRuleTriggerCondition](https://docs.microsoft.com/powershell/module/az.monitor/new-azscheduledqueryruletriggercondition) cmdlet tarafından giriş olarak kullanılır.
1. [Get-AzScheduledQueryRule](https://docs.microsoft.com/powershell/module/az.monitor/get-azscheduledqueryrule) : Powershell cmdlet varolan günlük uyarı kuralları veya belirli bir günlük uyarı kuralı listelemek için
1. [Update-AzScheduledQueryRule](https://docs.microsoft.com/powershell/module/az.monitor/update-azscheduledqueryrule) : Powershell cmdlet etkinleştirmek veya günlük uyarı kuralı devre dışı bırakmak için
1. [Remove-AzScheduledQueryRule](https://docs.microsoft.com/powershell/module/az.monitor/remove-azscheduledqueryrule): Varolan bir günlük uyarı kuralını silmek için powershell cmdlet

> [!NOTE]
> ScheduledQueryRules PowerShell cmdlets yalnızca cmdlet kendisi oluşturulan kuralları yönetebilirsiniz veya Azure Monitor kullanarak - [Zamanlanmış Sorgu Kuralları API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/). Eski [Log Analytics Alert API](api-alerts.md) ve Log Analytics'in eski şablonları kullanılarak oluşturulan günlük uyarı [kuralları, planlanmış aramalar ve uyarılar,](../insights/solutions-resources-searches-alerts.md) yalnızca kullanıcı [Log Analytics Uyarıları için API tercihini değiştirdikten](alerts-log-api-switch.md)sonra ScheduledQueryRules PowerShell cmdlets kullanılarak yönetilebilir.

Aşağıdaki resimde, scheduledQueryRules PowerShell cmdlets kullanarak örnek bir günlük uyarı kuralı oluşturma adımları dır.
```powershell
$source = New-AzScheduledQueryRuleSource -Query 'Heartbeat | summarize AggregatedValue = count() by bin(TimeGenerated, 5m), _ResourceId' -DataSourceId "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/contosoRG/providers/microsoft.OperationalInsights/workspaces/servicews"

$schedule = New-AzScheduledQueryRuleSchedule -FrequencyInMinutes 15 -TimeWindowInMinutes 30

$metricTrigger = New-AzScheduledQueryRuleLogMetricTrigger -ThresholdOperator "GreaterThan" -Threshold 2 -MetricTriggerType "Consecutive" -MetricColumn "_ResourceId"

$triggerCondition = New-AzScheduledQueryRuleTriggerCondition -ThresholdOperator "LessThan" -Threshold 5 -MetricTrigger $metricTrigger

$aznsActionGroup = New-AzScheduledQueryRuleAznsActionGroup -ActionGroup "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/contosoRG/providers/microsoft.insights/actiongroups/sampleAG" -EmailSubject "Custom email subject" -CustomWebhookPayload "{ `"alert`":`"#alertrulename`", `"IncludeSearchResults`":true }"

$alertingAction = New-AzScheduledQueryRuleAlertingAction -AznsAction $aznsActionGroup -Severity "3" -Trigger $triggerCondition

New-AzScheduledQueryRule -ResourceGroupName "contosoRG" -Location "Region Name for your Application Insights App or Log Analytics Workspace" -Action $alertingAction -Enabled $true -Description "Alert description" -Schedule $schedule -Source $source -Name "Alert Name"
```

## <a name="managing-log-alerts-using-cli-or-api"></a>CLI veya API kullanarak günlük uyarılarını yönetme

Azure Monitor - [Zamanlanmış Sorgu Kuralları API'si](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/) bir REST API'sidir ve Azure Kaynak Yöneticisi REST API ile tam uyumludur. Bu nedenle, Azure CLI için Kaynak Yöneticisi komutlarını kullanarak Powershell üzerinden kullanılabilir.


> [!NOTE]
> Log Analytics için günlük uyarıları, eski [Log Analytics Alert API'sı](api-alerts.md) ve [Log Analytics kaydedilmiş arama ve uyarıların](../insights/solutions-resources-searches-alerts.md) eski şablonları kullanılarak da yönetilebilir. Varsayılan olarak burada ayrıntılı olarak ayrıntılı olarak yeni ScheduledQueryRules API'sini kullanma hakkında daha fazla bilgi [için, Log Analytics Uyarıları için yeni API'ye geçiş bölümüne](alerts-log-api-switch.md)bakın.

Günlük uyarıları şu anda özel CLI komutları yok; ancak aşağıda gösterildiği gibi, Kaynak Şablonu bölümünde daha önce gösterilen örnek Kaynak Şablonu (örnekPlanlıQueryRule.json) için Azure Kaynak Yöneticisi CLI komutu ile kullanılabilir:

```azurecli
az group deployment create --resource-group contosoRG --template-file sampleScheduledQueryRule.json
```

Başarılı bir işlemde, 201 devlet yeni uyarı kuralı oluşturma döndürülür veya varolan bir uyarı kuralı değiştirilirse 200 döndürülür.

## <a name="next-steps"></a>Sonraki adımlar

* Azure [Uyarıları'nda Günlük Uyarıları](../../azure-monitor/platform/alerts-unified-log.md) hakkında bilgi edinin
* [Günlük uyarıları için Webhook eylemlerini](../../azure-monitor/platform/alerts-log-webhook.md) anlama
* [Uygulama Öngörüleri](../../azure-monitor/app/analytics.md) hakkında daha fazla bilgi edinin
* [Günlük sorguları](../log-query/log-query-overview.md)hakkında daha fazla bilgi edinin.
