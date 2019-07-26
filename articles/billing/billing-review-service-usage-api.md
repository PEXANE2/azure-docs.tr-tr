---
title: REST API ile Azure hizmeti kaynak kullanımını inceleyin | Microsoft Docs
description: Azure hizmeti kaynak kullanımını gözden geçirmek için Azure REST API 'Lerini nasıl kullanacağınızı öğrenin.
services: billing
documentationcenter: na
author: lleonard-msft
manager: ''
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2018
ms.author: banders
ms.openlocfilehash: 47e19fae26d6e3bc465799980c587d7bb7ed5e92
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68443058"
---
# <a name="review-azure-resource-usage-using-the-rest-api"></a>REST API kullanarak Azure Kaynak kullanımını gözden geçirin

Azure maliyet yönetimi API 'Leri, Azure kaynaklarınızın tüketimini gözden geçirmenize ve yönetmenize yardımcı olur.

Bu makalede, saatlik kullanım bilgilerinizi içeren bir virgülle ayrılmış değer belgesi oluşturacak günlük bir rapor oluşturmayı ve sonra sanal makinelerin, veritabanlarının ve etiketlerinizin kullanımını sorgulayabilmeniz için filtreleri nasıl kullanacağınızı öğreneceksiniz bir Azure Kaynak grubundaki kaynaklar.

>[!NOTE]
> Maliyet yönetimi API 'SI Şu anda özel önizlemededir.

## <a name="create-a-basic-cost-management-report"></a>Temel maliyet yönetimi raporu oluşturma

Maliyet raporlama 'nın nasıl oluşturulduğunu ve raporların nerede yayımlanalınacağını tanımlamak için maliyet yönetimi API 'sindeki işlemikullanın.`reports`

```http
https://management.azure.com/subscriptions/{subscriptionGuid}/providers/Microsoft.CostManagement/reports/{reportName}?api-version=2018-09-01-preview
Content-Type: application/json   
Authorization: Bearer
```

`{subscriptionGuid}` Parametresi gereklidir ve API belirtecinde belirtilen kimlik bilgileri kullanılarak okunabilecek bir abonelik kimliği içermelidir. İçin`{reportName}`

Aşağıdaki üstbilgiler gereklidir: 

|İstek üst bilgisi|Açıklama|  
|--------------------|-----------------|  
|*Content-Type:*| Gerekli. Olarak `application/json`ayarlayın. |  
|*Authorization:*| Gerekli. Geçerli `Bearer` bir belirteç olarak ayarlayın. |

HTTP istek gövdesinde raporun parametrelerini yapılandırın. Aşağıdaki örnekte rapor, etkin olduğunda her gün oluşturulacak şekilde ayarlanır, bir Azure Depolama Blobu kapsayıcısına yazılmış bir CSV dosyasıdır ve kaynak grubundaki `westus`tüm kaynaklar için saatlik maliyet bilgilerini içerir.

```json
{
    "properties": {
        "schedule": {
            "status": "Inactive",
            "recurrence": "Daily",
            "recurrencePeriod": {
                "from": "2018-08-21",
                "to": "2019-10-31"
            }
        },
        "deliveryInfo": {
            "destination": {
                "resourceId": "/subscriptions/{subscriptionGuid}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}",
                "container": "MyReportContainer",
                "rootFolderPath": "MyScheduleTest"
            }
        },
        "format": "Csv",
        "definition": {
            "type": "Usage",
            "timeframe": "MonthToDate",
            "dataSet": {
                "granularity": "Hourly",
                "filter": {
                    "dimensions": {
                        "name": "ResourceLocation",
                        "operator": "In",
                        "values": [
                            "westus"
                        ]
                    }
                }
            }
        }
    }
}
```

Sanal Makineye (VM) bağlı bir veya birden çok işletim sistemi diski içerdiği için

## <a name="filtering-reports"></a>Raporları filtreleme

Bir rapor `dimensions` oluştururken istek gövdesinin vebölümü,belirlikaynaktürlerininmaliyetlerineodaklanabilmenizisağlar.`filter` Önceki istek gövdesinde, bir bölgedeki tüm kaynaklara göre nasıl filtreleneceği gösterilir. 

### <a name="get-all-compute-usage"></a>Tüm işlem kullanımını al

Tüm bölgelerde aboneliğinizdeki Azure sanal makine maliyetlerini raporlamak için boyutukullanın.`ResourceType`

```json
"filter": {
    "dimensions": {
        "name": "ResourceType",
        "operator": "In",
        "values": [
                "Microsoft.ClassicCompute/virtualMachines", 
                "Microsoft.Compute/virtualMachines"
        ] 
    }
}
```

### <a name="get-all-database-usage"></a>Tüm veritabanı kullanımını al

Tüm bölgelerde aboneliğinizdeki Azure SQL veritabanı maliyetlerini raporlamak için boyutukullanın.`ResourceType`

```json
"filter": {
    "dimensions": {
        "name": "ResourceType",
        "operator": "In",
        "values": [
                "Microsoft.Sql/servers"
        ] 
    }
}
```

### <a name="report-on-specific-instances"></a>Belirli örnekler hakkında rapor

Boyut `Resource` , belirli kaynaklar için maliyetleri raporlamanızı sağlar.

```json
"filter": {
    "dimensions": {
        "name": "Resource",
        "operator": "In",
        "values": [
            "subscriptions/{subscriptionGuid}/resourceGroups/{resourceGroup}/providers/Microsoft.ClassicCompute/virtualMachines/{ResourceName}"
        ]
    }
}
```

### <a name="changing-timeframes"></a>Zaman çerçevelerini değiştirme

Tanımı, `timeframe` haftanın dışında `Custom` tarih ve ay yerleşik seçeneklerinin bulunduğu zaman dilimini ayarlamak için olarak ayarlayın.

```json
"timeframe": "Custom",
"timePeriod": {
    "from": "2017-12-31T00:00:00.000Z",
    "to": "2018-12-30T00:00:00.000Z"
}
```

## <a name="next-steps"></a>Sonraki adımlar
- [Azure REST API kullanmaya başlama](https://docs.microsoft.com/rest/api/azure/)   
