---
title: REST API kullanarak Azure Cosmos DB geri yüklenebilen SQL API kapsayıcıları listeleme
description: Belirli bir veritabanı altındaki tüm Azure Cosmos DB SQL kapsayıcılarında yapılan tüm mutasyonların olay akışını görüntüleyin. Bu, kapsayıcının yanlışlıkla silindiği senaryoya yardımcı olur.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 02/03/2021
ms.author: govindk
ms.openlocfilehash: bba00b10d1a24dc29c1e986de1f0144c00350f5d
ms.sourcegitcommit: 44188608edfdff861cc7e8f611694dec79b9ac7d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2021
ms.locfileid: "99537433"
---
# <a name="list-restorable-sql-api-containers-in-azure-cosmos-db-using-rest-api"></a>REST API kullanarak Azure Cosmos DB geri yüklenebilen SQL API kapsayıcıları listeleme

> [!IMPORTANT]
> Azure Cosmos DB için zaman noktası geri yükleme özelliği (sürekli yedekleme modu) Şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Belirli bir veritabanı altındaki tüm Azure Cosmos DB SQL kapsayıcılarında yapılan tüm mutasyonların olay akışını görüntüleyin. Bu, kapsayıcının yanlışlıkla silindiği senaryoya yardımcı olur. Bu API için `Microsoft.DocumentDB/locations/restorableDatabaseAccounts/*/read` izin gerekiyor

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.DocumentDB/locations/{location}/restorableDatabaseAccounts/{instanceId}/restorableSqlContainers?api-version=2020-06-01-preview
```

İsteğe bağlı parametrelerle:

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.DocumentDB/locations/{location}/restorableDatabaseAccounts/{instanceId}/restorableSqlContainers?api-version=2020-06-01-preview&amp;restorableSqlDatabaseRid={restorableSqlDatabaseRid}
```

## <a name="uri-parameters"></a>URI parametreleri

| Name | İçinde | Gerekli | Tür | Description |
| --- | --- | --- | --- | --- |
| **InstanceId** | path | True |string| Geri yüklenebilen bir veritabanı hesabının InstanceId GUID 'SI. |
| **konumuna** | path | True | string| Azure Cosmos DB bölge, sözcükler ve her sözcük için büyük harfler arasında boşluk. |
| **subscriptionId** | path | True | string| Hedef aboneliğin KIMLIĞI. |
| **api sürümü** | sorgu | True | string | Bu işlem için kullanılacak API sürümü. |
| **restorableSqlDatabaseRid** | sorgu | |string| SQL veritabanının kaynak KIMLIĞI. |

## <a name="responses"></a>Yanıtlar

| Ad | Tür | Description |
| --- | --- | --- |
| 200 TAMAM | [Restorayblesqlcontainerslistresult](#restorablesqlcontainerslistresult)| İşlem başarıyla tamamlandı. |
| Diğer durum kodları | [DefaultErrorResponse](#defaulterrorresponse)| İşlemin neden başarısız olduğunu açıklayan hata yanıtı. |

## <a name="examples"></a>Örnekler

### <a name="cosmosdbrestorablesqlcontainerlist"></a>Cosmosdbrestokıblesqlcontainerlist

**Örnek istek**

```http
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.DocumentDB/locations/WestUS/restorableDatabaseAccounts/98a570f2-63db-4117-91f0-366327b7b353/restorableSqlContainers?api-version=2020-06-01-preview&amp;restorableSqlDatabaseRid=3fu-hg==
```

**Örnek yanıt**

Durum kodu: 200

```json
{
  "value": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.DocumentDb/locations/westus/restorableDatabaseAccounts/98a570f2-63db-4117-91f0-366327b7b353/restorableSqlContainers/79609a98-3394-41f8-911f-cfab0c075c86",
      "type": "Microsoft.DocumentDB/locations/restorableDatabaseAccounts/restorableSqlContainers",
      "name": "79609a98-3394-41f8-911f-cfab0c075c86",
      "properties": {
        "resource": {
          "_rid": "zAyAPQAAAA==",
          "eventTimestamp": "2020-10-13T04:56:42Z",
          "ownerId": "Container1",
          "ownerResourceId": "V18LoLrv-qA=",
          "operationType": "Create",
          "container": {
            "id": "Container1",
            "indexingPolicy": {
              "indexingMode": "Consistent",
              "automatic": true,
              "includedPaths": [
                {
                  "path": "/*"
                },
                {
                  "path": "/\"_ts\"/?"
                }
              ],
              "excludedPaths": [
                {
                  "path": "/\"_etag\"/?"
                }
              ]
            },
            "conflictResolutionPolicy": {
              "mode": "LastWriterWins",
              "conflictResolutionPath": "/_ts",
              "conflictResolutionProcedure": ""
            },
            "_rid": "V18LoLrv-qA=",
            "_self": "dbs/V18LoA==/colls/V18LoLrv-qA=/",
            "_etag": "\"00003e00-0000-0700-0000-5f85338a0000\""
          }
        }
      }
    },
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.DocumentDb/locations/westus/restorableDatabaseAccounts/98a570f2-63db-4117-91f0-366327b7b353/restorableSqlContainers/e85298a1-c631-4726-825e-a7ca092e9098",
      "type": "Microsoft.DocumentDB/locations/restorableDatabaseAccounts/restorableSqlContainers",
      "name": "e85298a1-c631-4726-825e-a7ca092e9098",
      "properties": {
        "resource": {
          "_rid": "PrArcgAAAA==",
          "eventTimestamp": "2020-10-13T05:03:27Z",
          "ownerId": "Container1",
          "ownerResourceId": "V18LoLrv-qA=",
          "operationType": "Replace",
          "container": {
            "id": "Container1",
            "indexingPolicy": {
              "indexingMode": "Consistent",
              "automatic": true,
              "includedPaths": [
                {
                  "path": "/*"
                },
                {
                  "path": "/\"_ts\"/?"
                }
              ],
              "excludedPaths": [
                {
                  "path": "/\"_etag\"/?"
                }
              ]
            },
            "defaultTtl": 12345,
            "conflictResolutionPolicy": {
              "mode": "LastWriterWins",
              "conflictResolutionPath": "/_ts",
              "conflictResolutionProcedure": ""
            },
            "_rid": "V18LoLrv-qA=",
            "_self": "dbs/V18LoA==/colls/V18LoLrv-qA=/",
            "_etag": "\"00004400-0000-0700-0000-5f85351f0000\""
          }
        }
      }
    }
  ]
}
```

## <a name="definitions"></a>Tanımlar

| Tanım | Açıklama | | --- || --- | | [Kapsayıcı](#container) | SQL Container Resource nesnesi Azure Cosmos DB | | [Defaulterrorresponse](#defaulterrorresponse) | Hizmetten bir hata yanıtı. | | [ErrorResponse](#errorresponse) | Hata yanıtı. | | [OperationType](#operationtype) | Etkinliğin işlem türünü belirtmek için sabit listesi. | | [Kaynak](#resource) | Azure Cosmos DB SQL kapsayıcı olayının kaynağı | | [Restorayblesqlcontainergetresult](#restorablesqlcontainergetresult) | Azure Cosmos DB bir SQL kapsayıcı olayı | | [Restorampa\blesqlcontainerproperties](#restorablesqlcontainerproperties) | Azure Cosmos DB SQL kapsayıcı olayının özellikleri | | [Restooyblesqlcontainerslistresult](#restorablesqlcontainerslistresult) | SQL kapsayıcı olaylarını ve bunların özelliklerini içeren liste işlemi yanıtı. |

### <a name="container"></a><a id="container"></a>Kapsayıcı

Azure Cosmos DB SQL Container Resource nesnesi

| **Ad**  |  **Tür**  |  **Açıklama** | | --- || --- | ---| | _etag | dize | İyimser eşzamanlılık denetimi için gereken kaynak ETag 'i temsil eden sistem tarafından üretilmiş bir özellik. | | _rid | dize | Sistem tarafından oluşturulan bir özellik. Benzersiz bir tanımlayıcı. | | _self | dize | Kapsayıcı kaynağının adreslenebilir yolunu belirten sistem tarafından oluşturulan bir özellik. | | _ts | | Kaynağın son güncelleştirilme zaman damgasını belirten sistem tarafından oluşturulan bir özellik. | | KIMLIK | dize | Azure Cosmos DB SQL kapsayıcısının adı |

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
| Oluştur |string|kapsayıcı oluşturma olayı|
| Sil |string|kapsayıcı silme olayı|
| Değiştir |string|kapsayıcı değiştirme olayı|
| SystemOperation |string|sistem tarafından tetiklenen kapsayıcı değiştirme olayı. Bu olay kullanıcı tarafından başlatılmaz|

### <a name="resource"></a><a id="resource"></a>Kaynak

Azure Cosmos DB SQL kapsayıcı olayının kaynağı

| **Ad** | **Tür** | **Açıklama** |
| --- | --- | --- |
| _rid |string| Sistem tarafından oluşturulan bir özellik. Benzersiz bir tanımlayıcı. |
| kapsayıcı |[Kapsayıcı](#container)| Azure Cosmos DB SQL Container Resource nesnesi |
| eventTimestamp |string| Bu kapsayıcı olayının gerçekleştiği zaman. |
| operationType |[OperationType](#operationtype)| Bu kapsayıcı olayının işlem türü. |
| OwnerId |string| SQL kapsayıcısının adı. |
| Ownerresourceıd |string| SQL kapsayıcısının kaynak KIMLIĞI. |

### <a name="restorablesqlcontainergetresult"></a><a id="restorablesqlcontainergetresult"></a>Restorayblesqlcontainergetresult

Azure Cosmos DB bir SQL kapsayıcı olayı

| **Ad** | **Tür** | **Açıklama** |
| --- | --- | ---
| ID |string| Azure Resource Manager kaynağının benzersiz kaynak tanımlayıcısı. |
| name |string| Azure Resource Manager kaynağının adı. |
| properties |[Restorayblesqlcontainerproperties](#restorablesqlcontainerproperties)| Bir SQL kapsayıcı olayının özellikleri. |
| tür |string| Azure kaynağının türü. |

### <a name="restorablesqlcontainerproperties"></a><a id="restorablesqlcontainerproperties"></a>Restorayblesqlcontainerproperties

Azure Cosmos DB SQL kapsayıcı olayının özellikleri

| **Ad** | **Tür** | **Açıklama** |
| --- | --- | --- |
| kaynak |[Kaynak](#resource)| Azure Cosmos DB SQL kapsayıcı olayının kaynağı |

### <a name="restorablesqlcontainerslistresult"></a><a id="restorablesqlcontainerslistresult"></a>Restorayblesqlcontainerslistresult

SQL kapsayıcı olaylarını ve bunların özelliklerini içeren liste işlemi yanıtı.

| **Ad** | **Tür** | **Açıklama** |
| --- | --- | --- |
| değer |[Restorayblesqlcontainergetresult](#restorablesqlcontainergetresult)[]| SQL Container olaylarının ve bunların özelliklerinin listesi. |

## <a name="next-steps"></a>Sonraki adımlar

* REST API kullanarak MongoDB için Azure Cosmos DB API 'sinde geri [yüklenebilen veritabanlarını listeleyin](restorable-mongodb-databases-list.md) .
* Sürekli yedekleme modunun [kaynak modeli](continuous-backup-restore-resource-model.md) .