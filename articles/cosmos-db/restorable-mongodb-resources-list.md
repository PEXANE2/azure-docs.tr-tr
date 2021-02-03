---
title: REST API kullanarak MongoDB için Azure Cosmos DB API 'sinde geri yüklenebilen kaynakları listeleme
description: Hesapta verilen zaman damgasından ve konumda bulunan veritabanı ve koleksiyon Birleşik giriş listesinin bir listesini döndürür. Bu, verilen zaman damgasında ve konumunda hangi kaynakların mevcut olduğunu doğrulamaya yönelik senaryolara yardımcı olur.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 02/03/2021
ms.author: govindk
ms.openlocfilehash: dc90bfb6325831276b3c6171b73aebfa2877cf68
ms.sourcegitcommit: ea822acf5b7141d26a3776d7ed59630bf7ac9532
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2021
ms.locfileid: "99527924"
---
# <a name="list-restorable-resources-in-azure-cosmos-db-api-for-mongodb-using-rest-api"></a>REST API kullanarak MongoDB için Azure Cosmos DB API 'sinde geri yüklenebilen kaynakları listeleme

Hesapta verilen zaman damgasından ve konumda bulunan veritabanı ve koleksiyon Birleşik giriş listesinin bir listesini döndürür. Bu, verilen zaman damgasında ve konumunda hangi kaynakların mevcut olduğunu doğrulamaya yönelik senaryolara yardımcı olur. Bu API için `Microsoft.DocumentDB/locations/restorableDatabaseAccounts/*/read` izin gerekir.

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.DocumentDB/locations/{location}/restorableDatabaseAccounts/{instanceId}/restorableMongodbResources?api-version=2020-06-01-preview
```

İsteğe bağlı parametrelerle:

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.DocumentDB/locations/{location}/restorableDatabaseAccounts/{instanceId}/restorableMongodbResources?api-version=2020-06-01-preview&amp;restoreLocation={restoreLocation}&amp;restoreTimestampInUtc={restoreTimestampInUtc}
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
| 200 TAMAM | [Restooyblemongodbresourceslistresult](#restorablemongodbresourceslistresult)| İşlem başarıyla tamamlandı. |
| Diğer durum kodları | [DefaultErrorResponse](#defaulterrorresponse)| İşlemin neden başarısız olduğunu açıklayan hata yanıtı. |


## <a name="examples"></a>Örnekler

### <a name="cosmosdbrestorablemongodbresourcelist"></a>Cosmosdbrestorayblemongodbresourcelist

**Örnek istek**

```http
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.DocumentDB/locations/WestUS/restorableDatabaseAccounts/d9b26648-2f53-4541-b3d8-3044f4f9810d/restorableMongodbResources?api-version=2020-06-01-preview&amp;restoreLocation=WestUS&amp;restoreTimestampInUtc=10/13/2020 4:56
```

**Örnek yanıt**

Durum kodu: 200

```json
{
  "value": [
    {
      "databaseName": "Database1",
      "collectionNames": [
        "Collection1"
      ]
    },
    {
      "databaseName": "Database2",
      "collectionNames": [
        "Collection1",
        "Collection2"
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

| Tanım | Açıklama | | --- || --- | | [DatabaseRestoreResource](#databaserestoreresource) | Geri yüklenecek belirli veritabanları. | | [Defaulterrorresponse](#defaulterrorresponse) | Hizmetten bir hata yanıtı. | | [ErrorResponse](#errorresponse) | Hata yanıtı. | | [Restorayblemongodbresourceslistresult](#restorablemongodbresourceslistresult) | Geri yüklenebilen SQL kaynaklarını içeren liste işlemi yanıtı. |

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

### <a name="restorablemongodbresourceslistresult"></a><a id="restorablemongodbresourceslistresult"></a>Restooyblemongodbresourceslistresult

MongoDB kaynakları için yeniden yüklenebilen Azure Cosmos DB API 'sini içeren, işlem yanıtı listeleyin.

| **Ad** | **Tür** | **Açıklama** |
| --- | --- | --- |
| değer |[DatabaseRestoreResource](#databaserestoreresource)[]| MongoDB kaynakları için veritabanı ve koleksiyon adları dahil, yeniden yüklenebilen Azure Cosmos DB API listesi. |

## <a name="next-steps"></a>Sonraki adımlar

* REST API kullanarak MongoDB için Azure Cosmos DB API 'sinde geri [yüklenebilen veritabanlarını listeleyin](restorable-mongodb-databases-list.md) .
* Sürekli yedekleme modunun [kaynak modeli](continuous-backup-restore-resource-model.md) .