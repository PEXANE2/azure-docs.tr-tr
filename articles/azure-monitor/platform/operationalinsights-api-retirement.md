---
title: Azure Izleyici API 'SI emekli
description: Operationalınsights kaynak sağlayıcısı API 'sinin eski sürümlerini kullanımdan kaldırma işlemini açıklar.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/02/2020
ms.openlocfilehash: 708901dbc37daa075cf38d3f9ef046ae658ea979
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91744993"
---
# <a name="operationalinsights-api-version-retirement"></a>Operationalınsights API sürümü kullanımdan kaldırma
Microsoft, daha yeni/desteklenen bir sürüme geçişi düzgünleştirmek için API 'YI devre dışı bırakma konusunda en az 12 ay önceden bildirim sağlar. **Operationalınsights** kaynak sağlayıcısı API 'leri için yeni bir sürüm (2020-08-01) yayımladık ve 31 Ekim 2023 TARIHINDE önceki API sürümlerini devre dışı bırakacağız. Yeni özellikler ve işlevler ve iyileştirmeler yalnızca geçerli API 'ye eklendiğinden, en son API sürümüne mümkün olduğunca erken yükseltmeniz gerekir.

31 Ekim 2023 ' den sonra Azure Izleyici artık 2020-08-01 ' den önceki API sürümlerini desteklememektedir. Yükseltmemeyi tercih ediyorsanız, önceki sürümlerden gönderilen istekler, 31 Ekim 2023 ' e kadar Azure Izleyici hizmeti tarafından sunulmayı sürdürmeye devam edecektir.

Kullandığınız yapılandırma yöntemine bağlı olarak, aşağıdaki örnekler temelinde REST isteklerindeki yeni sürümü ve Kaynak Yöneticisi şablonlarını güncelleştirmeniz gerekir:


## <a name="rest"></a>REST
REST API istekler, isteğin URL 'sindeki API sürümünü kullanır. Aşağıdaki örnekte gösterildiği gibi bu sürümü en son sürümle değiştirin.

```rest
https://management.azure.com/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{workspaceName}?api-version=2020-08-01
```

## <a name="azure-resource-manager"></a>Azure Resource Manager
Kaynak Yöneticisi şablonlar, kaynağın **Apiversion** özelliğindeki API sürümünü kullanır. Aşağıdaki örnekte gösterildiği gibi bu sürümü en son sürümle değiştirin.


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

- [Operationalınsights API 'sine yönelik başvuruya](https://docs.microsoft.com/azure/templates/microsoft.operationalinsights/allversions)bakın.
