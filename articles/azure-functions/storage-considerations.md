---
title: Azure Işlevleri için depolama konuları
description: Azure Işlevlerinin depolama gereksinimlerini ve depolanan verileri şifreleme hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 01/21/2020
ms.openlocfilehash: f094996ca44ec36d46330e54eac56b28794ef22e
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78358174"
---
# <a name="storage-considerations-for-azure-functions"></a>Azure Işlevleri için depolama konuları

Bir işlev uygulaması örneği oluşturduğunuzda Azure Işlevleri bir Azure depolama hesabı gerektirir. Aşağıdaki depolama hizmetleri, işlev uygulamanız tarafından kullanılabilir:


|Depolama hizmeti  | İşlev kullanımı  |
|---------|---------|
| [Azure Blob Depolama](../storage/blobs/storage-blobs-introduction.md)     | Bağlama durumu ve işlev anahtarlarını koruyun.  <br/>Ayrıca [, dayanıklı işlevler görev hub 'ları](durable/durable-functions-task-hubs.md)tarafından kullanılır. |
| [Azure Dosyalar](../storage/files/storage-files-introduction.md)  | İşlev uygulaması kodunuzu bir [Tüketim planında](functions-scale.md#consumption-plan)depolamak ve çalıştırmak için kullanılan dosya paylaşma. |
| [Azure kuyruk depolama](../storage/queues/storage-queues-introduction.md)     | [Dayanıklı işlevler 'de görev hub 'ları](durable/durable-functions-task-hubs.md)tarafından kullanılır.   |
| [Azure Tablo Depolama](../storage/tables/table-storage-overview.md)  |  [Dayanıklı işlevler 'de görev hub 'ları](durable/durable-functions-task-hubs.md)tarafından kullanılır.       |

> [!IMPORTANT]
> Tüketim barındırma planı kullanılırken işlev kodunuz ve bağlama yapılandırma dosyalarınız ana depolama hesabındaki Azure Dosya depolama alanında saklanır. Ana depolama hesabını sildiğinizde bu içerik silinir ve kurtarılamaz.

## <a name="storage-account-requirements"></a>Depolama hesabı gereksinimleri

Bir işlev uygulaması oluştururken blob, kuyruk ve tablo depolamayı destekleyen genel amaçlı bir Azure depolama hesabı oluşturmanız veya bağlamanız gerekir. Bunun nedeni, Işlevlerin Tetikleyicileri yönetme ve işlev yürütmelerinin günlüğe kaydedilmesini gibi işlemler için Azure Storage 'ı temel aldığından oluşur. Bazı depolama hesapları kuyrukları ve tabloları desteklemez. Bu hesaplar yalnızca BLOB depolama hesapları, Azure Premium Depolama ve ZRS çoğaltması olan genel amaçlı depolama hesapları içerir. Bu desteklenmeyen hesaplar, bir işlev uygulaması oluştururken depolama hesabı dikey penceresinden filtrelenir.

Depolama hesabı türleri hakkında daha fazla bilgi edinmek için bkz. [Azure Depolama Hizmetlerine Giriş](../storage/common/storage-introduction.md#azure-storage-services). 

İşlev uygulamanızla mevcut bir depolama hesabını kullanabilirsiniz, ancak bu gereksinimleri karşıladığından emin olmanız gerekir. Bu depolama hesabı gereksinimlerini karşılamak için, işlev uygulaması oluşturma akışının bir parçası olarak oluşturulan depolama hesapları garanti edilir.  

## <a name="storage-account-guidance"></a>Depolama hesabı Kılavuzu

Her işlev uygulamasının çalışması için bir depolama hesabının olması gerekir. Bu hesap silinirse, işlev uygulamanız çalışmaz. Depolama ile ilgili sorunları gidermek için bkz. [depolama ile ilgili sorunları giderme](functions-recover-storage-account.md). Aşağıdaki ek konular, işlev uygulamaları tarafından kullanılan depolama hesabı için geçerlidir.

### <a name="storage-account-connection-setting"></a>Depolama hesabı bağlantı ayarı

Depolama hesabı bağlantısı [AzureWebJobsStorage uygulama ayarında](./functions-app-settings.md#azurewebjobsstorage)tutulur. 

Depolama anahtarlarını yeniden oluşturduğunuzda depolama hesabı bağlantı dizesinin güncellenmesi gerekir. [Depolama anahtarı yönetimi hakkında buradan daha fazla bilgi edinin](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account).

### <a name="shared-storage-accounts"></a>Paylaşılan depolama hesapları

Birden çok işlevli uygulamanın herhangi bir sorun olmadan aynı depolama hesabını paylaşması mümkündür. Örneğin, Visual Studio 'da Azure Storage öykünücüsü 'nü kullanarak birden çok uygulama geliştirebilirsiniz. Bu durumda, öykünücü tek bir depolama hesabı gibi davranır. İşlev uygulamanız tarafından kullanılan depolama hesabı, uygulama verilerinizi depolamak için de kullanılabilir. Ancak, bu yaklaşım her zaman bir üretim ortamında iyi bir fikir değildir.

### <a name="optimize-storage-performance"></a>Depolama performansını iyileştirme

[!INCLUDE [functions-shared-storage](../../includes/functions-shared-storage.md)]

## <a name="storage-data-encryption"></a>Depolama veri şifrelemesi

Azure depolama, bekleyen bir depolama hesabındaki tüm verileri şifreler. Daha fazla bilgi için bkz. [bekleyen veriler Için Azure depolama şifrelemesi](../storage/common/storage-service-encryption.md).

Varsayılan olarak, veriler Microsoft tarafından yönetilen anahtarlarla şifrelenir. Şifreleme anahtarları üzerinde ek denetim için, blob ve dosya verilerinin şifrelenmesi için kullanmak üzere müşteri tarafından yönetilen anahtarlar sağlayabilirsiniz. Bu anahtarların, depolama hesabına erişebilmeleri için Azure Key Vault ' de mevcut olması gerekir. Daha fazla bilgi için bkz. [Azure Portal kullanarak müşteri tarafından yönetilen anahtarları Azure Key Vault yapılandırma](../storage/common/storage-encryption-keys-portal.md).  

## <a name="next-steps"></a>Sonraki adımlar

Azure Işlevleri barındırma seçenekleri hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Azure İşlevleri’ni ölçeklendirme ve barındırma](functions-scale.md)


