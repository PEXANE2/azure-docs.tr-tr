---
title: Yönetim çözümlerinde kaydedilen aramalar | Microsoft Dokümanlar
description: Yönetim çözümleri genellikle çözüm tarafından toplanan verileri çözümlemek için kaydedilmiş günlük sorguları içerir. Bu makalede, Kaynak Yöneticisi şablonunda kaydedilmiş günlük analitiği aramaları nasıl tanımlanılıştırıştır.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 07/29/2019
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 61fc64e140af091b5ff3f631398daf901557791b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77663037"
---
# <a name="adding-log-analytics-saved-searches-and-alerts-to-management-solution-preview"></a>Log Analytics'in yönetim çözümüne aramaları ve uyarıları kaydetmesini ekleme (Önizleme)

> [!IMPORTANT]
> [Daha önce duyurulduğu](https://azure.microsoft.com/updates/switch-api-preference-log-alerts/)gibi, 1 Haziran *2019'dan* sonra oluşturulan günlük analitik çalışma alanı(lar) **yalnızca** Azure scheduledQueryRules [REST API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/), [Azure Kaynak Yöneticisi Şablonu](../../azure-monitor/platform/alerts-log.md#managing-log-alerts-using-azure-resource-template) ve [PowerShell cmdlet](../../azure-monitor/platform/alerts-log.md#managing-log-alerts-using-powershell)kullanarak uyarı kurallarını yönetebilecektir. Müşteriler, azure monitor scheduledQueryRules'dan varsayılan olarak yararlanmak ve yerel PowerShell cmdlets'i kullanma becerisi, kurallarda artan geri dönüş süresi, ayrı kaynak grubunda veya abonelikte kurallar oluşturulması ve çok daha fazlası gibi birçok [yeni avantaj](../../azure-monitor/platform/alerts-log-api-switch.md#benefits-of-switching-to-new-azure-api) elde etmek için eski çalışma alanları için tercih ettikleri uyarı kuralı yönetimi araçlarını kolayca [değiştirebilirler.](../../azure-monitor/platform/alerts-log-api-switch.md#process-of-switching-from-legacy-log-alerts-api)

> [!NOTE]
> Bu, şu anda önizlemede olan yönetim çözümleri oluşturmak için ön belgelerdir. Aşağıda açıklanan herhangi bir şema değişebilir.

[Yönetim çözümleri](solutions.md) genellikle, çözüm tarafından toplanan verileri analiz etmek için Log Analytics'te [kaydedilmiş aramaları](../../azure-monitor/log-query/log-query-overview.md) içerir. Ayrıca, kullanıcıyı bilgilendirmek için [uyarılar](../../azure-monitor/platform/alerts-overview.md) tanımlayabilir veya kritik bir soruna yanıt olarak otomatik olarak harekete geçebilir. Bu makalede, Yönetim [çözümlerine](solutions-creating.md)dahil edilebilmeleri için [Kaynak Yönetimi şablonundaki](../../azure-resource-manager/templates/quickstart-create-templates-use-the-portal.md) Günlük Analitiği kaydedilmiş aramaları ve uyarılarınasıl tanımlanabilecekleri açıklanmaktadır.

> [!NOTE]
> Bu makaledeki örnekler, yönetim çözümleri için gerekli veya ortak olan ve Tasarım'da açıklanan parametreleri ve değişkenleri kullanır [ve Azure'da bir yönetim çözümü oluşturur](solutions-creating.md)

## <a name="prerequisites"></a>Ön koşullar
Bu makalede, bir yönetim çözümü ve [Kaynak Yöneticisi şablonu](../../azure-resource-manager/templates/template-syntax.md) ve çözüm dosyasının yapısı [oluşturmak](solutions-creating.md) için zaten bildiğiniz varsayar.


## <a name="log-analytics-workspace"></a>Log Analytics Çalışma Alanı
Log Analytics'teki tüm kaynaklar bir [çalışma alanında](../../azure-monitor/platform/manage-access.md)bulunur. [Log Analytics çalışma alanı ve Otomasyon hesabında](solutions.md#log-analytics-workspace-and-automation-account)açıklandığı gibi, çalışma alanı yönetim çözümüne dahil değildir, ancak çözüm yüklenmeden önce var olmalıdır. Kullanılamıyorsa, çözüm yüklemesi başarısız olur.

Çalışma alanının adı her Log Analytics kaynağının adınadır. Bu, **SavedSearch** kaynağının aşağıdaki örneğinde olduğu gibi çalışma alanı parametresi ile çözümde yapılır.

    "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearchId'))]"

## <a name="log-analytics-api-version"></a>Log Analytics API sürümü
Kaynak Yöneticisi şablonunda tanımlanan tüm Log Analytics kaynakları, kaynağın kullanması gereken API sürümünü tanımlayan bir özellik **apiVersion'a** sahiptir.

Aşağıdaki tabloda, bu örnekte kullanılan kaynağın API sürümü listeleneb.)'de.

| Kaynak türü | API sürümü | Sorgu |
|:---|:---|:---|
| savedAramalar | 2017-03-15-önizleme | EventLevelName == "Hata" olay &#124;  |


## <a name="saved-searches"></a>Kayıtlı Aramalar
Kullanıcıların çözümünüz tarafından toplanan verileri sorgulamasına izin vermek için [kaydedilen aramaları](../../azure-monitor/log-query/log-query-overview.md) bir çözüme ekleyin. Kaydedilen aramalar Azure portalında **Kaydedilmiş Aramalar** altında görünür. Kaydedilen bir arama da her uyarı için gereklidir.

[Log Analytics kaydedilen arama](../../azure-monitor/log-query/log-query-overview.md) kaynakları `Microsoft.OperationalInsights/workspaces/savedSearches` nın bir türü vardır ve aşağıdaki yapıya sahiptir. Bu, bu kod parçacıklarını çözüm dosyanıza kopyalayıp yapıştırabilmeniz ve parametre adlarını değiştirebilmeniz için ortak değişkenleri ve parametreleri içerir.

    {
        "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name)]",
        "type": "Microsoft.OperationalInsights/workspaces/savedSearches",
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "dependsOn": [
        ],
        "tags": { },
        "properties": {
            "etag": "*",
            "query": "[variables('SavedSearch').Query]",
            "displayName": "[variables('SavedSearch').DisplayName]",
            "category": "[variables('SavedSearch').Category]"
        }
    }

Kaydedilmiş bir aramanın her özelliği aşağıdaki tabloda açıklanmıştır.

| Özellik | Açıklama |
|:--- |:--- |
| category | Kaydedilen arama kategorisi.  Aynı çözümde kaydedilen tüm aramalar genellikle tek bir kategoriyi paylaşır, böylece konsolda gruplanırlar. |
| Displayname | Portalda kaydedilen arama için görüntülenecek ad. |
| sorgu | Çalıştırılabilmek için sorgu. |

> [!NOTE]
> JSON olarak yorumlanabilecek karakterler içeriyorsa, sorguda kaçış karakterlerini kullanmanız gerekebilir. Örneğin, sorgunuz AzureEtkinliği ise **| OperationName:"Microsoft.Compute/virtualMachines/write"**, bu **AzureActivity | OperationName:/\"Microsoft.Compute/virtualMachines/write\"**.

## <a name="alerts"></a>Uyarılar
[Azure Günlüğü uyarıları,](../../azure-monitor/platform/alerts-unified-log.md) belirtilen günlük sorgularını düzenli aralıklarla çalıştıran Azure Uyarısı kuralları tarafından oluşturulur. Sorgu sonuçları belirtilen ölçütleri eşleşirse, bir uyarı kaydı oluşturulur ve bir veya daha fazla eylem [Eylem Grupları](../../azure-monitor/platform/action-groups.md)kullanılarak çalıştırılır.

Uyarıları Azure'a genişleten kullanıcılar için eylemler artık Azure eylem gruplarında denetlenir. Bir çalışma alanı ve uyarıları Azure'a genişletildiğinde, [Eylem Grubu - Azure Kaynak Yöneticisi Şablonu'nu](../../azure-monitor/platform/action-groups-create-resource-manager-template.md)kullanarak eylem alabilir veya ekleyebilirsiniz.
Eski yönetim çözümünde uyarı kuralları aşağıdaki üç farklı kaynaktan oluşur.

- **Kaydedilmiş arama.** Çalıştırılan günlük aramasını tanımlar. Birden çok uyarı kuralı, kaydedilen tek bir aramayı paylaşabilir.
- **Zamanlama.** Günlük aramasının ne sıklıkta çalıştırıldığını tanımlar. Her uyarı kuralının bir ve yalnızca bir zamanlaması vardır.
- **Uyarı eylemi.** Her uyarı kuralında, uyarı kaydının ne zaman oluşturulduğu ve uyarının önem derecesi gibi uyarının ayrıntılarını tanımlayan bir **uyarı** türüne sahip bir eylem grubu kaynağı veya eylem kaynağı (eski) vardır. [Eylem grubu](../../azure-monitor/platform/action-groups.md) kaynağı, sesli arama, SMS, e-posta, webhook, ITSM aracı, otomasyon runbook, mantık uygulaması, vb gibi uyarı ateşlendiğinde yapılacak yapılandırılmış eylemlerin bir listesi olabilir.

Kaydedilen arama kaynakları yukarıda açıklanmıştır. Diğer kaynaklar aşağıda açıklanmıştır.

### <a name="schedule-resource"></a>Kaynak zamanlama

Kaydedilen bir aramada, her zamanlama ayrı bir uyarı kuralını temsil eden bir veya daha fazla zamanlama olabilir. Zamanlama, aramanın ne sıklıkta çalıştırıldığını ve verilerin alındığı zaman aralığını tanımlar. Zamanlama kaynakları nın `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/` bir türü vardır ve aşağıdaki yapıya sahiptir. Bu, bu kod parçacıklarını çözüm dosyanıza kopyalayıp yapıştırabilmeniz ve parametre adlarını değiştirebilmeniz için ortak değişkenleri ve parametreleri içerir.

    {
        "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name, '/', variables('Schedule').Name)]",
        "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/",
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('SavedSearch').Name)]"
        ],
        "properties": {
            "etag": "*",
            "interval": "[variables('Schedule').Interval]",
            "queryTimeSpan": "[variables('Schedule').TimeSpan]",
            "enabled": "[variables('Schedule').Enabled]"
        }
    }
Zamanlama kaynaklarının özellikleri aşağıdaki tabloda açıklanmıştır.

| Öğe adı | Gerekli | Açıklama |
|:--|:--|:--|
| enabled       | Evet | Uyarı oluşturulduğunda etkin olup olmadığını belirtir. |
| interval      | Evet | Sorgunun dakikalar içinde ne sıklıkta çalıştığı. |
| queryTimeSpan | Evet | Sonuçları değerlendirmek için dakika içinde süre. |

Zamanlama kaynağı, zamanlamadan önce oluşturulabilmesi için kaydedilen aramaya bağlı olmalıdır.
> [!NOTE]
> Zamanlama Adı belirli bir çalışma alanında benzersiz olmalıdır; iki zamanlama, farklı kaydedilmiş aramalarla ilişkili olsalar bile aynı kimliğe sahip olamaz. Ayrıca, Log Analytics API ile oluşturulan tüm kaydedilen aramalar, zamanlamalar ve eylemlerin adı küçük olmalıdır.

### <a name="actions"></a>Eylemler
Zamanlamada birden çok eylem olabilir. Bir eylem, posta gönderme veya runbook başlatma gibi gerçekleştirmek için bir veya daha fazla işlem tanımlayabilir veya bir aramanın sonuçlarının bazı ölçütlerle ne zaman eşleşebileceğini belirleyen bir eşik tanımlayabilir. Bazı eylemler, eşik karşılandığında işlemlerin gerçekleştirileceği şekilde her ikisini de tanımlar.
Eylemler [eylem grubu] kaynağı veya eylem kaynağı kullanılarak tanımlanabilir.

**Tür** özelliği tarafından belirtilen iki tür eylem kaynağı vardır. Zamanlama, uyarı kuralının ayrıntılarını ve bir uyarı oluşturulduğunda hangi eylemlerin alındığını tanımlayan bir **Uyarı** eylemi gerektirir. Eylem kaynaklarının `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions`bir türü var.

Uyarı eylemleri aşağıdaki yapıya sahiptir. Bu, bu kod parçacıklarını çözüm dosyanıza kopyalayıp yapıştırabilmeniz ve parametre adlarını değiştirebilmeniz için ortak değişkenleri ve parametreleri içerir.

```json
{
    "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name, '/', variables('Schedule').Name, '/', variables('Alert').Name)]",
    "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions",
    "apiVersion": "[variables('LogAnalyticsApiVersion')]",
    "dependsOn": [
        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('SavedSearch').Name, '/schedules/', variables('Schedule').Name)]"
    ],
    "properties": {
        "etag": "*",
        "type": "Alert",
        "name": "[variables('Alert').Name]",
        "description": "[variables('Alert').Description]",
        "severity": "[variables('Alert').Severity]",
        "threshold": {
            "operator": "[variables('Alert').Threshold.Operator]",
            "value": "[variables('Alert').Threshold.Value]",
            "metricsTrigger": {
                "triggerCondition": "[variables('Alert').Threshold.Trigger.Condition]",
                "operator": "[variables('Alert').Trigger.Operator]",
                "value": "[variables('Alert').Trigger.Value]"
            },
        },
        "AzNsNotification": {
            "GroupIds": "[variables('MyAlert').AzNsNotification.GroupIds]",
            "CustomEmailSubject": "[variables('MyAlert').AzNsNotification.CustomEmailSubject]",
            "CustomWebhookPayload": "[variables('MyAlert').AzNsNotification.CustomWebhookPayload]"
        }
    }
}
```

Uyarı eylem kaynaklarının özellikleri aşağıdaki tablolarda açıklanmıştır.

| Öğe adı | Gerekli | Açıklama |
|:--|:--|:--|
| `type` | Evet | Eylem türü.  Bu uyarı eylemleri için **Uyarıdır.** |
| `name` | Evet | Uyarının adını görüntüleyin.  Bu, uyarı kuralı için konsolda görüntülenen addır. |
| `description` | Hayır | Uyarının isteğe bağlı açıklaması. |
| `severity` | Evet | Aşağıdaki değerlerden uyarı kaydının önem derecesi:<br><br> **Kritik**<br>**warning**<br>**Bilgi**

#### <a name="threshold"></a>Eşik
Bu bölüm gereklidir. Uyarı eşiğinin özelliklerini tanımlar.

| Öğe adı | Gerekli | Açıklama |
|:--|:--|:--|
| `Operator` | Evet | Aşağıdaki değerlerden karşılaştırma için işleç:<br><br>**gt =<br>lt'den büyük = daha az** |
| `Value` | Evet | Sonuçları karşılaştırmak için değer. |

##### <a name="metricstrigger"></a>Metriktetik
Bu bölüm isteğe bağlıdır. Metrik ölçüm uyarısı için ekleyin.

| Öğe adı | Gerekli | Açıklama |
|:--|:--|:--|
| `TriggerCondition` | Evet | Eşiğin toplam ihlal sayısı veya aşağıdaki değerlerden art arda ihlaller için olup olmadığını belirtir:<br><br>**Ardışık Toplam<br>** |
| `Operator` | Evet | Aşağıdaki değerlerden karşılaştırma için işleç:<br><br>**gt =<br>lt'den büyük = daha az** |
| `Value` | Evet | Uyarıyı tetiklemek için ölçütlerin karşılanması gereken sayı. |


#### <a name="throttling"></a>Azaltma
Bu bölüm isteğe bağlıdır. Bir uyarı oluşturulduktan sonra bir süre için aynı kural uyarıları bastırmak istiyorsanız bu bölümü ekleyin.

| Öğe adı | Gerekli | Açıklama |
|:--|:--|:--|
| Süreler Dakika | Azaltma öğesi dahil sayılsa evet | Aynı uyarı kuralından bir uyarı oluşturulduktan sonra uyarıları bastırmak için dakika sayısı. |

#### <a name="azure-action-group"></a>Azure eylem grubu
Azure'daki tüm uyarılar, eylemleri işlemek için varsayılan mekanizma olarak Eylem Grubu'nu kullanın. Action Group ile eylemlerinizi bir kez belirtebilir ve ardından eylem grubunu Azure genelinde birden çok uyarıyla ilişkilendirebilirsiniz. Gerek kalmadan, tekrar tekrar aynı eylemleri ilan etmek. Eylem Grupları, e-posta, SMS, Sesli Arama, ITSM Bağlantısı, Otomasyon Runbook, Webhook URI ve daha fazlası dahil olmak üzere birden çok eylemi destekler.

Uyarılarını Azure'a genişleten kullanıcı için bir zamanlama, bir uyarı oluşturabilmek için artık eylem grubu ayrıntılarının eşikle birlikte geçirilmesi gerekir. E-posta ayrıntıları, Webhook URL'leri, Runbook Otomasyonu ayrıntıları ve diğer Eylemler, bir uyarı oluşturmadan önce bir Eylem Grubu tarafında tanımlanması gerekir; Portal'da [Azure Monitor'dan Eylem Grubu](../../azure-monitor/platform/action-groups.md) oluşturabilir veya Eylem Grubu - Kaynak [Şablonu'nun](../../azure-monitor/platform/action-groups-create-resource-manager-template.md)kullanılmasını kullanabilir.

| Öğe adı | Gerekli | Açıklama |
|:--|:--|:--|
| AzNsBildirimi | Evet | Uyarı ölçütleri karşılandığında gerekli eylemleri yapmak için uyarıyla ilişkilendirilecek Azure eylem grubunun kaynak kimliği. |
| CustomEmailKonu | Hayır | İlişkili eylem grubunda belirtilen tüm adreslere gönderilen postanın özel konu satırı. |
| CustomWebhookPayload | Hayır | İlişkili eylem grubunda tanımlanan tüm webhook uç noktalarına gönderilecek özelleştirilmiş yük. Biçimi webhook bekliyor ne bağlıdır ve geçerli bir seri JSON olmalıdır. |

## <a name="sample"></a>Örnek

Aşağıdaki kaynakları içeren bir çözüm örneği vereme:

- Kayıtlı arama
- Zamanlama
- Eylem grubu

Örnek, kaynak tanımlarında hardcoding değerlerinin aksine, bir çözümde yaygın olarak kullanılacak [standart çözüm parametreleri]( solutions-solution-file.md#parameters) değişkenlerini kullanır.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0",
    "parameters": {
        "workspaceName": {
            "type": "string",
            "metadata": {
                "Description": "Name of Log Analytics workspace"
            }
        },
        "workspaceregionId": {
            "type": "string",
            "metadata": {
                "Description": "Region of Log Analytics workspace"
            }
        },
        "actiongroup": {
            "type": "string",
            "metadata": {
                "Description": "List of action groups for alert actions separated by semicolon"
            }
        }
    },
    "variables": {
        "SolutionName": "MySolution",
        "SolutionVersion": "1.0",
        "SolutionPublisher": "Contoso",
        "ProductName": "SampleSolution",
        "LogAnalyticsApiVersion-Search": "2017-03-15-preview",
        "LogAnalyticsApiVersion-Solution": "2015-11-01-preview",
        "MySearch": {
            "displayName": "Error records by hour",
            "query": "MyRecord_CL | summarize AggregatedValue = avg(Rating_d) by Instance_s, bin(TimeGenerated, 60m)",
            "category": "Samples",
            "name": "Samples-Count of data"
        },
        "MyAlert": {
            "Name": "[toLower(concat('myalert-',uniqueString(resourceGroup().id, deployment().name)))]",
            "DisplayName": "My alert rule",
            "Description": "Sample alert. Fires when 3 error records found over hour interval.",
            "Severity": "critical",
            "ThresholdOperator": "gt",
            "ThresholdValue": 3,
            "Schedule": {
                "Name": "[toLower(concat('myschedule-',uniqueString(resourceGroup().id, deployment().name)))]",
                "Interval": 15,
                "TimeSpan": 60
            },
            "MetricsTrigger": {
                "TriggerCondition": "Consecutive",
                "Operator": "gt",
                "Value": 3
            },
            "ThrottleMinutes": 60,
            "AzNsNotification": {
                "GroupIds": [
                    "[parameters('actiongroup')]"
                ],
                "CustomEmailSubject": "Sample alert"
            }
        }
    },
    "resources": [
        {
            "name": "[concat(variables('SolutionName'), '[' ,parameters('workspacename'), ']')]",
            "location": "[parameters('workspaceRegionId')]",
            "tags": { },
            "type": "Microsoft.OperationsManagement/solutions",
            "apiVersion": "[variables('LogAnalyticsApiVersion-Solution')]",
            "dependsOn": [
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches', parameters('workspacename'), variables('MySearch').Name)]",
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name)]",
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Name)]"
            ],
            "properties": {
                "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspacename'))]",
                "referencedResources": [
                ],
                "containedResources": [
                    "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches', parameters('workspacename'), variables('MySearch').Name)]",
                    "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name)]",
                    "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Name)]"
                ]
            },
            "plan": {
                "name": "[concat(variables('SolutionName'), '[' ,parameters('workspaceName'), ']')]",
                "Version": "[variables('SolutionVersion')]",
                "product": "[variables('ProductName')]",
                "publisher": "[variables('SolutionPublisher')]",
                "promotionCode": ""
            }
        },
        {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches",
            "apiVersion": "[variables('LogAnalyticsApiVersion-Search')]",
            "dependsOn": [ ],
            "tags": { },
            "properties": {
                "etag": "*",
                "query": "[variables('MySearch').query]",
                "displayName": "[variables('MySearch').displayName]",
                "category": "[variables('MySearch').category]"
            }
        },
        {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name, '/', variables('MyAlert').Schedule.Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/",
            "apiVersion": "[variables('LogAnalyticsApiVersion-Search')]",
            "dependsOn": [
                "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('MySearch').Name)]"
            ],
            "properties": {
                "etag": "*",
                "interval": "[variables('MyAlert').Schedule.Interval]",
                "queryTimeSpan": "[variables('MyAlert').Schedule.TimeSpan]",
                "enabled": true
            }
        },
        {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name, '/', variables('MyAlert').Schedule.Name, '/', variables('MyAlert').Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions",
            "apiVersion": "[variables('LogAnalyticsApiVersion-Search')]",
            "dependsOn": [
                "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('MySearch').Name, '/schedules/', variables('MyAlert').Schedule.Name)]"
            ],
            "properties": {
                "etag": "*",
                "Type": "Alert",
                "Name": "[variables('MyAlert').DisplayName]",
                "Description": "[variables('MyAlert').Description]",
                "Severity": "[variables('MyAlert').Severity]",
                "Threshold": {
                    "Operator": "[variables('MyAlert').ThresholdOperator]",
                    "Value": "[variables('MyAlert').ThresholdValue]",
                    "MetricsTrigger": {
                        "TriggerCondition": "[variables('MyAlert').MetricsTrigger.TriggerCondition]",
                        "Operator": "[variables('MyAlert').MetricsTrigger.Operator]",
                        "Value": "[variables('MyAlert').MetricsTrigger.Value]"
                    }
                },
                "Throttling": {
                    "DurationInMinutes": "[variables('MyAlert').ThrottleMinutes]"
                },
                "AzNsNotification": {
                    "GroupIds": "[variables('MyAlert').AzNsNotification.GroupIds]",
                    "CustomEmailSubject": "[variables('MyAlert').AzNsNotification.CustomEmailSubject]"
                }
            }
        }
    ]
}
```

Aşağıdaki parametre dosyası bu çözüm için örnek değerleri sağlar.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workspacename": {
            "value": "myWorkspace"
        },
        "accountName": {
            "value": "myAccount"
        },
        "workspaceregionId": {
            "value": "East US"
        },
        "regionId": {
            "value": "East US 2"
        },
        "pricingTier": {
            "value": "Free"
        },
        "actiongroup": {
            "value": "/subscriptions/3b540246-808d-4331-99aa-917b808a9166/resourcegroups/myTestGroup/providers/microsoft.insights/actiongroups/sample"
        }
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar
* Yönetim çözümünüze [görünümler ekleyin.](solutions-resources-views.md)
* Yönetim çözümünüze [Otomasyon runbook'larını ve diğer kaynakları ekleyin.](solutions-resources-automation.md)
