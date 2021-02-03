---
title: REST API kullanarak Azure Cosmos DB geri yüklenebilen SQL API veritabanlarını listeleme
description: Geri yüklenebilen hesap altındaki tüm Azure Cosmos DB SQL veritabanlarında yapılan tüm mutasyonların olay akışını görüntüleyin. Bu, silme süresini almak için veritabanının yanlışlıkla silindiği senaryoya yardımcı olur.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 02/03/2021
ms.author: govindk
ms.openlocfilehash: d3d72cff5fcfeed17d60e2f856be4adc1a983819
ms.sourcegitcommit: ea822acf5b7141d26a3776d7ed59630bf7ac9532
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2021
ms.locfileid: "99527771"
---
# <a name="list-restorable-sql-api-databases-in-azure-cosmos-db-using-rest-api"></a>REST API kullanarak Azure Cosmos DB geri yüklenebilen SQL API veritabanlarını listeleme

Geri yüklenebilen hesap altındaki tüm Azure Cosmos DB SQL veritabanlarında yapılan tüm mutasyonların olay akışını görüntüleyin. Bu, silme süresini almak için veritabanının yanlışlıkla silindiği senaryoya yardımcı olur. Bu API için `Microsoft.DocumentDB/locations/restorableDatabaseAccounts/*/read` izin gerekiyor

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.DocumentDB/locations/{location}/restorableDatabaseAccounts/{instanceId}/restorableSqlDatabases?api-version=2020-06-01-preview
```

## <a name="uri-parameters"></a>URI parametreleri

| Name | İçinde | Gerekli | Tür | Description |
| --- | --- | --- | --- | --- |
| **InstanceId** | path | True |string| Geri yüklenebilen bir veritabanı hesabının InstanceId GUID 'SI. |
| **konumuna** | path | True | string| Azure Cosmos DB bölge, sözcükler ve her sözcük için büyük harfler arasında boşluk. |
| **subscriptionId** | path | True | string| Hedef aboneliğin KIMLIĞI. |
| **api sürümü** | sorgu | True | string | Bu işlem için kullanılacak API sürümü. |

## <a name="responses"></a>Yanıtlar

| Ad | Tür | Description |
| --- | --- | --- |
| 200 TAMAM | [RestorableSqlDatabasesListResult](#restorablesqldatabaseslistresult)| İşlem başarıyla tamamlandı. |
| Diğer durum kodları | [DefaultErrorResponse](#defaulterrorresponse)| İşlemin neden başarısız olduğunu açıklayan hata yanıtı. |

## <a name="examples"></a>Örnekler

### <a name="cosmosdbrestorablesqldatabaselist"></a>Cosmosdbrestorayblesqldatabaselist

**Örnek İstek**

```http
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.DocumentDB/locations/WestUS/restorableDatabaseAccounts/d9b26648-2f53-4541-b3d8-3044f4f9810d/restorableSqlDatabases?api-version=2020-06-01-preview
```

**Örnek yanıt**

Durum kodu: 200

```json
{
  "value": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.DocumentDb/locations/westus/restorableDatabaseAccounts/36f09704-6be3-4f33-aa05-17b73e504c75/restorableSqlDatabases/59c21367-b98b-4a8e-abb7-b6f46600decc",
      "type": "Microsoft.DocumentDB/locations/restorableDatabaseAccounts/restorableSqlDatabases",
      "name": "59c21367-b98b-4a8e-abb7-b6f46600decc",
      "properties": {
        "resource": {
          "_rid": "DLB14gAAAA==",
          "eventTimestamp": "2020-09-02T19:45:03Z",
          "ownerId": "Database1",
          "ownerResourceId": "3fu-hg==",
          "operationType": "Create",
          "database": {
            "id": "Database1",
            "_rid": "3fu-hg==",
            "_self": "dbs/3fu-hg==/",
            "_etag": "\"0000c20a-0000-0700-0000-5f4ff63f0000\"",
            "_colls": "colls/",
            "_users": "users/"
          }
        }
      }
    },
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.DocumentDb/locations/westus/restorableDatabaseAccounts/d9b26648-2f53-4541-b3d8-3044f4f9810d/restorableSqlDatabases/8456cb17-cdb0-4c6a-8db8-d0ff3f886257",
      "type": "Microsoft.DocumentDB/locations/restorableDatabaseAccounts/restorableSqlDatabases",
      "name": "8456cb17-cdb0-4c6a-8db8-d0ff3f886257",
      "properties": {
        "resource": {
          "_rid": "ESXNLAAAAA==",
          "eventTimestamp": "2020-09-02T19:53:42Z",
          "ownerId": "Database1",
          "ownerResourceId": "3fu-hg==",
          "database": {
            "id": "Database1",
            "_rid": "3fu-hg==",
            "_self": "dbs/3fu-hg==/",
            "_etag": "\"0000c20a-0000-0700-0000-5f4ff63f0000\"",
            "_colls": "colls/",
            "_users": "users/",
            "_ts": 1599075903
          },
          "operationType": "Delete"
        }
      }
    },
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.DocumentDb/locations/westus/restorableDatabaseAccounts/d9b26648-2f53-4541-b3d8-3044f4f9810d/restorableSqlDatabases/2c07991b-9c7c-4e85-be68-b18c1f2ff326",
      "type": "Microsoft.DocumentDB/locations/restorableDatabaseAccounts/restorableSqlDatabases",
      "name": "2c07991b-9c7c-4e85-be68-b18c1f2ff326",
      "properties": {
        "resource": {
          "_rid": "aXFqUQAAAA==",
          "eventTimestamp": "2020-09-02T19:53:15Z",
          "ownerId": "Database2",
          "ownerResourceId": "0SziSg==",
          "database": {
            "id": "Database2",
            "_rid": "0SziSg==",
            "_self": "dbs/0SziSg==/",
            "_etag": "\"0000ca0a-0000-0700-0000-5f4ff82b0000\"",
            "_colls": "colls/",
            "_users": "users/"
          },
          "operationType": "Create"
        }
      }
    }
  ]
}
```

## <a name="definitions"></a>Tanımlar

| Tanım | Açıklama | | --- || --- | | [Veritabanı](#database) | SQL veritabanı kaynak nesnesini Azure Cosmos DB | | [Defaulterrorresponse](#defaulterrorresponse) | Hizmetten bir hata yanıtı. | | [ErrorResponse](#errorresponse) | Hata yanıtı. | | [OperationType](#operationtype) | Etkinliğin işlem türünü belirtmek için sabit listesi. | | [Kaynak](#resource) | Azure Cosmos DB SQL veritabanı olayının kaynağı | | [Restorayblesqldatabasegetresult](#restorablesqldatabasegetresult) | Bir SQL veritabanı olayı Azure Cosmos DB | | [Restooyblesqldatabaseproperties](#restorablesqldatabaseproperties) | Azure Cosmos DB SQL veritabanı olayının özellikleri | | [RestorableSqlDatabasesListResult](#restorablesqldatabaseslistresult) | SQL veritabanı olaylarını ve bunların özelliklerini içeren liste işlemi yanıtı. |

### <a name="database"></a><a id="database"></a>Veritabanınızı

SQL veritabanı kaynak nesnesi Azure Cosmos DB

| **Ad**  |  **Tür**  |  **Açıklama** | | --- || --- | ---| | _colls | dize | Koleksiyonlar kaynağının adreslenebilir yolunu belirten sistem tarafından oluşturulan bir özellik. | | _etag | dize | İyimser eşzamanlılık denetimi için gereken kaynak ETag 'i temsil eden sistem tarafından üretilmiş bir özellik. | | _rid | dize | Sistem tarafından oluşturulan bir özellik. Benzersiz bir tanımlayıcı. | | _self | dize | Veritabanı kaynağının adreslenebilir yolunu belirten sistem tarafından oluşturulan bir özellik. | | _ts | | Kaynağın son güncelleştirilme zaman damgasını belirten sistem tarafından oluşturulan bir özellik. | | _users | dize | Kullanıcının kaynağının adreslenebilir yolunu belirten sistem tarafından üretilmiş bir özellik. | | KIMLIK | dize | Azure Cosmos DB SQL veritabanının adı |

### <a name="defaulterrorresponse"></a><a id="defaulterrorresponse"></a>DefaultErrorResponse

Hizmetten bir hata yanıtı.

| **Ad** | **Tür** | **Açıklama** |
| --- | --- | --- |
| error | [ErrorResponse](#errorresponse)| Hata yanıtı. |

### <a name="errorresponse"></a><a id="errorresponse"></a>ErrorResponse

Hata yanıtı.

| **Ad** | **Tür** | **Açıklama** |
| --- | --- | --- |
| kod |string| Hata kodu. |
| message |string| İşlemin neden başarısız olduğunu belirten hata iletisi. |

### <a name="operationtype"></a><a id="operationtype"></a>OperationType

Etkinliğin işlem türünü belirtmek için sabit listesi.

| **Ad** | **Tür** | **Açıklama** |
| --- | --- | --- |
| Oluştur |string|veritabanı oluşturma olayı|
| Sil |string|Veritabanı silme olayı|
| Değiştir |string|veritabanı değiştirme olayı|
| SystemOperation |string|sistem tarafından tetiklenen veritabanı değiştirme olayı. Bu olay kullanıcı tarafından başlatılmaz|

### <a name="resource"></a><a id="resource"></a>Kaynak

Azure Cosmos DB SQL veritabanı olayının kaynağı

| **Ad** | **Tür** | **Açıklama** |
| --- | --- | --- |
| _rid |string| Sistem tarafından oluşturulan bir özellik. Benzersiz bir tanımlayıcı. |
| database |[Veritabanı](#database)| SQL veritabanı kaynak nesnesi Azure Cosmos DB |
| eventTimestamp |string| Bu veritabanı olayının gerçekleştiği zaman. |
| operationType |[OperationType](#operationtype)| Bu veritabanı olayının işlem türü. |
| OwnerId |string| SQL veritabanının adı. |
| Ownerresourceıd |string| SQL veritabanının kaynak KIMLIĞI. |

### <a name="restorablesqldatabasegetresult"></a><a id="restorablesqldatabasegetresult"></a>Restorayblesqldatabasegetresult

Azure Cosmos DB SQL veritabanı olayı

| **Ad** | **Tür** | **Açıklama** |
| --- | --- | --- |
| ID |string| Azure Resource Manager kaynağının benzersiz kaynak tanımlayıcısı. |
| name |string| Azure Resource Manager kaynağının adı. |
| properties | [Restooyblesqldatabaseproperties](#restorablesqldatabaseproperties)| Bir SQL veritabanı olayının özellikleri. |
| tür |string| Azure kaynağının türü. |

### <a name="restorablesqldatabaseproperties"></a><a id="restorablesqldatabaseproperties"></a>Restooyblesqldatabaseproperties

Azure Cosmos DB SQL veritabanı olayının özellikleri

| **Ad** | **Tür** | **Açıklama** |
| --- | --- | --- |
| kaynak |[Kaynak](#resource)| Azure Cosmos DB SQL veritabanı olayının kaynağı |

### <a name="restorablesqldatabaseslistresult"></a><a id="restorablesqldatabaseslistresult"></a>RestorableSqlDatabasesListResult

SQL veritabanı olaylarını ve bunların özelliklerini içeren liste işlemi yanıtı.

| **Ad** | **Tür** | **Açıklama** |
| --- | --- | --- |
| değer |[Restorayblesqldatabasegetresult](#restorablesqldatabasegetresult)[]| SQL veritabanı olaylarının ve bunların özelliklerinin listesi. |

## <a name="next-steps"></a>Sonraki adımlar

* REST API kullanarak Azure Cosmos DB SQL API 'sindeki geri [yüklenebilen kapsayıcıları listeleyin](restorable-sql-containers-list.md) .
* Sürekli yedekleme modunun [kaynak modeli](continuous-backup-restore-resource-model.md) .