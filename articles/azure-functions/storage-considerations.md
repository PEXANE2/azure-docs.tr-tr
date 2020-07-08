---
title: Azure Işlevleri için depolama konuları
description: Azure Işlevlerinin depolama gereksinimlerini ve depolanan verileri şifreleme hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 01/21/2020
ms.openlocfilehash: 324516240d09a5443908cbffec514e4caba2b604
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83648774"
---
# <a name="storage-considerations-for-azure-functions"></a>Azure Işlevleri için depolama konuları

Bir işlev uygulaması örneği oluşturduğunuzda Azure Işlevleri bir Azure depolama hesabı gerektirir. Aşağıdaki depolama hizmetleri, işlev uygulamanız tarafından kullanılabilir:


|Depolama hizmeti  | İşlev kullanımı  |
|---------|---------|
| [Azure Blob Depolama](../storage/blobs/storage-blobs-introduction.md)     | Bağlama durumu ve işlev anahtarlarını koruyun.  <br/>Ayrıca [, dayanıklı işlevler görev hub 'ları](durable/durable-functions-task-hubs.md)tarafından kullanılır. |
| [Azure Dosyaları](../storage/files/storage-files-introduction.md)  | İşlev uygulaması kodunuzu bir [Tüketim planında](functions-scale.md#consumption-plan)depolamak ve çalıştırmak için kullanılan dosya paylaşma. |
| [Azure kuyruk depolama](../storage/queues/storage-queues-introduction.md)     | [Dayanıklı işlevler 'de görev hub 'ları](durable/durable-functions-task-hubs.md)tarafından kullanılır.   |
| [Azure Tablo Depolama](../storage/tables/table-storage-overview.md)  |  [Dayanıklı işlevler 'de görev hub 'ları](durable/durable-functions-task-hubs.md)tarafından kullanılır.       |

> [!IMPORTANT]
> Tüketim barındırma planı kullanılırken işlev kodunuz ve bağlama yapılandırma dosyalarınız ana depolama hesabındaki Azure Dosya depolama alanında saklanır. Ana depolama hesabını sildiğinizde bu içerik silinir ve kurtarılamaz.

## <a name="storage-account-requirements"></a>Depolama hesabı gereksinimleri

Bir işlev uygulaması oluştururken blob, kuyruk ve tablo depolamayı destekleyen genel amaçlı bir Azure depolama hesabı oluşturmanız veya bağlamanız gerekir. Bunun nedeni, Işlevlerin Tetikleyicileri yönetme ve işlev yürütmelerinin günlüğe kaydedilmesini gibi işlemler için Azure Storage 'ı temel aldığından oluşur. Bazı depolama hesapları kuyrukları ve tabloları desteklemez. Bu hesaplar yalnızca BLOB depolama hesapları, Azure Premium Depolama ve ZRS çoğaltması olan genel amaçlı depolama hesapları içerir. Bu desteklenmeyen hesaplar, bir işlev uygulaması oluştururken depolama hesabı dikey penceresinden filtrelenir.

Depolama hesabı türleri hakkında daha fazla bilgi edinmek için bkz. [Azure Depolama Hizmetlerine Giriş](../storage/common/storage-introduction.md#core-storage-services). 

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

[!INCLUDE [functions-storage-encryption](../../includes/functions-storage-encryption.md)]

## <a name="mount-file-shares-linux"></a>Bağlama dosyası paylaşımları (Linux)

Mevcut Azure dosya paylaşımlarını Linux işlev uygulamalarınıza bağlayabilirsiniz. Linux işlev uygulamanıza bir paylaşma bağlayarak, işlevlerinizde mevcut makine öğrenimi modellerini veya diğer verileri kullanabilirsiniz. [`az webapp config storage-account add`](/cli/azure/webapp/config/storage-account#az-webapp-config-storage-account-add)Komutunu, mevcut bir paylaşımın Linux işlev uygulamanıza bağlamak için kullanabilirsiniz. 

Bu komutta, `share-name` var olan Azure dosya paylaşımının adıdır ve `custom-id` işlev uygulamasına bağlandığında, bu paylaşımın benzersiz bir şekilde tanımlayan herhangi bir dize olabilir. Ayrıca, `mount-path` işlev uygulamanızda paylaşıma erişildiği yoldur. `mount-path`biçiminde olmalıdır `/dir-name` ve ile başlayamaz `/home` .

Tüm bir örnek için bkz. [Python işlev uygulaması oluşturma ve Azure dosya paylaşma bağlama](scripts/functions-cli-mount-files-storage-linux.md)içindeki betikler. 

Şu anda yalnızca bir `storage-type` , `AzureFiles` desteklenir. Yalnızca belirli bir işlev uygulamasına beş paylaşım bağlayabilirsiniz. Bir dosya paylaşımının bağlanması, en az 200-300ms veya depolama hesabı farklı bir bölgedeyse daha da fazla olan soğuk başlangıç süresini artırabilir.

Bağlı paylaşma, belirtilen işlev kodunuz için kullanılabilir `mount-path` . Örneğin, olduğunda `mount-path` `/path/to/mount` , aşağıdaki Python örneğinde olduğu gibi, hedef dizine dosya sistemi API 'leri ile erişebilirsiniz:

```python
import os
...

files_in_share = os.listdir("/path/to/mount")
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Işlevleri barındırma seçenekleri hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Azure İşlevleri’ni ölçeklendirme ve barındırma](functions-scale.md)


