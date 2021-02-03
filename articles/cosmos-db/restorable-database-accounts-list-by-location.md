---
title: Azure Cosmos DB REST API kullanarak geri yüklenebilen veritabanı hesaplarını konuma göre listeleme
description: Abonelik altında ve bir bölgede bulunan tüm geri yüklenebilen Azure Cosmos DB veritabanı hesaplarını listeler.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 02/03/2021
ms.author: govindk
ms.openlocfilehash: 2a3fbc1bb00c57c20436c19602c135f1917c6a60
ms.sourcegitcommit: ea822acf5b7141d26a3776d7ed59630bf7ac9532
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2021
ms.locfileid: "99527899"
---
# <a name="list-restorable-database-accounts-by-location-using-azure-cosmos-db-rest-api"></a>Azure Cosmos DB REST API kullanarak geri yüklenebilen veritabanı hesaplarını konuma göre listeleme

Abonelik altında ve bir bölgede bulunan tüm geri yüklenebilen Azure Cosmos DB veritabanı hesaplarını listeler. Bu çağrı için `Microsoft.DocumentDB/locations/restorableDatabaseAccounts/read` izin gerekir.

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.DocumentDB/locations/{location}/restorableDatabaseAccounts?api-version=2020-06-01-preview

```

## <a name="uri-parameters"></a>URI parametreleri

| Name | İçinde | Gerekli | Tür | Description |
| --- | --- | --- | --- | --- |
| **konumuna** | path | True | string| Azure Cosmos DB bölge, sözcükler ve her sözcük için büyük harfler arasında boşluk. |
| **subscriptionId** | path | True | string| Hedef aboneliğin KIMLIĞI. |
| **api sürümü** | sorgu | True | string | Bu işlem için kullanılacak API sürümü. |

## <a name="responses"></a>Yanıtlar

| Ad | Tür | Description |
| --- | --- | --- |
| 200 TAMAM | [Restokıbledatabaseaccountslistresult](#restorabledatabaseaccountslistresult)| İşlem başarıyla tamamlandı. |
| Diğer durum kodları | [DefaultErrorResponse](#defaulterrorresponse)| İşlemin neden başarısız olduğunu açıklayan hata yanıtı. |

## <a name="examples"></a>Örnekler

### <a name="cosmosdbdatabaseaccountlist"></a>CosmosDBDatabaseAccountList

**Örnek istek**

```http
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.DocumentDB/locations/West US/restorableDatabaseAccounts?api-version=2020-06-01-preview
```

**Örnek yanıt**

Durum kodu: 200

```json
{
  "value": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.DocumentDB/locations/West US/restorableDatabaseAccounts/d9b26648-2f53-4541-b3d8-3044f4f9810d",
      "name": "d9b26648-2f53-4541-b3d8-3044f4f9810d",
      "location": "West US",
      "type": "Microsoft.DocumentDB/locations/restorableDatabaseAccounts",
      "properties": {
        "accountName": "ddb1",
        "creationTime": "2020-04-11T21:56:15Z",
        "deletionTime": "2020-06-12T22:05:09Z",
        "apiType": "Sql",
        "restorableLocations": [
          {
            "locationName": "South Central US",
            "regionalDatabaseAccountInstanceId": "d7a01f78-606f-45c6-9dac-0df32f433bb5",
            "creationTime": "2020-10-30T21:13:10Z",
            "deletionTime": "2020-10-30T21:13:35Z"
          },
          {
            "locationName": "West US",
            "regionalDatabaseAccountInstanceId": "fdb43d84-1572-4697-b6e7-2bcda0c51b2c",
            "creationTime": "2020-10-30T21:13:10Z"
          }
        ]
      }
    },
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.DocumentDB/locations/West US/restorableDatabaseAccounts/4f9e6ace-ac7a-446c-98bc-194c502a06b4",
      "name": "4f9e6ace-ac7a-446c-98bc-194c502a06b4",
      "location": "West US",
      "type": "Microsoft.DocumentDB/locations/restorableDatabaseAccounts",
      "properties": {
        "accountName": "ddb2",
        "creationTime": "2020-05-01T08:05:18Z",
        "apiType": "Sql",
        "restorableLocations": [
          {
            "locationName": "South Central US",
            "regionalDatabaseAccountInstanceId": "d7a01f78-606f-45c6-9dac-0df32f433bb5",
            "creationTime": "2020-10-30T21:13:10Z",
            "deletionTime": "2020-10-30T21:13:35Z"
          },
          {
            "locationName": "West US",
            "regionalDatabaseAccountInstanceId": "fdb43d84-1572-4697-b6e7-2bcda0c51b2c",
            "creationTime": "2020-10-30T21:13:10Z"
          }
        ]
      }
    }
  ]
}
```

## <a name="definitions"></a>Tanımlar

|Tanım  | Description|
| --- | --- |
| [Apitürü](#apitype) | Geri yüklenebilen veritabanı hesabının API türünü belirtmek için sabit listesi. |
| [DefaultErrorResponse](#defaulterrorresponse) | Hizmetten bir hata yanıtı. |
| [ErrorResponse](#errorresponse) | Hata yanıtı. |
| [Restokıbledatabaseaccountgetresult](#restorabledatabaseaccountgetresult) | Azure Cosmos DB geri yüklenebilen bir veritabanı hesabı. |
| [Restokıbledatabaseaccountslistresult](#restorabledatabaseaccountslistresult) | Geri yüklenebilen veritabanı hesaplarını ve bunların özelliklerini içeren liste işlemi yanıtı. |
| [Restokıblelocationresource](#restorablelocationresource) | Bölgesel geri yüklenebilen hesabın özellikleri. |

### <a name="apitype"></a><a id="apitype"></a>Apitürü

Geri yüklenebilen veritabanı hesabının API türünü belirtmek için sabit listesi.

| **Ad** | **Tür** |
| --- | --- |
| Cassandra |string|
| Gremlin |string|
| GremlinV2 |string|
| MongoDB |string|
| Sql |string|
| Tablo |string|

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

### <a name="restorabledatabaseaccountgetresult"></a><a id="restorabledatabaseaccountgetresult"></a>Restokıbledatabaseaccountgetresult

Azure Cosmos DB geri yüklenebilen bir veritabanı hesabı.

| **Ad** | **Tür** | **Açıklama** |
| --- | --- | --- |
| ID |string| Azure Resource Manager kaynağının benzersiz kaynak tanımlayıcısı. |
| location |string| Kaynağın ait olduğu kaynak grubunun konumu. |
| name |string| Kaynak Yöneticisi kaynağının adı. |
| Properties. accountName |string| Genel veritabanı hesabının adı. |
| Properties. Apittype |[Apitürü](#apitype)| Geri yüklenebilen veritabanı hesabının API türü. |
| Properties. creationTime |string| Yeniden yüklenebilen veritabanı hesabının oluşturulma zamanı (ISO-8601 biçimi). |
| Özellikler. deletionTime |string| Geri yüklenebilen veritabanı hesabının silindiği zaman (ISO-8601 biçimi). |
| Properties. Restokıblelocations |[Restokıblelocationresource](#restorablelocationresource)[]| Veritabanı hesabının geri yüklenebildiği bölgelerin listesi. |
| tür |string| Azure kaynağının türü. |

### <a name="restorabledatabaseaccountslistresult"></a><a id="restorabledatabaseaccountslistresult"></a>Restokıbledatabaseaccountslistresult

Geri yüklenebilen veritabanı hesaplarını ve bunların özelliklerini içeren liste işlemi yanıtı.

| **Ad** | **Tür** | **Açıklama** |
| --- | --- | --- |
| değer |[Restooybledatabaseaccountgetresult](#restorabledatabaseaccountgetresult)[]| Geri yüklenebilen veritabanı hesaplarının ve bunların özelliklerinin listesi. |

### <a name="restorablelocationresource"></a><a id="restorablelocationresource"></a>Restokıblelocationresource

Bölgesel geri yüklenebilen hesabın özellikleri.

| **Ad** | **Tür** | **Açıklama** |
| --- | --- | --- |
| creationTime |string| Bölgesel geri yüklenebilen veritabanı hesabının Oluşturulma saati (ISO-8601 biçimi). |
| deletionTime |string| Bölgesel geri yüklenebilen veritabanı hesabının silindiği zaman (ISO-8601 biçimi). |
| locationName |string| Bölgesel geri yüklenebilen hesabın konumu. |
| regionalDatabaseAccountInstanceId |string| Bölgesel geri yüklenebilen hesabın örnek KIMLIĞI. |

## <a name="next-steps"></a>Sonraki adımlar

* REST API kullanarak MongoDB için Azure Cosmos DB API 'sinde geri [yüklenebilen koleksiyonları listeleyin](restorable-database-accounts-list-by-location.md) .
* Sürekli yedekleme modunun [kaynak modeli](continuous-backup-restore-resource-model.md) .