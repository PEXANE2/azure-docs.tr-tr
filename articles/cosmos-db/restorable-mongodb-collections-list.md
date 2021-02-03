---
title: Azure Cosmos DB MongoDB API 'sinde geri yüklenebilen koleksiyonları listeleme-REST API
description: Belirli bir veritabanı altındaki tüm Azure Cosmos DB MongoDB koleksiyonlarında yapılan tüm mutasyonların olay akışını görüntüleyin. Bu, kapsayıcının yanlışlıkla silindiği senaryoya yardımcı olur.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 02/03/2021
ms.author: govindk
ms.openlocfilehash: 83e1c7c27e8c5d179c4ec6aa4ba64b3367294a48
ms.sourcegitcommit: ea822acf5b7141d26a3776d7ed59630bf7ac9532
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2021
ms.locfileid: "99527937"
---
# <a name="list-restorable-collections-in-azure-cosmos-db-api-for-mongodb-using-rest-api"></a>REST API kullanarak MongoDB için Azure Cosmos DB API 'sinde geri yüklenebilen koleksiyonları listeleme

Belirli bir veritabanı altındaki MongoDB koleksiyonları için tüm Azure Cosmos DB API 'leri üzerinde yapılan tüm mutasyonların olay akışını görüntüleyin. Bu, kapsayıcının yanlışlıkla silindiği senaryoya yardımcı olur. Bu API için `Microsoft.DocumentDB/locations/restorableDatabaseAccounts/*/read` izin gerekiyor

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.DocumentDB/locations/{location}/restorableDatabaseAccounts/{instanceId}/restorableMongodbCollections?api-version=2020-06-01-preview
```

İsteğe bağlı parametrelerle:

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.DocumentDB/locations/{location}/restorableDatabaseAccounts/{instanceId}/restorableMongodbCollections?api-version=2020-06-01-preview&amp;restorableMongodbDatabaseRid={restorableMongodbDatabaseRid}
```

## <a name="uri-parameters"></a>URI parametreleri

| Name | İçinde | Gerekli | Tür | Description |
| --- | --- | --- | --- | --- |
| **InstanceId** | path | True |string| Geri yüklenebilen bir veritabanı hesabının InstanceId GUID 'SI. |
| **konumuna** | path | True | string| Azure Cosmos DB bölge, sözcükler ve her sözcük için büyük harfler arasında boşluk. |
| **subscriptionId** | path | True | string| Hedef aboneliğin KIMLIĞI. |
| **api sürümü** | sorgu | True | string | Bu işlem için kullanılacak API sürümü. |
| **restorableMongodbDatabaseRid** | sorgu | |string| MongoDB veritabanı için Azure Cosmos DB API 'sinin kaynak KIMLIĞI. |

## <a name="responses"></a>Yanıtlar

| Ad | Tür | Description |
| --- | --- | --- |
| 200 TAMAM | [Restooyblemongodbcollectionslistresult](#restorablemongodbcollectionslistresult)| İşlem başarıyla tamamlandı. |
| Diğer durum kodları | [DefaultErrorResponse](#defaulterrorresponse)| İşlemin neden başarısız olduğunu açıklayan hata yanıtı.|

## <a name="examples"></a>Örnekler

### <a name="cosmosdbrestorablemongodbcollectionlist"></a>Cosmosdbrestooyblemongodbcollectionlist

**Örnek istek**

```http
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.DocumentDB/locations/WestUS/restorableDatabaseAccounts/98a570f2-63db-4117-91f0-366327b7b353/restorableMongodbCollections?api-version=2020-06-01-preview&amp;restorableMongodbDatabaseRid=PD5DALigDgw=
```

**Örnek yanıt**

Durum kodu: 200

```json
{
  "value": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.DocumentDb/locations/westus/restorableDatabaseAccounts/98a570f2-63db-4117-91f0-366327b7b353/restorableMongodbCollections/79609a98-3394-41f8-911f-cfab0c075c86",
      "type": "Microsoft.DocumentDB/locations/restorableDatabaseAccounts/restorableMongodbCollections",
      "name": "79609a98-3394-41f8-911f-cfab0c075c86",
      "properties": {
        "resource": {
          "_rid": "zAyAPQAAAA==",
          "eventTimestamp": "2020-10-13T04:56:42Z",
          "ownerId": "Collection1",
          "ownerResourceId": "V18LoLrv-qA=",
          "operationType": "Create"
        }
      }
    }
  ]
}
```

## <a name="definitions"></a>Tanımlar

|Tanım  | Description|
| --- | --- |
| [DefaultErrorResponse](#defaulterrorresponse) | Hizmetten bir hata yanıtı. |
| [ErrorResponse](#errorresponse) | Hata yanıtı. |
| [OperationType](#operationtype) | Etkinliğin işlem türünü belirtmek için sabit listesi. |
| [Kaynak](#resource) | MongoDB koleksiyon olayı için Azure Cosmos DB API 'sinin kaynağı |
| [Restooyblemongodbcollectiongetresult](#restorablemongodbcollectiongetresult) | MongoDB koleksiyon olayı için Azure Cosmos DB API 'SI |
| [Restooyblemongodbcollectionproperties](#restorablemongodbcollectionproperties) | MongoDB koleksiyon olayı için Azure Cosmos DB API 'sinin özellikleri |
| [Restooyblemongodbcollectionslistresult](#restorablemongodbcollectionslistresult) | Koleksiyon olaylarını ve bunların özelliklerini içeren liste işlemi yanıtı. |

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
| Oluştur |string|koleksiyon oluşturma olayı|
| Sil |string|koleksiyon silme olayı|
| Değiştir |string|koleksiyon değiştirme olayı|

### <a name="resource"></a><a id="resource"></a>Kaynak

Azure Cosmos DB MongoDB koleksiyon olayının kaynağı

| **Ad** | **Tür** | **Açıklama** |
| --- | --- | --- |
| _rid |string| Sistem tarafından oluşturulan bir özellik. Benzersiz bir tanımlayıcı. |
| eventTimestamp |string| Bu koleksiyon olayının gerçekleştiği zaman. |
| operationType |[OperationType](#operationtype)|  Bu koleksiyon olayının işlem türü. |
| OwnerId |string| MongoDB koleksiyonunun adı.|
| Ownerresourceıd |string| MongoDB koleksiyonunun kaynak KIMLIĞI. |

### <a name="restorablemongodbcollectiongetresult"></a><a id="restorablemongodbcollectiongetresult"></a>Restooyblemongodbcollectiongetresult

Azure Cosmos DB MongoDB toplama olayı

| **Ad** | **Tür** | **Açıklama** |
| --- | --- | --- |
| ID |string| Azure Resource Manager kaynağının benzersiz kaynak tanımlayıcısı. |
| name |string| Kaynak Yöneticisi kaynağının adı. |
| properties |[Restooyblemongodbcollectionproperties](#restorablemongodbcollectionproperties)| Bir koleksiyon olayının özellikleri. |
| tür |string| Azure kaynağının türü. |

### <a name="restorablemongodbcollectionproperties"></a><a id="restorablemongodbcollectionproperties"></a>Restooyblemongodbcollectionproperties

Azure Cosmos DB MongoDB koleksiyon olayının özellikleri

| **Ad** | **Tür** | **Açıklama** |
| --- | --- | --- |
| kaynak | [Kaynak](#resource)| MongoDB koleksiyon olayı için Azure Cosmos DB API 'sinin kaynağı |

### <a name="restorablemongodbcollectionslistresult"></a><a id="restorablemongodbcollectionslistresult"></a>Restooyblemongodbcollectionslistresult

Koleksiyon olaylarını ve bunların özelliklerini içeren liste işlemi yanıtı.

| **Ad** | **Tür** | **Açıklama** |
| --- | --- | --- |
| değer |[Restooyblemongodbcollectiongetresult](#restorablemongodbcollectiongetresult)[]| MongoDB koleksiyon olayları ve özellikleri için Azure Cosmos DB API 'sinin listesi. |

## <a name="next-steps"></a>Sonraki adımlar

* REST API kullanarak MongoDB için Azure Cosmos DB API 'sinde geri [yüklenebilen veritabanlarını listeleyin](restorable-mongodb-databases-list.md) .
* Sürekli yedekleme modunun [kaynak modeli](continuous-backup-restore-resource-model.md) .