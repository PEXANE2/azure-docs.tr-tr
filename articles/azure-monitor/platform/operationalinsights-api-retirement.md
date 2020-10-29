---
title: Azure Izleyici API 'SI emekli
description: Operationalınsights kaynak sağlayıcısı API 'sinin eski sürümlerini kullanımdan kaldırma işlemini açıklar.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/29/2020
ms.openlocfilehash: 4075e27c3711cbc4c584005126b53f7c99942049
ms.sourcegitcommit: dd45ae4fc54f8267cda2ddf4a92ccd123464d411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2020
ms.locfileid: "92926734"
---
# <a name="operationalinsights-api-version-retirement"></a>Operationalınsights API sürümü kullanımdan kaldırma
Microsoft, daha yeni/desteklenen bir sürüme geçişi düzgünleştirmek için API 'YI devre dışı bırakma konusunda en az 12 ay önceden bildirim sağlar. **Operationalınsights** kaynak sağlayıcısı API 'leri için yeni bir sürüm (2020-08-01) yayımladık ve 29 Şubat 2014 ' te önceki API sürümlerini devre dışı bırakacağız.

[Adanmış küme](https://docs.microsoft.com/azure/azure-monitor/log-query/logs-dedicated-clusters), [müşteri tarafından yönetilen anahtarlar](https://docs.microsoft.com/azure/azure-monitor/platform/customer-managed-keys), [özel bağlantı](https://docs.microsoft.com/azure/azure-monitor/platform/private-link-security) ve [veri dışa aktarma](https://docs.microsoft.com/azure/azure-monitor/platform/logs-data-export)gibi yeni işlevlerin avantajlarından yararlanmak için şimdi 2020-08-01 sürümünü kullanmaya başlamanız önerilir. Ayrıca, yeni özellikler ve işlevler ve iyileştirmeler yalnızca geçerli API 'ye eklenir.

29 Şubat 2014 ' ten sonra 2014 Azure Izleyici, artık 2020-08-01 ' den önceki API sürümlerini desteklemezler. Yükseltmemeyi tercih ediyorsanız, önceki sürümlerden gönderilen istekler, 29 Şubat 2014 ' e kadar Azure Izleyici hizmeti tarafından sunulmayı sürdürmeye devam edecektir.

## <a name="migration-steps"></a>Geçiş adımları
Kullandığınız yapılandırma yöntemine bağlı olarak, **rest** isteklerindeki yeni sürümü ve **Kaynak Yöneticisi şablonları** güncelleştirmelisiniz. API sürümünü güncelleştirmek için aşağıdaki örnekleri izleyin:

1. REST API istekler, isteğin URL 'sindeki API sürümünü kullanır. Aşağıdaki örnekte gösterildiği gibi, bu sürümü en son sürüm (2020-08-01) ile değiştirin.

    ```rest
    https://management.azure.com/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{workspaceName}?api-version=2020-08-01
    ```

2. Azure Resource Manager şablonlar, kaynağın **Apiversion** özelliğindeki API sürümünü kullanır. Aşağıdaki örnekte gösterildiği gibi, bu sürümü en son sürüm (2020-08-01) ile değiştirin.


    ```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2019-08-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "workspaceName": {
                "type": "string",
                "metadata": {
                "description": "Name of the workspace."
                }
            },
            "resources": [
            {
                "type": "Microsoft.OperationalInsights/workspaces",
                "name": "[parameters('workspaceName')]",
                "apiVersion": "2020-08-01",
                "location": "westus",
                "properties": {
                    "sku": {
                        "name": "pergb2018"
                    },
                    "retentionInDays": 30,
                    "features": {
                        "searchVersion": 1,
                        "legacy": 0,
                        "enableLogAccessUsingOnlyResourcePermissions": true
                    }
                }
            }
        ]
    }
    }
    ```


## <a name="next-steps"></a>Sonraki adımlar

- [Operationalınsights çalışma alanı API 'sine yönelik başvuruya](/rest/api/loganalytics/workspaces)bakın.
