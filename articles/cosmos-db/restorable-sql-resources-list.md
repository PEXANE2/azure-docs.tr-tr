---
title: REST API kullanarak Azure Cosmos DB geri yüklenebilen SQL API kaynaklarını listeleme
description: Belirtilen zaman damgasında ve konumda hesapta bulunan veritabanı ve kapsayıcı Birleşik giriş listesini döndürür. Bu, verilen zaman damgasında ve konumunda hangi kaynakların mevcut olduğunu doğrulamaya yönelik senaryolara yardımcı olur.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 02/03/2021
ms.author: govindk
ms.openlocfilehash: dbee87098dcc712669c332deac656cf5656ef4c4
ms.sourcegitcommit: ea822acf5b7141d26a3776d7ed59630bf7ac9532
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2021
ms.locfileid: "99527770"
---
# <a name="list-restorable-sql-api-resources-in-azure-cosmos-db-using-rest-api"></a>REST API kullanarak Azure Cosmos DB geri yüklenebilen SQL API kaynaklarını listeleme

Belirtilen zaman damgasında ve konumda hesapta bulunan veritabanı ve kapsayıcı Birleşik giriş listesini döndürür. Bu, verilen zaman damgasında ve konumunda hangi kaynakların mevcut olduğunu doğrulamaya yönelik senaryolara yardımcı olur. Bu API için `Microsoft.DocumentDB/locations/restorableDatabaseAccounts/*/read` izin gerekir.

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.DocumentDB/locations/{location}/restorableDatabaseAccounts/{instanceId}/restorableSqlResources?api-version=2020-06-01-preview
```

İsteğe bağlı parametrelerle:

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.DocumentDB/locations/{location}/restorableDatabaseAccounts/{instanceId}/restorableSqlResources?api-version=2020-06-01-preview&amp;restoreLocation={restoreLocation}&amp;restoreTimestampInUtc={restoreTimestampInUtc}
```

## <a name="uri-parameters"></a>URI parametreleri

| Name | İçinde | Gerekli | Tür | Description |
| --- | --- | --- | --- | --- |
| **InstanceId** | path | True |string| Geri yüklenebilen bir veritabanı hesabının InstanceId GUID 'SI. |
| **konumuna** | path | True | string| Azure Cosmos DB bölge, sözcükler ve her sözcük için büyük harfler arasında boşluk. |
| **subscriptionId** | path | True | string| Hedef aboneliğin KIMLIĞI. |
| **api sürümü** | sorgu | True | string | Bu işlem için kullanılacak API sürümü. |
| **restoreLocation** | sorgu | |string| Geri yüklenebilen kaynakların bulunduğu konum. |
| **Restoretimestamp ınutc** | sorgu | |string| Geri yüklenebilen kaynaklar var olduğunda zaman damgası. |

## <a name="responses"></a>Yanıtlar

| Ad | Tür | Description |
| --- | --- | --- |
| 200 TAMAM | [Restorayblesqlresourceslistresult](#restorablesqlresourceslistresult)| İşlem başarıyla tamamlandı. |
| Diğer durum kodları | [DefaultErrorResponse](#defaulterrorresponse)| İşlemin neden başarısız olduğunu açıklayan hata yanıtı. |

## <a name="examples"></a>Örnekler

### <a name="cosmosdbrestorablesqlresourcelist"></a>Cosmosdbrestorayblesqlresourcelist

**Örnek istek**

```http
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.DocumentDB/locations/WestUS/restorableDatabaseAccounts/d9b26648-2f53-4541-b3d8-3044f4f9810d/restorableSqlResources?api-version=2020-06-01-preview&amp;restoreLocation=WestUS&amp;restoreTimestampInUtc=10/13/2020 4:56
```

**Örnek yanıt**

Durum kodu: 200

```json
{
  "value": [
    {
      "databaseName": "Database1",
      "collectionNames": [
        "Container1"
      ]
    },
    {
      "databaseName": "Database2",
      "collectionNames": [
        "Container1",
        "Container2"
      ]
    },
    {
      "databaseName": "Database3",
      "collectionNames": []
    }
  ]
}
```

## <a name="definitions"></a>Tanımlar

| Tanım | Açıklama | | --- || --- | | [DatabaseRestoreResource](#databaserestoreresource) | Geri yüklenecek belirli veritabanları. | | [Defaulterrorresponse](#defaulterrorresponse) | Hizmetten bir hata yanıtı. | | [ErrorResponse](#errorresponse) | Hata yanıtı. | | [Restorayblesqlresourceslistresult](#restorablesqlresourceslistresult) | Geri yüklenebilen SQL kaynaklarını içeren liste işlemi yanıtı. |

### <a name="databaserestoreresource"></a><a id="databaserestoreresource"></a>DatabaseRestoreResource

Geri yüklenecek belirli veritabanları.

| **Ad** | **Tür** | **Açıklama** |
| --- | --- | --- |
| collectionNames |String []| Geri yükleme için kullanılabilen koleksiyonların adları. |
| Dosyasında |string| Geri yükleme için kullanılabilen veritabanının adı. |

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

### <a name="restorablesqlresourceslistresult"></a><a id="restorablesqlresourceslistresult"></a>Restorayblesqlresourceslistresult

Geri yüklenebilen SQL kaynaklarını içeren liste işlemi yanıtı.

| **Ad** | **Tür** | **Açıklama** |
| --- | --- | --- |
| değer |[DatabaseRestoreResource](#databaserestoreresource)[]| Veritabanı ve koleksiyon adları dahil, geri yüklenebilen SQL kaynaklarının listesi. |

## <a name="next-steps"></a>Sonraki adımlar

* REST API kullanarak Azure Cosmos SQL API 'sindeki geri [yüklenebilen veritabanlarını listeleyin](restorable-sql-databases-list.md) .
* Sürekli yedekleme modunun [kaynak modeli](continuous-backup-restore-resource-model.md) .