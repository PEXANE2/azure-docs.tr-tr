---
title: Azure İşlevleri için depolama yla ilgili hususlar
description: Azure İşlevlerinin depolama gereksinimleri ve depolanan verileri şifreleme hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 01/21/2020
ms.openlocfilehash: 48ff2dedd997cccb76b13acdadc895504f656ea3
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80984172"
---
# <a name="storage-considerations-for-azure-functions"></a>Azure İşlevleri için depolama yla ilgili hususlar

Azure İşlevler, bir işlev uygulaması örneği oluşturduğunuzda bir Azure Depolama hesabı gerektirir. Aşağıdaki depolama hizmetleri işlev uygulamanız tarafından kullanılabilir:


|Depolama hizmeti  | Fonksiyonkullanımı  |
|---------|---------|
| [Azure Blob depolama](../storage/blobs/storage-blobs-introduction.md)     | Bağlama durumunu ve işlev anahtarlarını koruyun.  <br/>[Ayrıca Dayanıklı İşlevler görev hub'ları](durable/durable-functions-task-hubs.md)tarafından kullanılır. |
| [Azure Dosyalar](../storage/files/storage-files-introduction.md)  | [İşlev](functions-scale.md#consumption-plan)uygulama kodunuzu Tüketim Planı'nda depolamak ve çalıştırmak için kullanılan dosya paylaşımı. |
| [Azure Sıra depolama](../storage/queues/storage-queues-introduction.md)     | Dayanıklı [İşlevler'de görev hub'ları](durable/durable-functions-task-hubs.md)tarafından kullanılır.   |
| [Azure Tablo depolama](../storage/tables/table-storage-overview.md)  |  Dayanıklı [İşlevler'de görev hub'ları](durable/durable-functions-task-hubs.md)tarafından kullanılır.       |

> [!IMPORTANT]
> Tüketim barındırma planı kullanılırken işlev kodunuz ve bağlama yapılandırma dosyalarınız ana depolama hesabındaki Azure Dosya depolama alanında saklanır. Ana depolama hesabını sildiğinizde bu içerik silinir ve kurtarılamaz.

## <a name="storage-account-requirements"></a>Depolama hesabı gereksinimleri

Bir işlev uygulaması oluştururken, Blob, Queue ve Tablo depolama alanını destekleyen genel amaçlı bir Azure Depolama hesabı oluşturmanız veya bağlantı kurmanız gerekir. Bunun nedeni, Işlevlerin tetikleyicileri yönetme ve işlev yürütmelerini günlüğe kaydetme gibi işlemler için Azure Depolama'ya dayanmasıdır. Bazı depolama hesapları kuyrukları ve tabloları desteklemez. Bu hesaplar, yalnızca blob depolama hesaplarını, Azure Premium Depolama'yı ve ZRS çoğaltma içeren genel amaçlı depolama hesaplarını içerir. Bu desteklenmeyen hesaplar, bir işlev uygulaması oluşturulurken Depolama Hesabı bıyığınızdan filtrelenir.

Depolama hesabı türleri hakkında daha fazla bilgi edinmek için bkz. [Azure Depolama Hizmetlerine Giriş](../storage/common/storage-introduction.md#core-storage-services). 

İşlev uygulamanızla varolan bir depolama hesabı kullanabiliyorken, bu gereksinimleri karşıladığından emin olmalısınız. İşlev uygulamasının bir parçası olarak oluşturulan depolama hesapları, bu depolama hesabı gereksinimlerini karşılayacak şekilde akış oluşturur.  

## <a name="storage-account-guidance"></a>Depolama hesabı kılavuzu

Her işlev uygulamasının çalışması için bir depolama hesabı gerektirir. Bu hesap silinirse işlev uygulamanız çalışmaz. Depolama yla ilgili sorunları gidermek için, [depolama yla ilgili sorunları nasıl giderir'e](functions-recover-storage-account.md)bakın. İşlev uygulamaları tarafından kullanılan Depolama hesabı için aşağıdaki ek hususlar geçerlidir.

### <a name="storage-account-connection-setting"></a>Depolama hesabı bağlantı ayarı

Depolama hesabı bağlantısı [AzureWebJobsStorage uygulama ayarında](./functions-app-settings.md#azurewebjobsstorage)korunur. 

Depolama alanı anahtarlarını yeniden oluşturduğunuzda depolama hesabı bağlantı dizesi güncelleştirilmelidir. [Burada depolama anahtarı yönetimi hakkında daha fazla bilgi edinin.](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account)

### <a name="shared-storage-accounts"></a>Paylaşılan depolama hesapları

Birden çok işlevli uygulamanın aynı depolama hesabını herhangi bir sorun olmadan paylaşması mümkündür. Örneğin, Visual Studio'da Azure Depolama Emülatörü'ni kullanarak birden çok uygulama geliştirebilirsiniz. Bu durumda, emülatör tek bir depolama hesabı gibi davranır. İşlev uygulamanız tarafından kullanılan aynı depolama hesabı, uygulama verilerinizi depolamak için de kullanılabilir. Ancak, bu yaklaşım her zaman bir üretim ortamında iyi bir fikir değildir.

### <a name="optimize-storage-performance"></a>Depolama performansını iyileştirme

[!INCLUDE [functions-shared-storage](../../includes/functions-shared-storage.md)]

## <a name="storage-data-encryption"></a>Depolama veri şifrelemesi

Azure Depolama, bir depolama hesabındaki tüm verileri istirahat halinde şifreler. Daha fazla bilgi [için, veriler için Azure Depolama şifrelemesi'ne](../storage/common/storage-service-encryption.md)bakın.

Varsayılan olarak, veriler Microsoft tarafından yönetilen anahtarlarla şifrelenir. Şifreleme anahtarları üzerinde ek denetim için, blob ve dosya verilerinin şifrelemesi için kullanmak üzere müşteri tarafından yönetilen anahtarları sağlayabilirsiniz. Bu anahtarların depolama hesabına erişebilmesi için Işlevler için Azure Anahtar Kasası'nda bulunması gerekir. Daha fazla bilgi edinmek için Azure [portalını kullanarak Azure Anahtar Kasası ile müşteri tarafından yönetilen anahtarları yapılandır'a](../storage/common/storage-encryption-keys-portal.md)bakın.  

## <a name="mount-file-shares-linux"></a>Dosya paylaşımlarını dağıla (Linux)

Mevcut Azure Dosyaları paylaşımlarını Linux işlev uygulamalarınıza monte edebilirsiniz. Linux işlev uygulamanıza bir paylaşım ekleyerek, işlevlerinizdeki mevcut makine öğrenimi modellerinden veya diğer verilerden yararlanabilirsiniz. Linux işlev [`az webapp config storage-account add`](/cli/azure/webapp/config/storage-account#az-webapp-config-storage-account-add) uygulamanıza varolan bir paylaşım damak için komutu kullanabilirsiniz. 

Bu komutta, `share-name` varolan Azure Dosyaları paylaşımının `custom-id` adıdır ve işlev uygulamasına monte edildiğinde paylaşımı benzersiz olarak tanımlayan herhangi bir dize olabilir. Ayrıca, `mount-path` işlev uygulamanızda paylaşıma erişilen yoldur. `mount-path`biçiminde `/dir-name`olmalıdır ve `/home`'ile başlayamaz.

Tam bir örnek için, [Python işlev iþlemi uygulaması oluÅ](scripts/functions-cli-mount-files-storage-linux.md)up komut dosyasına bakın ve Azure Dosyalar ışığını bindir. 

Şu anda, yalnızca bir `storage-type` `AzureFiles` desteklenir. Belirli bir işlev uygulamasına yalnızca beş paylaşım monte edebilirsiniz. Dosya paylaşımını monte etmek, soğuk başlangıç süresini en az 200-300ms, hatta depolama hesabı farklı bir bölgede yken daha da artırabilir.

Monte edilen paylaşım, `mount-path` belirtilen işlev kodunuz tarafından kullanılabilir. Örneğin, ne `mount-path` `/path/to/mount`zaman , aşağıdaki Python örneğinde olduğu gibi, dosya sistemi API'leri tarafından hedef dizine erişebilirsiniz:

```python
import os
...

files_in_share = os.listdir("/path/to/mount")
```

## <a name="next-steps"></a>Sonraki adımlar

Azure İşlevler barındırma seçenekleri hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Azure İşlevleri’ni ölçeklendirme ve barındırma](functions-scale.md)


