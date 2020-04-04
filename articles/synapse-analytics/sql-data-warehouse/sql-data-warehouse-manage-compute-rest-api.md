---
title: REST API'leri ile duraklatma, devam, ölçeklendirme
description: Azure Synapse Analytics veri ambarında işlem gücünü REST API'leri aracılığıyla yönetin.
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 03/29/2019
ms.author: kevin
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: 4efd5c63af9f09d41733e8e172270410245977ec
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80633212"
---
# <a name="rest-apis-for-azure-sql-data-warehouse"></a>Azure SQL Veri Ambarı için REST API'leri

Azure Synapse Analytics veri ambarında bilgi işlem yönetimi için REST API'leri.

## <a name="scale-compute"></a>Hesaplamayı ölçeklendirme

Veri ambarı birimlerini değiştirmek için [Veritabanı OLUŞTUR veya Güncelleştir](/rest/api/sql/databases/createorupdate?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) API'sini kullanın. Aşağıdaki örnek, veri ambarı birimlerini, sunucu MyServer'da barındırılan MySQLDW veritabanı için DW1000 olarak ayarlar. Sunucu, ResourceGroup1 adlı bir Azure kaynak grubundadır.

```
PATCH https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}?api-version=2014-04-01-preview HTTP/1.1
Content-Type: application/json; charset=UTF-8

{
    "properties": {
        "requestedServiceObjectiveName": DW1000
    }
}
```

## <a name="pause-compute"></a>İşlemi duraklatma

Veritabanını duraklatmak için [Veritabanı](/rest/api/sql/databases/pause?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) REST API'sini kullanın. Aşağıdaki örnek, Server01 adlı bir sunucuda barındırılan Database02 adlı bir veritabanını duraklatacaktır. Sunucu, ResourceGroup1 adlı bir Azure kaynak grubundadır.

```
POST https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/pause?api-version=2014-04-01-preview HTTP/1.1
```

## <a name="resume-compute"></a>Özgeçmiş hesaplama

Veritabanı başlatmak için [Özgeçmiş Veritabanı](/rest/api/sql/databases/resume?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) REST API'sini kullanın. Aşağıdaki örnek, Server01 adlı bir sunucuda barındırılan Database02 adlı bir veritabanı nı başlatır. Sunucu, ResourceGroup1 adlı bir Azure kaynak grubundadır.

```
POST https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/resume?api-version=2014-04-01-preview HTTP/1.1
```

## <a name="check-database-state"></a>Veritabanı durumunu denetleme

> [!NOTE]
> Veritabanı çevrimiçi iş akışını tamamlarken veritabanı durumunu denetle, bağlantı hatalarına neden olabilir. Bağlantı girişimlerini tetiklemek için bu API çağrısını kullanıyorsanız, uygulama kodunuza 2 ila 3 dakika gecikme eklemeniz gerekebilir.

```
GET https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}?api-version=2014-04-01 HTTP/1.1
```

## <a name="get-maintenance-schedule"></a>Bakım çizelgesi alın

Veri ambarı için ayarlanan bakım zamanlamasını denetleyin.

```
GET https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/maintenanceWindows/current?maintenanceWindowName=current&api-version=2017-10-01-preview HTTP/1.1

```

## <a name="set-maintenance-schedule"></a>Bakım çizelgesini ayarlama

Varolan bir veri ambarında bakım çizelgesi ayarlamak ve güncelleştirmek için.

```
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/maintenanceWindows/current?maintenanceWindowName=current&api-version=2017-10-01-preview HTTP/1.1

{
    "properties": {
        "timeRanges": [
                {
                                "dayOfWeek": Saturday,
                                "startTime": 00:00,
                                "duration": 08:00,
                },
                {
                                "dayOfWeek": Wednesday
                                "startTime": 00:00,
                                "duration": 08:00,
                }
                ]
    }
}

```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için [bkz.](sql-data-warehouse-manage-compute-overview.md)
