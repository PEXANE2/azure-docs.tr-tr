---
title: REST API kullanarak MongoDB için Azure Cosmos DB API 'sinde geri yüklenebilen veritabanlarını listeleme
description: Geri yüklenebilen hesap altındaki tüm Azure Cosmos DB MongoDB veritabanlarında yapılan tüm mutasyonların olay akışını görüntüleyin. Bu, silme süresini almak için veritabanının yanlışlıkla silindiği senaryoya yardımcı olur.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 02/03/2021
ms.author: govindk
ms.openlocfilehash: 5ebb0a23822074f61a16bf1d7652dba589399542
ms.sourcegitcommit: 44188608edfdff861cc7e8f611694dec79b9ac7d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2021
ms.locfileid: "99537519"
---
# <a name="list-restorable-databases-in-azure-cosmos-db-api-for-mongodb-using-rest-api"></a>REST API kullanarak MongoDB için Azure Cosmos DB API 'sinde geri yüklenebilen veritabanlarını listeleme

> [!IMPORTANT]
> Azure Cosmos DB için zaman noktası geri yükleme özelliği (sürekli yedekleme modu) Şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Geri yüklenebilen hesap altındaki tüm Azure Cosmos DB MongoDB veritabanlarında yapılan tüm mutasyonların olay akışını görüntüleyin. Bu, silme süresini almak için veritabanının yanlışlıkla silindiği senaryoya yardımcı olur. Bu API için `Microsoft.DocumentDB/locations/restorableDatabaseAccounts/*/read` izin gerekiyor

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.DocumentDB/locations/{location}/restorableDatabaseAccounts/{instanceId}/restorableMongodbDatabases?api-version=2020-06-01-preview
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
| 200 TAMAM | [RestorableMongodbDatabasesListResult](#restorablemongodbdatabaseslistresult)| İşlem başarıyla tamamlandı. |
| Diğer durum kodları | [DefaultErrorResponse](#defaulterrorresponse)| İşlemin neden başarısız olduğunu açıklayan hata yanıtı.|

## <a name="examples"></a>Örnekler

### <a name="cosmosdbrestorablemongodbdatabaselist"></a>Cosmosdbrestorayblemongodbdatabaselist

**Örnek istek**

```http
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.DocumentDB/locations/WestUS/restorableDatabaseAccounts/d9b26648-2f53-4541-b3d8-3044f4f9810d/restorableMongodbDatabases?api-version=2020-06-01-preview
```

**Örnek yanıt**

Durum kodu: 200

```json
{
  "value": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.DocumentDb/locations/westus/restorableDatabaseAccounts/36f09704-6be3-4f33-aa05-17b73e504c75/restorableMongodbDatabases/59c21367-b98b-4a8e-abb7-b6f46600decc",
      "type": "Microsoft.DocumentDB/locations/restorableDatabaseAccounts/restorableMongodbDatabases",
      "name": "59c21367-b98b-4a8e-abb7-b6f46600decc",
      "properties": {
        "resource": {
          "_rid": "DLB14gAAAA==",
          "eventTimestamp": "2020-09-02T19:45:03Z",
          "ownerId": "Database1",
          "ownerResourceId": "PD5DALigDgw=",
          "operationType": "Create"
        }
      }
    },
    {
      "id": "/subscriptions/2296c272-5d55-40d9-bc05-4d56dc2d7588/providers/Microsoft.DocumentDb/locations/westus/restorableDatabaseAccounts/d9b26648-2f53-4541-b3d8-3044f4f9810d/restorableMongodbDatabases/8456cb17-cdb0-4c6a-8db8-d0ff3f886257",
      "type": "Microsoft.DocumentDB/locations/restorableDatabaseAccounts/restorableMongodbDatabases",
      "name": "8456cb17-cdb0-4c6a-8db8-d0ff3f886257",
      "properties": {
        "resource": {
          "_rid": "ESXNLAAAAA==",
          "eventTimestamp": "2020-09-02T19:53:42Z",
          "ownerId": "Database1",
          "ownerResourceId": "PD5DALigDgw=",
          "operationType": "Delete"
        }
      }
    }
  ]
}
```

## <a name="definitions"></a>Tanımlar

| Tanım | Açıklama | | --- || --- | | [Defaulterrorresponse](#defaulterrorresponse) | Hizmetten bir hata yanıtı. | | [ErrorResponse](#errorresponse) | Hata yanıtı. | | [OperationType](#operationtype) | Etkinliğin işlem türünü belirtmek için sabit listesi. | | [Kaynak](#resource) | MongoDB veritabanı olayı için bir Azure Cosmos DB API 'SI kaynağı | | [Restooyblemongodbdatabasegetresult](#restorablemongodbdatabasegetresult) | MongoDB veritabanı olayı için Azure Cosmos DB API 'SI | | [Restorampabblemongodbdatabaseproperties](#restorablemongodbdatabaseproperties) | MongoDB veritabanı olayı için Azure Cosmos DB API 'sinin özellikleri | | [RestorableMongodbDatabasesListResult](#restorablemongodbdatabaseslistresult) | MongoDB veritabanı olayları ve özellikleri için Azure Cosmos DB API 'sini içeren, işlem yanıtı listeleyin. |

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

### <a name="resource"></a><a id="resource"></a>Kaynak

MongoDB veritabanı olayı için Azure Cosmos DB API 'sinin kaynağı

| **Ad** | **Tür** | **Açıklama** |
| --- | --- | --- |
| _rid |string| Sistem tarafından oluşturulan bir özellik. Benzersiz bir tanımlayıcı. |
| eventTimestamp |string| Bu veritabanı olayının gerçekleştiği zaman. |
| operationType |[OperationType](#operationtype)| Bu veritabanı olayının işlem türü.  |
| OwnerId |string| MongoDB veritabanı için Azure Cosmos DB API 'sinin adı.|
| Ownerresourceıd |string| MongoDB veritabanı için Azure Cosmos DB kaynak KIMLIĞI. |

### <a name="restorablemongodbdatabasegetresult"></a><a id="restorablemongodbdatabasegetresult"></a>Restooyblemongodbdatabasegetresult

MongoDB veritabanı olayı için Azure Cosmos DB API 'SI

| **Ad** | **Tür** | **Açıklama** |
| --- | --- | --- |
| ID |string| Azure Resource Manager kaynağının benzersiz kaynak tanımlayıcısı. |
| name |string| Kaynak Yöneticisi kaynağının adı. |
| properties |[Restooyblemongodbdatabaseproperties](#restorablemongodbdatabaseproperties)| MongoDB veritabanı olayı için Azure Cosmos DB API 'sinin özellikleri. |
| tür |string| Azure kaynağının türü. |

### <a name="restorablemongodbdatabaseproperties"></a><a id="restorablemongodbdatabaseproperties"></a>Restooyblemongodbdatabaseproperties

MongoDB veritabanı olayı için Azure Cosmos DB API 'sinin özellikleri

| **Ad** | **Tür** | **Açıklama** |
| --- | --- | --- |
| kaynak |[Kaynak](#resource)| MongoDB veritabanı olayı için Azure Cosmos DB API 'sinin kaynağı |

### <a name="restorablemongodbdatabaseslistresult"></a><a id="restorablemongodbdatabaseslistresult"></a>RestorableMongodbDatabasesListResult

Veritabanı olaylarını ve bunların özelliklerini içeren liste işlem yanıtı.

| **Ad** | **Tür** | **Açıklama** |
| --- | --- | --- |
| değer |[Restorampabblemongodbdatabasegetresult](#restorablemongodbdatabasegetresult)[]| Veritabanı olaylarının ve bunların özelliklerinin listesi. |

## <a name="next-steps"></a>Sonraki adımlar

* REST API kullanarak MongoDB için Azure Cosmos DB API 'sinde geri [yüklenebilen kaynakları listeleyin](restorable-mongodb-resources-list.md) .
* REST API kullanarak MongoDB için Azure Cosmos DB API 'sinde geri [yüklenebilen koleksiyonları listeleyin](restorable-mongodb-collections-list.md) .
* Sürekli yedekleme modunun [kaynak modeli](continuous-backup-restore-resource-model.md) .