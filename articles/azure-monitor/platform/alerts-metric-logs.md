---
title: Azure Monitör'deki Günlükler için Metrik Uyarılar Oluşturma
description: Popüler günlük analizi verilerinde neredeyse gerçek zamanlı metrik uyarıları oluşturma konusunda öğretici.
author: yanivlavi
ms.author: yalavi
ms.topic: conceptual
ms.date: 09/17/2018
ms.subservice: alerts
ms.openlocfilehash: 6b21f228858954292e7a3bc5561d5e86fcfaaf41
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80055172"
---
# <a name="create-metric-alerts-for-logs-in-azure-monitor"></a>Azure Monitör'deki Günlükler için Metrik Uyarılar Oluşturma

## <a name="overview"></a>Genel Bakış

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure Monitör, [klasik uyarılara](../../azure-monitor/platform/alerts-classic-portal.md)göre avantajları olan [metrik uyarı türünü](../../azure-monitor/platform/alerts-metric-near-real-time.md) destekler. Ölçümler, Azure [hizmetlerinin büyük listesi](../../azure-monitor/platform/metrics-supported.md)için kullanılabilir. Bu makalede, kaynak için bir alt küme `Microsoft.OperationalInsights/workspaces`(yani) kullanımını açıklar - .

Azure'daki veya şirket içi kaynaklar da dahil olmak üzere Günlüklerden Ölçümlerin bir parçası olarak metrik olarak çıkarılan popüler Log Analytics günlüklerinde metrik uyarıları kullanabilirsiniz. Desteklenen Log Analytics çözümleri aşağıda listelenmiştir:

- Windows & Linux makineleri için [performans sayaçları](../../azure-monitor/platform/data-sources-performance-counters.md)
- [Ajan Sağlık için Kalp Atışı kayıtları](../../azure-monitor/insights/solution-agenthealth.md)
- [Yönetim](../../automation/automation-update-management.md) kayıtlarını güncelleştirme
- [Olay veri](../../azure-monitor/platform/data-sources-windows-events.md) günlükleri

Azure'da sorgu tabanlı [Günlük Uyarıları](../../azure-monitor/platform/alerts-log.md) üzerinden **Günlükler için Metrik Uyarıları** kullanmanın birçok avantajı vardır; bazıları aşağıda listelenmiştir:

- Metrik Uyarılar, aynı şeyi sağlamak için günlük kaynağından gelen günlükler için neredeyse gerçek zamanlı izleme yeteneği ve Metrik Uyarılar sunar.
- Metrik Uyarılar durum ludur - yalnızca uyarı ateşlendiğinde bir kez ve uyarı çözüldüğünde bir kez bildirme; stateless ve uyarı koşulu karşılanırsa her aralıkta ateş tutmak Log uyarıları, aksine.
- Günlük için Metrik Uyarılar, Bilgisayarlar, İşletim Sistemi Türü, vb. gibi belirli değerlere filtreleme sağlayan birden çok boyut sağlar; analitik sorgu kaleme gerek kalmadan.

> [!NOTE]
> Belirli metrik ve/veya boyut yalnızca seçilen dönemde veri varsa gösterilir. Bu ölçümler, Azure Günlük Analizi çalışma alanlarına sahip müşteriler için kullanılabilir.

## <a name="metrics-and-dimensions-supported-for-logs"></a>Günlükler için desteklenen ölçümler ve boyutlar

 Metrik uyarılar, boyutları kullanan ölçümler için uyarıyı destekler. Ölçümünüzü doğru düzeye filtrelemek için boyutları kullanabilirsiniz. [Log Analytics çalışma alanlarından](../../azure-monitor/platform/metrics-supported.md#microsoftoperationalinsightsworkspaces) Günlükler için desteklenen ölçümlerin tam listesi listelenir; desteklenen çözümler arasında.

> [!NOTE]
> [Azure Monitor - Ölçümler](../../azure-monitor/platform/metrics-charts.md)üzerinden Bir Günlük Analizi çalışma alanından çıkarılan desteklenen bir ölçütünü görüntülemek için, bu belirli metrikte bir günlük için metrik uyarı oluşturulması gerekir. Günlükler için metrik uyarıda seçilen boyutlar - yalnızca Azure Monitor - Ölçümler üzerinden keşif için görünür.

## <a name="creating-metric-alert-for-log-analytics"></a>Log Analytics için metrik uyarı oluşturma

Popüler günlüklerden gelen metrik veriler, Log Analytics'te işlenmeden önce Azure Monitor - Ölçümler'e aktarılır. Bu, kullanıcıların Metrik platformun ve metrik uyarının özelliklerinden yararlanabına olanak tanır - 1 dakikagibi düşük sıklıkta uyarılar da dahil.
Aşağıda günlükleri için bir metrik uyarı oluşturma aracıdır.

## <a name="prerequisites-for-metric-alert-for-logs"></a>Günlükler için Metrik Uyarı için Ön koşullar

Log Analytics verilerinde toplanan Günlükler için Metrik çalışmadan önce aşağıdakilerin ayarlanıp kullanılabilir olması gerekir:

1. **Active Log Analytics Workspace**: Geçerli ve etkin bir Log Analytics çalışma alanı bulunmalıdır. Daha fazla bilgi için bkz. [Azure portalında Bir Günlük Analizi Çalışma Alanı Oluştur.](../../azure-monitor/learn/quick-create-workspace.md)
2. **Aracı, Log Analytics Workspace için yapılandırılmıştır**: Aracının, önceki adımda kullanılan Log Analytics Çalışma Alanına veri göndermek için Azure VM'leri (ve/veya) şirket içi VM'leri için yapılandırılması gerekir. Daha fazla bilgi için [Bkz. Log Analytics - Agent Overview](../../azure-monitor/platform/agents-overview.md).
3. **Desteklenen Log Analytics Çözümleri yüklenir**: Log Analytics çözümü yapılandırılmalı ve Log Analytics çalışma alanına veri gönderilmelidir - desteklenen çözümler [Windows & Linux için Performans sayaçlarıdır,](../../azure-monitor/platform/data-sources-performance-counters.md)Ajan Sağlık için Heartbeat [kayıtları,](../../azure-monitor/insights/solution-agenthealth.md) [Güncelleme yönetimi](../../automation/automation-update-management.md)ve [Olay verileridir.](../../azure-monitor/platform/data-sources-windows-events.md)
4. **Günlük göndermek için yapılandırılan Log Analytics çözümleri**: Log Analytics çözümü, [Log Analytics çalışma alanları için desteklenen ölçümlere](../../azure-monitor/platform/metrics-supported.md#microsoftoperationalinsightsworkspaces) karşılık gelen gerekli günlüklere/verilere sahip olmalıdır. Örneğin, *% Kullanılabilir Bellek* sayacı için önce Performans [sayaçları](../../azure-monitor/platform/data-sources-performance-counters.md) çözümünde yapılandırılmalıdır.

## <a name="configuring-metric-alert-for-logs"></a>Günlükler için Metrik UyarıYı Yapılandırma

 Azure portalı, Kaynak Yöneticisi Şablonları, REST API, PowerShell ve Azure CLI kullanılarak metrik uyarılar oluşturulabilir ve yönetilebilir. Günlükler için Metrik Uyarılar, metrik uyarıların bir varyantı olduğundan - ön koşullar yapıldıktan sonra, belirtilen Log Analytics çalışma alanı için günlükler için metrik uyarı oluşturulabilir. [Metrik uyarıların](../../azure-monitor/platform/alerts-metric-near-real-time.md) tüm özellikleri ve işlevleri günlükler için metrik uyarılar için de geçerli olacaktır; yük şeması, geçerli kota limitleri ve faturalı fiyat dahil.

Adım adım ayrıntılar ve örnekler için metrik [uyarılar oluşturma ve yönetme](https://aka.ms/createmetricalert)ye bakın. Özellikle, Günlükler için Metrik Uyarılar için - metrik uyarıları yönetmek için yönergeleri izleyin ve aşağıdakileri sağlamak:

- Metrik uyarı için hedef geçerli bir *Log Analytics çalışma alanıdır*
- Seçili *Log Analytics çalışma alanı* için metrik uyarı için seçilen sinyal metrik tür **Metric**
- Boyut filtrelerini kullanarak belirli koşullar veya kaynak için filtreleme; günlükleri için ölçümler çok boyutlu
- *Sinyal Mantığını*yapılandırırken, birden çok boyut değerine (Bilgisayar gibi) yayılacak tek bir uyarı oluşturulabilir
- Seçili *Log Analytics çalışma alanı*için metrik uyarı oluşturmak için Azure portalını **kullanmıyorsanız;** daha sonra kullanıcı, [azure monitor - Zamanlanmış Sorgu Kuralları'nı](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules)kullanarak günlük verilerini metrme dönüştürmek için önce el ile açık bir kural oluşturmalıdır.

> [!NOTE]
> Azure portalı üzerinden Log Analytics çalışma alanı için metrik uyarı oluştururken - azure monitor üzerinden günlük verilerini metrik olarak dönüştürmek için ilgili kural [- Planlanmış Sorgu Kuralları,](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) *herhangi bir kullanıcı müdahalesi veya eylemine gerek kalmadan*arka planda otomatik olarak oluşturulur. Azure portalı dışındaki araçları kullanarak günlük oluşturma için metrik uyarı için metrik uyarı için bkz: Metrik uyarı oluşturmadan önce metrik dönüştürme kuralına scheduledqueryrule tabanlı günlük oluşturma nın örnek yollarından [Günlükler için Metrik Uyarılar](#resource-template-for-metric-alerts-for-logs) için Kaynak Şablonu bölümüne bakın , aksi takdirde oluşturulan günlüklerde metrik uyarı için veri olmayacaktır.

## <a name="resource-template-for-metric-alerts-for-logs"></a>Günlükler için Metrik Uyarılar için Kaynak Şablonu

Daha önce de belirtildiği gibi, günlüklerden metrik uyarıların oluşturulması işlemi iki uçlu:

1. ScheduledQueryRule API'yi kullanarak desteklenen günlüklerden ölçümleri ayıklamak için bir kural oluşturma
2. Hedef kaynak olarak günlükten (adım1'de) ve Log Analytics çalışma alanından çıkarılan metrik için metrik uyarı oluşturma

### <a name="metric-alerts-for-logs-with-static-threshold"></a>Statik eşikli Günlükler için Metrik Uyarılar

Aynı başarıyı elde etmek için aşağıdaki örnek Azure Kaynak Yöneticisi Şablonu'nu kullanabilirsiniz - statik eşik metrik uyarısı oluşturulması, scheduledQueryRule aracılığıyla günlüklerden ölçümleri ayıklamak için kuralın başarılı bir şekilde oluşturulmasına bağlıdır.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "convertRuleName": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the rule to convert log to metric"
            }
        },
        "convertRuleDescription": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Description for log converted to metric"
            }
        },
        "convertRuleRegion": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the region used by workspace"
            }
        },
        "convertRuleStatus": {
            "type": "string",
            "defaultValue": "true",
            "metadata": {
                "description": "Specifies whether the log conversion rule is enabled"
            }
        },
        "convertRuleMetric": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the metric once extraction done from logs."
            }
        },
        "alertName": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the alert"
            }
        },
        "alertDescription": {
            "type": "string",
            "defaultValue": "This is a metric alert",
            "metadata": {
                "description": "Description of alert"
            }
        },
        "alertSeverity": {
            "type": "int",
            "defaultValue": 3,
            "allowedValues": [
                0,
                1,
                2,
                3,
                4
            ],
            "metadata": {
                "description": "Severity of alert {0,1,2,3,4}"
            }
        },
        "isEnabled": {
            "type": "bool",
            "defaultValue": true,
            "metadata": {
                "description": "Specifies whether the alert is enabled"
            }
        },
        "resourceId": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Full Resource ID of the resource emitting the metric that will be used for the comparison. For example /subscriptions/00000000-0000-0000-0000-0000-00000000/resourceGroups/ResourceGroupName/providers/Microsoft.compute/virtualMachines/VM_xyz"
            }
        },
        "metricName": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the metric used in the comparison to activate the alert."
            }
        },
        "operator": {
            "type": "string",
            "defaultValue": "GreaterThan",
            "allowedValues": [
                "Equals",
                "NotEquals",
                "GreaterThan",
                "GreaterThanOrEqual",
                "LessThan",
                "LessThanOrEqual"
            ],
            "metadata": {
                "description": "Operator comparing the current value with the threshold value."
            }
        },
        "threshold": {
            "type": "string",
            "defaultValue": "0",
            "metadata": {
                "description": "The threshold value at which the alert is activated."
            }
        },
        "timeAggregation": {
            "type": "string",
            "defaultValue": "Average",
            "allowedValues": [
                "Average",
                "Minimum",
                "Maximum",
                "Total"
            ],
            "metadata": {
                "description": "How the data that is collected should be combined over time."
            }
        },
        "windowSize": {
            "type": "string",
            "defaultValue": "PT5M",
            "metadata": {
                "description": "Period of time used to monitor alert activity based on the threshold. Must be between five minutes and one day. ISO 8601 duration format."
            }
        },
        "evaluationFrequency": {
            "type": "string",
            "defaultValue": "PT1M",
            "metadata": {
                "description": "how often the metric alert is evaluated represented in ISO 8601 duration format"
            }
        },
        "actionGroupId": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "The ID of the action group that is triggered when the alert is activated or deactivated"
            }
        }
    },
    "variables": {
        "convertRuleTag": "hidden-link:/subscriptions/1234-56789-1234-567a/resourceGroups/resourceGroupName/providers/Microsoft.OperationalInsights/workspaces/workspaceName",
        "convertRuleSourceWorkspace": {
            "SourceId": "/subscriptions/1234-56789-1234-567a/resourceGroups/resourceGroupName/providers/Microsoft.OperationalInsights/workspaces/workspaceName"
        }
    },
    "resources": [
        {
            "name": "[parameters('convertRuleName')]",
            "type": "Microsoft.Insights/scheduledQueryRules",
            "apiVersion": "2018-04-16",
            "location": "[parameters('convertRuleRegion')]",
            "tags": {
                "[variables('convertRuleTag')]": "Resource"
            },
            "properties": {
                "description": "[parameters('convertRuleDescription')]",
                "enabled": "[parameters('convertRuleStatus')]",
                "source": {
                    "dataSourceId": "[variables('convertRuleSourceWorkspace').SourceId]"
                },
                "action": {
                    "odata.type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.Microsoft.AppInsights.Nexus.DataContracts.Resources.ScheduledQueryRules.LogToMetricAction",
                    "criteria": [{
                            "metricName": "[parameters('convertRuleMetric')]",
                            "dimensions": []
                        }
                    ]
                }
            }
        },
        {
            "name": "[parameters('alertName')]",
            "type": "Microsoft.Insights/metricAlerts",
            "location": "global",
            "apiVersion": "2018-03-01",
            "tags": {},
            "dependsOn":["[resourceId('Microsoft.Insights/scheduledQueryRules',parameters('convertRuleName'))]"],
            "properties": {
                "description": "[parameters('alertDescription')]",
                "severity": "[parameters('alertSeverity')]",
                "enabled": "[parameters('isEnabled')]",
                "scopes": ["[parameters('resourceId')]"],
                "evaluationFrequency":"[parameters('evaluationFrequency')]",
                "windowSize": "[parameters('windowSize')]",
                "criteria": {
                    "odata.type": "Microsoft.Azure.Monitor.SingleResourceMultipleMetricCriteria",
                    "allOf": [
                        {
                            "name" : "1st criterion",
                            "metricName": "[parameters('metricName')]",
                            "dimensions":[],
                            "operator": "[parameters('operator')]",
                            "threshold" : "[parameters('threshold')]",
                            "timeAggregation": "[parameters('timeAggregation')]"
                        }
                    ]
                },
                "actions": [
                    {
                        "actionGroupId": "[parameters('actionGroupId')]"
                    }
                ]
            }
        }
    ]
}
```

Yukarıdaki JSON metrikfromLogsAlertStatic.json olarak kaydedilir say - o zaman Kaynak Şablonu tabanlı oluşturma için bir parametre JSON dosyası ile birleştiğinde olabilir. Örnek bir parametre JSON dosyası aşağıda listelenmiştir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "convertRuleName": {
            "value": "TestLogtoMetricRule" 
        },
        "convertRuleDescription": {
            "value": "Test rule to extract metrics from logs via template"
        },
        "convertRuleRegion": {
            "value": "West Central US"
        },
        "convertRuleStatus": {
            "value": "true"
        },
        "convertRuleMetric": {
            "value": "Average_% Idle Time"
        },
        "alertName": {
            "value": "TestMetricAlertonLog"
        },
        "alertDescription": {
            "value": "New multi-dimensional metric alert created via template"
        },
        "alertSeverity": {
            "value":3
        },
        "isEnabled": {
            "value": true
        },
        "resourceId": {
            "value": "/subscriptions/1234-56789-1234-567a/resourceGroups/myRG/providers/Microsoft.OperationalInsights/workspaces/workspaceName"
        },
        "metricName":{
            "value": "Average_% Idle Time"
        },
        "operator": {
            "value": "GreaterThan"
        },
        "threshold":{
            "value": "1"
        },
        "timeAggregation":{
            "value": "Average"
        },
        "actionGroupId": {
            "value": "/subscriptions/1234-56789-1234-567a/resourceGroups/myRG/providers/microsoft.insights/actionGroups/actionGroupName"
        }
    }
}
```

Yukarıdaki parametre dosyasının metrik olarak kaydedilen varsayarsakLogsAlertStatic.parameters.json; daha sonra [Azure portalında oluşturmak için Kaynak Şablonu'nun](../../azure-resource-manager/templates/deploy-portal.md)kullanımı günlükleri için metrik uyarı oluşturabilirsiniz.

Alternatif olarak, aşağıdaki Azure Powershell komutunu da kullanabilirsiniz:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName "myRG" -TemplateFile metricfromLogsAlertStatic.json TemplateParameterFile metricfromLogsAlertStatic.parameters.json
```

Veya Azure CLI'yi kullanarak Kaynak Şablonu'na dağıt'ı kullanın:

```azurecli
az group deployment create --resource-group myRG --template-file metricfromLogsAlertStatic.json --parameters @metricfromLogsAlertStatic.parameters.json
```

### <a name="metric-alerts-for-logs-with-dynamic-thresholds"></a>Dinamik Eşiklere Sahip Günlükler için Metrik Uyarılar

Aynı başarıyı elde etmek için aşağıdaki örnek Azure Kaynak Yöneticisi Şablonu'nu kullanabilirsiniz - dinamik eşikler metrik uyarısı oluşturulması, scheduledQueryRule aracılığıyla günlüklerden ölçümler ilerletmek için kuralın başarılı bir şekilde oluşturulmasına bağlıdır.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "convertRuleName": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the rule to convert log to metric"
            }
        },
        "convertRuleDescription": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Description for log converted to metric"
            }
        },
        "convertRuleRegion": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the region used by workspace"
            }
        },
        "convertRuleStatus": {
            "type": "string",
            "defaultValue": "true",
            "metadata": {
                "description": "Specifies whether the log conversion rule is enabled"
            }
        },
        "convertRuleMetric": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the metric once extraction done from logs."
            }
        },
        "alertName": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the alert"
            }
        },
        "alertDescription": {
            "type": "string",
            "defaultValue": "This is a metric alert",
            "metadata": {
                "description": "Description of alert"
            }
        },
        "alertSeverity": {
            "type": "int",
            "defaultValue": 3,
            "allowedValues": [
                0,
                1,
                2,
                3,
                4
            ],
            "metadata": {
                "description": "Severity of alert {0,1,2,3,4}"
            }
        },
        "isEnabled": {
            "type": "bool",
            "defaultValue": true,
            "metadata": {
                "description": "Specifies whether the alert is enabled"
            }
        },
        "resourceId": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Full Resource ID of the resource emitting the metric that will be used for the comparison. For example /subscriptions/00000000-0000-0000-0000-0000-00000000/resourceGroups/ResourceGroupName/providers/Microsoft.compute/virtualMachines/VM_xyz"
            }
        },
        "metricName": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the metric used in the comparison to activate the alert."
            }
        },
        "operator": {
            "type": "string",
            "defaultValue": "GreaterOrLessThan",
            "allowedValues": [
                "GreaterThan",
                "LessThan",
                "GreaterOrLessThan"
            ],
            "metadata": {
                "description": "Operator comparing the current value with the threshold value."
            }
        },
        "alertSensitivity": {
            "type": "string",
            "defaultValue": "Medium",
            "allowedValues": [
                "High",
                "Medium",
                "Low"
            ],
            "metadata": {
                "description": "Tunes how 'noisy' the Dynamic Thresholds alerts will be: 'High' will result in more alerts while 'Low' will result in fewer alerts."
            }
        },
        "numberOfEvaluationPeriods": {
            "type": "string",
            "defaultValue": "4",
            "metadata": {
                "description": "The number of periods to check in the alert evaluation."
            }
        },
        "minFailingPeriodsToAlert": {
            "type": "string",
            "defaultValue": "3",
            "metadata": {
                "description": "The number of unhealthy periods to alert on (must be lower or equal to numberOfEvaluationPeriods)."
            }
        },
        "timeAggregation": {
            "type": "string",
            "defaultValue": "Average",
            "allowedValues": [
                "Average",
                "Minimum",
                "Maximum",
                "Total"
            ],
            "metadata": {
                "description": "How the data that is collected should be combined over time."
            }
        },
        "windowSize": {
            "type": "string",
            "defaultValue": "PT5M",
            "metadata": {
                "description": "Period of time used to monitor alert activity based on the threshold. Must be between five minutes and one day. ISO 8601 duration format."
            }
        },
        "evaluationFrequency": {
            "type": "string",
            "defaultValue": "PT1M",
            "metadata": {
                "description": "how often the metric alert is evaluated represented in ISO 8601 duration format"
            }
        },
        "actionGroupId": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "The ID of the action group that is triggered when the alert is activated or deactivated"
            }
        }
    },
    "variables": {
        "convertRuleTag": "hidden-link:/subscriptions/1234-56789-1234-567a/resourceGroups/resourceGroupName/providers/Microsoft.OperationalInsights/workspaces/workspaceName",
        "convertRuleSourceWorkspace": {
            "SourceId": "/subscriptions/1234-56789-1234-567a/resourceGroups/resourceGroupName/providers/Microsoft.OperationalInsights/workspaces/workspaceName"
        }
    },
    "resources": [
        {
            "name": "[parameters('convertRuleName')]",
            "type": "Microsoft.Insights/scheduledQueryRules",
            "apiVersion": "2018-04-16",
            "location": "[parameters('convertRuleRegion')]",
            "tags": {
                "[variables('convertRuleTag')]": "Resource"
            },
            "properties": {
                "description": "[parameters('convertRuleDescription')]",
                "enabled": "[parameters('convertRuleStatus')]",
                "source": {
                    "dataSourceId": "[variables('convertRuleSourceWorkspace').SourceId]"
                },
                "action": {
                    "odata.type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.Microsoft.AppInsights.Nexus.DataContracts.Resources.ScheduledQueryRules.LogToMetricAction",
                    "criteria": [{
                            "metricName": "[parameters('convertRuleMetric')]",
                            "dimensions": []
                        }
                    ]
                }
            }
        },
        {
            "name": "[parameters('alertName')]",
            "type": "Microsoft.Insights/metricAlerts",
            "location": "global",
            "apiVersion": "2018-03-01",
            "tags": {},
            "dependsOn":["[resourceId('Microsoft.Insights/scheduledQueryRules',parameters('convertRuleName'))]"],
            "properties": {
                "description": "[parameters('alertDescription')]",
                "severity": "[parameters('alertSeverity')]",
                "enabled": "[parameters('isEnabled')]",
                "scopes": ["[parameters('resourceId')]"],
                "evaluationFrequency":"[parameters('evaluationFrequency')]",
                "windowSize": "[parameters('windowSize')]",
                "criteria": {
                    "odata.type": "Microsoft.Azure.Monitor.MultipleResourceMultipleMetricCriteria",
                    "allOf": [
                        {
                            "criterionType": "DynamicThresholdCriterion",
                            "name" : "1st criterion",
                            "metricName": "[parameters('metricName')]",
                            "dimensions":[],
                            "operator": "[parameters('operator')]",
                            "alertSensitivity": "[parameters('alertSensitivity')]",
                            "failingPeriods": {
                                "numberOfEvaluationPeriods": "[parameters('numberOfEvaluationPeriods')]",
                                "minFailingPeriodsToAlert": "[parameters('minFailingPeriodsToAlert')]"
                            },
                            "timeAggregation": "[parameters('timeAggregation')]"
                        }
                    ]
                },
                "actions": [
                    {
                        "actionGroupId": "[parameters('actionGroupId')]"
                    }
                ]
            }
        }
    ]
}
```

Yukarıdaki JSON'un metricfromLogsAlertDynamic.json olarak kaydedileceğini varsayalım - o zaman Kaynak Şablonu tabanlı oluşturma için bir parametre JSON dosyası ile birleştiğinde olabilir. Örnek bir parametre JSON dosyası aşağıda listelenmiştir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "convertRuleName": {
            "value": "TestLogtoMetricRule"
        },
        "convertRuleDescription": {
            "value": "Test rule to extract metrics from logs via template"
        },
        "convertRuleRegion": {
            "value": "West Central US"
        },
        "convertRuleStatus": {
            "value": "true"
        },
        "convertRuleMetric": {
            "value": "Average_% Idle Time"
        },
        "alertName": {
            "value": "TestMetricAlertonLog"
        },
        "alertDescription": {
            "value": "New multi-dimensional metric alert created via template"
        },
        "alertSeverity": {
            "value":3
        },
        "isEnabled": {
            "value": true
        },
        "resourceId": {
            "value": "/subscriptions/1234-56789-1234-567a/resourceGroups/myRG/providers/Microsoft.OperationalInsights/workspaces/workspaceName"
        },
        "metricName":{
            "value": "Average_% Idle Time"
        },
        "operator": {
            "value": "GreaterOrLessThan"
          },
          "alertSensitivity": {
              "value": "Medium"
          },
          "numberOfEvaluationPeriods": {
              "value": "4"
          },
          "minFailingPeriodsToAlert": {
              "value": "3"
          },
        "timeAggregation":{
            "value": "Average"
        },
        "actionGroupId": {
            "value": "/subscriptions/1234-56789-1234-567a/resourceGroups/myRG/providers/microsoft.insights/actionGroups/actionGroupName"
        }
    }
}
```

Yukarıdaki parametre dosyasının metricfromLogsAlertDynamic.parameters.json olarak kaydedilen varsayıyor; daha sonra [Azure portalında oluşturmak için Kaynak Şablonu'nun](../../azure-resource-manager/templates/deploy-portal.md)kullanımı günlükleri için metrik uyarı oluşturabilirsiniz.

Alternatif olarak, aşağıdaki Azure Powershell komutunu da kullanabilirsiniz:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName "myRG" -TemplateFile metricfromLogsAlertDynamic.json TemplateParameterFile metricfromLogsAlertDynamic.parameters.json
```

Veya Azure CLI'yi kullanarak Kaynak Şablonu'na dağıt'ı kullanın:

```azurecli
az group deployment create --resource-group myRG --template-file metricfromLogsAlertDynamic.json --parameters @metricfromLogsAlertDynamic.parameters.json
```

## <a name="next-steps"></a>Sonraki adımlar

- [Metrik uyarılar](alerts-metric.md)hakkında daha fazla bilgi edinin.
- [Azure'daki günlük uyarıları](../../azure-monitor/platform/alerts-unified-log.md)hakkında bilgi edinin.
- [Azure'daki uyarılar](alerts-overview.md)hakkında bilgi edinin.
