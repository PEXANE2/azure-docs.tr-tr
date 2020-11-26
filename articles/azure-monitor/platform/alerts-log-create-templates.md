---
title: Azure Resource Manager şablonuyla bir günlük uyarısı oluşturun | Microsoft Docs
description: Günlük uyarısı oluşturmak için Kaynak Yöneticisi şablonu kullanmayı öğrenin
author: yanivlavi
ms.author: yalavi
ms.topic: conceptual
ms.date: 08/26/2020
ms.subservice: alerts
ms.openlocfilehash: 3fb0288d592af8bd23415b31ecf6a6e3448b92ba
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2020
ms.locfileid: "96186040"
---
# <a name="create-a-log-alert-with-a-resource-manager-template"></a>Resource Manager şablonu ile günlük uyarısı oluşturma

Günlük uyarıları, kullanıcıların kaynak günlüklerini her ayarlama sıklığını değerlendirmek ve sonuçlara göre bir uyarı tetiklemesi için [Log Analytics](../log-query/log-analytics-tutorial.md) bir sorgu kullanmasına izin verir. Kurallar, [eylem gruplarını](./action-groups.md)kullanarak bir veya daha fazla eylemi çalıştırmayı tetikleyebilir. [Günlük uyarılarının işlevselliği ve terminolojisi hakkında daha fazla bilgi edinin](alerts-unified-log.md).

Bu makalede, Azure Izleyici 'de [günlük uyarılarını](alerts-unified-log.md) yapılandırmak için bir [Azure Resource Manager şablonunu](../../azure-resource-manager/templates/template-syntax.md) nasıl kullanabileceğiniz gösterilmektedir. Kaynak Yöneticisi şablonlar, ortamınızda tutarlı ve tekrarlanabilir bir şekilde uyarı ayarlamanıza olanak sağlar. Günlük uyarıları `Microsoft.Insights/scheduledQueryRules` kaynak sağlayıcıda oluşturulur. Bkz. [zamanlanan sorgu KURALLARı API](/rest/api/monitor/scheduledqueryrules/)'SI için API başvurusu.

Temel adımlar aşağıdaki gibidir:

1. Aşağıdaki şablonlardan birini, uyarının nasıl oluşturulacağını açıklayan bir JSON dosyası olarak kullanın.
2. Uyarıyı özelleştirmek için ilgili parametreler dosyasını bir JSON olarak düzenleyin ve kullanın.
4. Herhangi bir dağıtım yöntemini kullanarak şablonu dağıtın.

> [!NOTE]
> [Log Analytics çalışma alanındaki](../log-query/log-analytics-tutorial.md) günlük verileri Azure izleyici ölçüm deposuna gönderilebilir. Ölçüm uyarıları [farklı davranışa](alerts-metric-overview.md)sahiptir ve bu işlem, çalıştığınız verilere bağlı olarak daha çok tercih edilebilir. Günlükleri ölçümlere yönlendirme hakkında bilgi için bkz. [Günlükler Için ölçüm uyarısı](alerts-metric-logs.md).

> [!NOTE]
> Eski [Log Analytics uyarı API 'si](api-alerts.md) ve [Log Analytics kayıtlı aramaların ve uyarıların](../insights/solutions.md)eski şablonları kullanılarak yönetilmek üzere kullanılan Log Analytics için günlük uyarıları. [Geçerli ScheduledQueryRules API 'sine geçiş hakkında daha fazla bilgi edinin](alerts-log-api-switch.md).


## <a name="simple-template-up-to-api-version-2018-04-16"></a>Basit şablon (API sürüm 2018-04-16 ' e kadar)

[Zamanlanan sorgu kuralları oluşturma](/rest/api/monitor/scheduledqueryrules/createorupdate) şablonu [sonuç günlüğü uyarısı](alerts-unified-log.md#count-of-the-results-table-rows) (örnek veri kümesi değişken) temelinde:

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

Bu JSON, [Azure portal Azure Resource Manager](../../azure-resource-manager/templates/deploy-portal.md#deploy-resources-from-custom-template)kullanılarak kaydedilip dağıtılabilir.

## <a name="template-with-cross-resource-query-up-to-api-version-2018-04-16"></a>Çapraz kaynak sorgusuna sahip şablon (API sürüm 2018-04-16 ' e kadar)

[Çapraz kaynakları](../log-query/cross-workspace-query.md) sorgulayan [ölçüm ölçüsüne](./alerts-unified-log.md#calculation-of-measure-based-on-a-numeric-column-such-as-cpu-counter-value) dayalı olarak [zamanlanan sorgu kuralları oluşturma](/rest/api/monitor/scheduledqueryrules/createorupdate) şablonu (örnek veri kümesi değişken olarak ayarlanır):

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

Bu JSON, [Azure portal Azure Resource Manager](../../azure-resource-manager/templates/deploy-portal.md#deploy-resources-from-custom-template)kullanılarak kaydedilip dağıtılabilir.

## <a name="template-for-all-resource-types-from-api-version-2020-05-01-preview"></a>Tüm kaynak türleri için şablon (API sürümünden 2020-05-01-Önizleme)

Tüm kaynak türleri için [Zamanlanmış sorgu kuralları oluşturma](/rest/api/monitor/scheduledqueryrules/createorupdate) şablonu (değişken olarak ayarlanan örnek veriler):

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "alertName": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the alert"
            }
        },
        "location": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Location of the alert"
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
        "query": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the metric used in the comparison to activate the alert."
            }
        },
        "metricMeasureColumn": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the measure column used in the alert evaluation."
            }
        },
        "resourceIdColumn": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the resource ID column used in the alert targeting the alerts."
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
                "Total",
                "Count"
            ],
            "metadata": {
                "description": "How the data that is collected should be combined over time."
            }
        },
        "windowSize": {
            "type": "string",
            "defaultValue": "PT5M",
            "allowedValues": [
                "PT1M",
                "PT5M",
                "PT15M",
                "PT30M",
                "PT1H",
                "PT6H",
                "PT12H",
                "PT24H"
            ],
            "metadata": {
                "description": "Period of time used to monitor alert activity based on the threshold. Must be between one minute and one day. ISO 8601 duration format."
            }
        },
        "evaluationFrequency": {
            "type": "string",
            "defaultValue": "PT1M",
            "allowedValues": [
                "PT1M",
                "PT5M",
                "PT15M",
                "PT30M",
                "PT1H"
            ],
            "metadata": {
                "description": "how often the metric alert is evaluated represented in ISO 8601 duration format"
            }
        },
        "muteActionsDuration": {
            "type": "string",
            "defaultValue": "PT5M",
            "allowedValues": [
                "PT1M",
                "PT5M",
                "PT15M",
                "PT30M",
                "PT1H",
                "PT6H",
                "PT12H",
                "PT24H"
            ],
            "metadata": {
                "description": "Mute actions for the chosen period of time (in ISO 8601 duration format) after the alert is fired."
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
    "variables": {  },
    "resources": [
        {
            "name": "[parameters('alertName')]",
            "type": "Microsoft.Insights/scheduledQueryRules",
            "location": "[parameters('location')]",
            "apiVersion": "2020-05-01-preview",
            "tags": {},
            "properties": {
                "description": "[parameters('alertDescription')]",
                "severity": "[parameters('alertSeverity')]",
                "enabled": "[parameters('isEnabled')]",
                "scopes": ["[parameters('resourceId')]"],
                "evaluationFrequency":"[parameters('evaluationFrequency')]",
                "windowSize": "[parameters('windowSize')]",
                "criteria": {
                    "allOf": [
                        {
                            "query": "[parameters('query')]",
                            "metricMeasureColumn": "[parameters('metricMeasureColumn')]",
                            "resourceIdColumn": "[parameters('resourceIdColumn')]",
                            "dimensions":[],
                            "operator": "[parameters('operator')]",
                            "threshold" : "[parameters('threshold')]",
                            "timeAggregation": "[parameters('timeAggregation')]",
                            "failingPeriods": {
                                "numberOfEvaluationPeriods": "[parameters('numberOfEvaluationPeriods')]",
                                "minFailingPeriodsToAlert": "[parameters('minFailingPeriodsToAlert')]"
                            }
                        }
                    ]
                },
                "muteActionsDuration": "[parameters('muteActionsDuration')]",
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

Bu JSON, [Azure portal Azure Resource Manager](../../azure-resource-manager/templates/deploy-portal.md#deploy-resources-from-custom-template)kullanılarak kaydedilip dağıtılabilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Günlük uyarıları](./alerts-unified-log.md) hakkında bilgi edinin
* [Günlük uyarılarını yönetme](./alerts-log.md) hakkında bilgi edinin
* [Günlük uyarıları için Web kancası eylemlerini](./alerts-log-webhook.md) anlama
* [Günlük sorguları](../log-query/log-query-overview.md)hakkında daha fazla bilgi edinin.