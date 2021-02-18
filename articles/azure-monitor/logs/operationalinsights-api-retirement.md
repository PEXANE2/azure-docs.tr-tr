---
title: Azure Izleyici API 'SI emekli
description: Operationalınsights kaynak sağlayıcısı API 'sinin eski sürümlerini kullanımdan kaldırma işlemini açıklar.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/29/2020
ms.openlocfilehash: 6564e7263639f0a78df6f2674ce7a4b610fb0fc1
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100623744"
---
# <a name="operationalinsights-api-version-retirement"></a>Operationalınsights API sürümü kullanımdan kaldırma
Microsoft, daha yeni/desteklenen bir sürüme geçişi düzgünleştirmek için API 'YI devre dışı bırakma konusunda en az 12 ay önceden bildirim sağlar. **Operationalınsights** kaynak sağlayıcısı API 'leri için yeni bir sürüm (2020-08-01) yayımladık ve 29 Şubat 2024 TARIHINDE önceki API sürümlerini devre dışı bırakacağız.

[Adanmış küme](../log-query/logs-dedicated-clusters.md), [müşteri tarafından yönetilen anahtarlar](../logs/customer-managed-keys.md), [özel bağlantı](./private-link-security.md) ve [veri dışa aktarma](./logs-data-export.md)gibi yeni işlevlerin avantajlarından yararlanmak için şimdi 2020-08-01 sürümünü kullanmaya başlamanız önerilir. Ayrıca, yeni özellikler ve işlevler ve iyileştirmeler yalnızca geçerli API 'ye eklenir.

29 Şubat 2024 tarihinden sonra Azure Izleyici, artık 2020-08-01 ' den önceki API sürümlerini desteklemezler. Yükseltmemeyi tercih ediyorsanız, önceki sürümlerden gönderilen istekler, 29 Şubat 2024 ' e kadar Azure Izleyici hizmeti tarafından sunulmayı sürdürmeye devam edecektir.

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


### <a name="more-information"></a>Daha fazla bilgi
Sorularınız varsa, [teknik topluluk uzmanlarımızın]( https://techcommunity.microsoft.com/t5/azure-monitor/bd-p/AzureMonitor)yanıtlarını alın. Destek planınız varsa ve teknik yardıma ihtiyacınız varsa, bir [destek isteği]( https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)oluşturun: 
1.  *Sorun türü* altında **Teknik**' i seçin. 
2.  *Abonelik* bölümünde aboneliğinizi seçin. 
3.  *Hizmet* altında **Hizmetlerim**' i seçin ve ardından **Log Analytics**' yi seçin. 
4.  *Özet* altında, sorununuzun açıklamasını yazın. 
5.  *Sorun türü* altında **Log Analytics çalışma alanı yönetimi**' ni seçin.  
6.  *Sorun alt türü*' nün altında **ARM şablonları, PowerShell ve CLI**' yi seçin. 

## <a name="next-steps"></a>Sonraki adımlar

- [Operationalınsights çalışma alanı API 'sine yönelik başvuruya](/rest/api/loganalytics/workspaces)bakın.