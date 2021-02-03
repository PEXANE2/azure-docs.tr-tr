---
title: Azure Cosmos DB zaman noktası geri yükleme özelliği için kaynak modeli.
description: Bu makalede Azure Cosmos DB zaman noktası geri yükleme özelliği için kaynak modeli açıklanmaktadır. SQL ve MongoDB hesapları için Azure Cosmos DB API 'sinde geri yüklenebileceği sürekli yedeklemeyi ve kaynakları destekleyen parametreleri açıklar.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 02/01/2021
ms.author: govindk
ms.reviewer: sngun
ms.openlocfilehash: 64033182356e66d6a69bd47c1780b7081416019e
ms.sourcegitcommit: ea822acf5b7141d26a3776d7ed59630bf7ac9532
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2021
ms.locfileid: "99527811"
---
# <a name="resource-model-for-the-azure-cosmos-db-point-in-time-restore-feature"></a>Azure Cosmos DB zaman noktası geri yükleme özelliği için kaynak modeli
[!INCLUDE[appliesto-sql-mongodb-api](includes/appliesto-sql-mongodb-api.md)]

Bu makalede Azure Cosmos DB zaman noktası geri yükleme özelliği için kaynak modeli açıklanmaktadır. SQL ve MongoDB hesapları için Azure Cosmos DB API 'sinde geri yüklenebileceği sürekli yedeklemeyi ve kaynakları destekleyen parametreleri açıklar.

## <a name="database-accounts-resource-model"></a>Veritabanı hesabının kaynak modeli

Veritabanı hesabının kaynak modeli, yeni geri yükleme senaryolarını desteklemek için bazı ekstra özelliklerle güncelleştirilir. Bu özellikler **Backuppolicy, CreateMode ve RestoreParameters.**

### <a name="backuppolicy"></a>BackupPolicy

"Backuppolicy" parametresi altında "Type" adlı hesap düzeyinde yedekleme ilkesinde yeni bir özellik, sürekli yedekleme ve zaman içinde geri yükleme işlevlerini mümkün bir şekilde sunar. Bu moda **sürekli yedekleme** adı verilir. Genel önizlemede, hesabı oluştururken yalnızca bu modu ayarlayabilirsiniz. Etkinleştirildikten sonra, bu hesap içinde oluşturulan tüm kapsayıcılar ve veritabanları, varsayılan olarak etkinleştirilen sürekli yedekleme ve zaman içinde geri yükleme işlevlerine sahip olur.

> [!NOTE]
> Şu anda bir noktadan noktaya geri yükleme özelliği genel önizlemede ve MongoDB için Azure Cosmos DB API ve SQL hesapları için kullanılabilir. Sürekli moda sahip bir hesap oluşturduktan sonra, düzenli moda geçiş yapamazsınız.

### <a name="createmode"></a>CreateMode

Bu özellik, hesabın nasıl oluşturulduğunu gösterir. Olası değerler şunlardır "varsayılan" ve "geri yükle". Geri yükleme gerçekleştirmek için, bu değeri "geri yükle" olarak ayarlayın ve özellikte uygun değerleri sağlayın `RestoreParameters` .

### <a name="restoreparameters"></a>RestoreParameters

`RestoreParameters`Kaynak, hesap kimliği, geri yüklenecek süre ve geri yüklenmesi gereken kaynaklar dahil geri yükleme işlemi ayrıntılarını içerir.

|Özellik Adı |Description  |
|---------|---------|
|restoreMode  | Geri yükleme modu "PointInTime" olmalıdır |
|restoreSource   |  Geri yüklemenin başlatılacağı kaynak hesabın InstanceId 'si.       |
|Restoretimestamp ınutc  | Hesabın geri yüklenmesi gereken UTC olarak zaman. |
|databasesToRestore   | `DatabaseRestoreSource`Hangi veritabanlarının ve kapsayıcıların geri yüklenmesi gerektiğini belirten nesne listesi. Bu değer boşsa, tüm hesap geri yüklenir.   |

**DatabaseRestoreResource** -her kaynak tek bir veritabanını ve bu veritabanı altındaki tüm koleksiyonları temsil eder.

|Özellik Adı |Description  |
|---------|---------|
|Dosyasında | Veritabanının adı |
| collectionNames| Bu veritabanı kapsamındaki kapsayıcıların listesi |

### <a name="sample-resource"></a>Örnek kaynak

Aşağıdaki JSON, sürekli yedeklemenin etkinleştirildiği bir örnek veritabanı hesabı kaynağıdır:

```json
{
  "location": "westus",
  "properties": {
    "databaseAccountOfferType": "Standard",
    "locations": [
      {
        "failoverPriority": 0,
        "locationName": "southcentralus",
        "isZoneRedundant": false
      }
    ],
    "createMode": "Restore",
    "restoreParameters": {
      "restoreMode": "PointInTime",
      "restoreSource": "/subscriptions/subid/providers/Microsoft.DocumentDB/locations/westus/restorableDatabaseAccounts/1a97b4bb-f6a0-430e-ade1-638d781830cc",
      "restoreTimestampInUtc": "2020-06-11T22:05:09Z",
      "databasesToRestore": [
        {
          "databaseName": "db1",
          "collectionNames": [
            "collection1",
            "collection2"
          ]
        },
        {
          "databaseName": "db2",
          "collectionNames": [
            "collection3",
            "collection4"
          ]
        }
      ]
    },
    "backupPolicy": {
      "type": "Continuous"
    },
}
}
```

## <a name="restorable-resources"></a>Geri yüklenebilen kaynaklar

Bir dizi yeni kaynak ve API 'Ler, geri yüklenebilecek kaynaklarla ilgili kritik bilgileri, geri yüklenebilecekleri konumları ve bu kaynaklarda önemli işlemler gerçekleştirildiğinde zaman damgalarını bulmanıza yardımcı olmak için kullanılabilir.

> [!NOTE]
> Bu kaynakları numaralandırmak için kullanılan tüm API 'leri aşağıdaki izinleri gerektirir:
> * `Microsoft.DocumentDB/locations/restorableDatabaseAccounts/*/read`
> * `Microsoft.DocumentDB/locations/restorableDatabaseAccounts/read`

### <a name="restorable-database-account"></a>Geri yüklenebilen veritabanı hesabı

Bu kaynak, geri yüklenemeyebilecek bir veritabanı hesabı örneği içeriyor. Veritabanı hesabı silinmiş ya da canlı bir hesap olabilir. Geri yüklemek istediğiniz kaynak veritabanı hesabını bulmanıza olanak sağlayan bilgiler içerir.

|Özellik Adı |Description  |
|---------|---------|
| ID | Kaynağın benzersiz tanımlayıcısı. |
| accountName | Genel veritabanı hesabı adı. |
| creationTime | Hesap oluşturulduğunda UTC olarak zaman.  |
| deletionTime | Hesap silindiğinde UTC olarak zaman.  Hesap canlı ise bu değer boştur. |
| Apitürü | Azure Cosmos DB hesabının API türü. |
| Restokıblelocations | Hesabın varolduğu konumların listesi. |
| Restokıblelocations: locationName | Bölgesel hesabın bölge adı. |
| Restokıblelocations: regionalDatabaseAccountInstanceI | Bölgesel hesabın GUID 'SI. |
| Restorat konumları: creationTime | Bölgesel hesabın oluşturulduğu saat (UTC).|
| Restorat konumları: deletionTime | Bölgesel hesap silindiğinde UTC olarak zaman. Bölgesel hesap canlı ise bu değer boştur.|

Geri yüklenebilen tüm hesapların bir listesini almak için bkz. yeniden yüklenebilen [veritabanı hesapları-liste](restorable-database-accounts-list.md) veya geri yüklenebilen [veritabanı hesapları-konuma göre liste](restorable-database-accounts-list-by-location.md) makaleleri.

### <a name="restorable-sql-database"></a>Geri yüklenebilen SQL veritabanı

Her kaynak, SQL veritabanında oluşan oluşturma ve silme gibi bir mutasyon olayının bilgilerini içerir. Bu bilgiler, veritabanının yanlışlıkla silindiği ve bu olayın ne zaman meydana geldiğini bulmanız gereken senaryolarda yardımcı olabilir.

|Özellik Adı |Description  |
|---------|---------|
| eventTimestamp | Veritabanı oluşturulduğunda veya silindiğinde UTC olarak zaman. |
| OwnerId | SQL veritabanının adı. |
| Ownerresourceıd | SQL veritabanının kaynak KIMLIĞI|
| operationType | Bu veritabanı olayının işlem türü. Olası değerler şunlardır:<br/><ul><li>Oluştur: veritabanı oluşturma olayı</li><li>Sil: veritabanı silme olayı</li><li>Değiştir: veritabanı değiştirme olayı</li><li>SystemOperation: sistem tarafından tetiklenen veritabanı değiştirme olayı. Bu olay kullanıcı tarafından başlatılmaz</li></ul> |
| database |Olay sırasında SQL veritabanının özellikleri|

Tüm veritabanı mutasyonlarının bir listesini almak için bkz. geri [yüklenebilen SQL veritabanları-liste](restorable-sql-databases-list.md) makalesi.

### <a name="restorable-sql-container"></a>Geri yüklenebilen SQL kapsayıcısı

Her kaynak, SQL kapsayıcısında oluşan oluşturma ve silme gibi bir mutasyon olayının bilgilerini içerir. Bu bilgiler kapsayıcının değiştirildiği veya silindiği senaryolarda ve bu olayın ne zaman meydana geldiğini bulmanız gerektiğinde yardımcı olabilir.

|Özellik Adı |Description  |
|---------|---------|
| eventTimestamp    | Bu kapsayıcı olay meydana geldiğinde UTC olarak zaman.|
| OwnerId| SQL kapsayıcısının adı.|
| Ownerresourceıd   | SQL kapsayıcısının kaynak KIMLIĞI.|
| operationType | Bu kapsayıcı olayının işlem türü. Olası değerler şunlardır: <br/><ul><li>Oluşturma: kapsayıcı oluşturma olayı</li><li>Sil: kapsayıcı silme olayı</li><li>Değiştir: kapsayıcı değiştirme olayı</li><li>SystemOperation: sistem tarafından tetiklenen kapsayıcı değiştirme olayı. Bu olay kullanıcı tarafından başlatılmaz</li></ul> |
| kapsayıcı | Olay sırasında SQL kapsayıcısının özellikleri.|

Aynı veritabanı altındaki tüm kapsayıcı mutasyonlarının bir listesini almak için, bkz. yeniden yüklenebilen [SQL kapsayıcıları-liste](restorable-sql-containers-list.md) makalesi.

### <a name="restorable-sql-resources"></a>Geri yüklenebilen SQL kaynakları

Her kaynak tek bir veritabanını ve bu veritabanı altındaki tüm kapsayıcıları temsil eder.

|Özellik Adı |Description  |
|---------|---------|
| Dosyasında  | SQL veritabanının adı.
| collectionNames   | Bu veritabanı altındaki SQL kapsayıcıları listesi.|

Hesapta verilen zaman damgasında ve konumda bulunan SQL veritabanı ve kapsayıcı Birleşik giriş listesinin bir listesini almak için, bkz. geri [yüklenebilen SQL kaynakları-liste](restorable-sql-resources-list.md) makalesi.

### <a name="restorable-mongodb-database"></a>Yeniden yüklenebilen MongoDB veritabanı

Her kaynak, MongoDB veritabanında oluşan oluşturma ve silme gibi bir mutasyon olayının bilgilerini içerir. Bu bilgiler, veritabanının yanlışlıkla silindiği ve kullanıcının bu olayın ne zaman meydana geldiğini bulması gereken senaryoda yardımcı olabilir.

|Özellik Adı |Description  |
|---------|---------|
|eventTimestamp| Bu veritabanı olayının gerçekleştiği UTC süresi.|
| OwnerId| MongoDB veritabanının adı. |
| Ownerresourceıd   | MongoDB veritabanının kaynak KIMLIĞI. |
| operationType |   Bu veritabanı olayının işlem türü. Olası değerler şunlardır:<br/><ul><li> Oluştur: veritabanı oluşturma olayı</li><li> Sil: veritabanı silme olayı</li><li> Değiştir: veritabanı değiştirme olayı</li><li> SystemOperation: sistem tarafından tetiklenen veritabanı değiştirme olayı. Bu olay kullanıcı tarafından başlatılmaz </li></ul> |

Tüm veritabanı mutasyonlarının bir listesini almak için bkz. [Restorestomongodb veritabanları-liste](restorable-mongodb-databases-list.md) makalesi.

### <a name="restorable-mongodb-collection"></a>Yeniden yüklenebilen MongoDB koleksiyonu

Her kaynak, MongoDB koleksiyonunda oluşan oluşturma ve silme gibi bir mutasyon olayının bilgilerini içerir. Bu bilgiler, koleksiyonun değiştirildiği veya silindiği senaryolarda, kullanıcının bu olayın ne zaman meydana geldiğini bulması için yardımcı olabilir.

|Özellik Adı |Description  |
|---------|---------|
| eventTimestamp |Bu koleksiyon olayının gerçekleştiği UTC süresi. |
| OwnerId| MongoDB koleksiyonunun adı. |
| Ownerresourceıd   | MongoDB koleksiyonunun kaynak KIMLIĞI. |
| operationType |Bu koleksiyon olayının işlem türü. Olası değerler şunlardır:<br/><ul><li>Oluştur: koleksiyon oluşturma olayı</li><li>Sil: koleksiyon silme olayı</li><li>Değiştir: koleksiyon değiştirme olayı</li><li>SystemOperation: sistem tarafından tetiklenen koleksiyon değiştirme olayı. Bu olay kullanıcı tarafından başlatılmaz</li></ul> |

Aynı veritabanı altındaki tüm kapsayıcı mutasyonların listesini almak için, bkz. [Restorestomongodb Collections-List](restorable-mongodb-collections-list.md) article.

### <a name="restorable-mongodb-resources"></a>Yeniden yüklenebilen MongoDB kaynakları

Her kaynak tek bir veritabanını ve bu veritabanı altındaki tüm koleksiyonları temsil eder.

|Özellik Adı |Description  |
|---------|---------|
| Dosyasında  |MongoDB veritabanının adı. |
| collectionNames | Bu veritabanı altındaki MongoDB koleksiyonlarının listesi. |

Hesapta verilen zaman damgasında ve konumda bulunan tüm MongoDB veritabanı ve koleksiyon birleşimlerinin listesini almak için bkz. [Restorestomongodb Resources-List](restorable-mongodb-resources-list.md) makalesi.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Portal](continuous-backup-restore-portal.md), [PowerShell](continuous-backup-restore-powershell.md), [CLI](continuous-backup-restore-command-line.md)veya [Azure Resource Manager](continuous-backup-restore-template.md)kullanarak sürekli yedeklemeyi yapılandırın ve yönetin.
* Sürekli yedekleme moduyla verileri geri yüklemek için gereken [Izinleri yönetin](continuous-backup-restore-permissions.md) .
