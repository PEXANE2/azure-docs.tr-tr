---
title: Azure Olay Izgara lı depolama etkinliği şeması
description: Azure Olay Ağıt'ı ile blob depolama etkinlikleri için sağlanan özellikleri açıklar
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: reference
ms.date: 01/17/2019
ms.author: spelluru
ms.openlocfilehash: 4a71f50a130bd9b22965d39fa942b47c70857a86
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79265044"
---
# <a name="azure-event-grid-event-schema-for-blob-storage"></a>Blob depolama için Azure Olay Izgara olay şeması

Bu makalede, blob depolama olayları için özellikleri ve şema sağlar.Etkinlik şemalarına giriş için [Azure Olay Izgara olay şemasına](event-schema.md)bakın.

Örnek komut dosyaları ve öğreticilerin listesi için [Depolama olay kaynağına](event-sources.md#storage)bakın.

>[!NOTE]
> Yalnızca depolama türü **StorageV2 (genel amaçlı v2)** ve **BlobStorage** destek olay tümleştirmedepolama hesapları. **Depolama (genral amaçlı v1)** Olay Grid ile tümleştirme *desteklemez.*

## <a name="list-of-events-for-blob-rest-apis"></a>Blob REST API'leri için etkinlikler listesi

Bu olaylar, istemci Blob REST API'lerini arayarak bir blob oluşturduğunda, değiştirdiğinde veya sildiğinde tetiklenir.

 |Olay adı |Açıklama|
 |----------|-----------|
 |**Microsoft.Storage.BlobCreated** |Bir leke oluşturulduğunda veya değiştirildiğinde tetiklenir. <br>Özellikle, bu olay istemciler Blob `PutBlockList`REST `CopyBlob` API'de bulunan `PutBlob`, veya işlemleri kullandığında tetiklenir.   |
 |**Microsoft.Storage.BlobDeleted** |Bir leke silindiğinde tetiklenir. <br>Özellikle, istemciler Blob REST `DeleteBlob` API'de bulunan işlemi aradığında bu olay tetiklenir. |

> [!NOTE]
> **Microsoft.Storage.BlobCreated** olayının yalnızca Bir Blok Blob tamamen işlendiğinde tetiklendiğinden emin olmak `CopyBlob`istiyorsanız, olayı , ve `PutBlob` `PutBlockList` REST API çağrıları için filtreuygulayın. Bu API **çağrıları, Microsoft.Storage.BlobCreated** olayını yalnızca veriler Bir Blok Blob'a tam olarak bağlandıktan sonra tetikler. Filtre oluşturmayı öğrenmek için [Olay Izgarası için Filtre olayları'na](https://docs.microsoft.com/azure/event-grid/how-to-filter-events)bakın.

## <a name="list-of-the-events-for-azure-data-lake-storage-gen-2-rest-apis"></a>Azure Veri Gölü Depolama Gen 2 REST API'leri için etkinlikler listesi

Depolama hesabında hiyerarşik bir ad alanı etkinleştiriseniz ve istemciler Azure Veri Gölü Depolama Gen2 REST API'lerini çağırırsanız, bu olaylar tetiklenir.

|Olay adı|Açıklama|
|----------|-----------|
|**Microsoft.Storage.BlobCreated** | Bir leke oluşturulduğunda veya değiştirildiğinde tetiklenir. <br>Özellikle, bu olay, istemciler `CreateFile` Azure `FlushWithClose` Veri Gölü Depolama Gen2 REST API'sinde bulunan işlemleri ve işlemleri kullandığında tetiklenir. |
|**Microsoft.Storage.BlobDeleted** |Bir leke silindiğinde tetiklenir. <br>Özellikle, bu olay, istemciler Azure `DeleteFile` Veri Gölü Depolama Gen2 REST API'de kullanılabilen işlemi aradığında da tetiklenir. |
|**Microsoft.Storage.BlobRenamed**|Bir blob yeniden adlandırıldı zaman tetiklenir. <br>Özellikle, bu olay, istemciler `RenameFile` Azure Veri Gölü Depolama Gen2 REST API'de kullanılabilen işlemi kullandığında tetiklenir.|
|**Microsoft.Storage.DirectoryCreated**|Bir dizin oluşturulduğunda tetiklenir. <br>Özellikle, bu olay, istemciler `CreateDirectory` Azure Veri Gölü Depolama Gen2 REST API'de kullanılabilen işlemi kullandığında tetiklenir.|
|**Microsoft.Storage.DirectoryRenamed**|Dizinin adı yeniden adlandırıldıklarında tetiklenir. <br>Özellikle, bu olay, istemciler `RenameDirectory` Azure Veri Gölü Depolama Gen2 REST API'de kullanılabilen işlemi kullandığında tetiklenir.|
|**Microsoft.Storage.DirectoryDeleted**|Bir dizin silindiğinde tetiklenir. <br>Özellikle, bu olay, istemciler `DeleteDirectory` Azure Veri Gölü Depolama Gen2 REST API'de kullanılabilen işlemi kullandığında tetiklenir.|

> [!NOTE]
> **Microsoft.Storage.BlobCreated** olayının yalnızca Bir Blok Blob tamamen işlendiğinde tetiklendiğinden emin olmak `FlushWithClose` istiyorsanız, REST API çağrısı için olayı filtreleyin. Bu API **çağrısı, Microsoft.Storage.BlobCreated** olayını yalnızca veriler Bir Blok Blob'a tam olarak bağlandıktan sonra tetikler. Filtre oluşturmayı öğrenmek için [Olay Izgarası için Filtre olayları'na](https://docs.microsoft.com/azure/event-grid/how-to-filter-events)bakın.

<a id="example-event" />

## <a name="the-contents-of-an-event-response"></a>Olay yanıtının içeriği

Bir olay tetiklendiğinde, Olay Izgara hizmeti bitiş noktasına abone olmak için bu olayla ilgili verileri gönderir.

Bu bölümde, bu verilerin her blob depolama olayı için nasıl görüneceğine dair bir örnek içerir.

### <a name="microsoftstorageblobcreated-event"></a>Microsoft.Storage.BlobCreated olay

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/test-container/blobs/new-file.txt",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "PutBlockList",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "eTag": "\"0x8D4BCC2E4835CD0\"",
    "contentType": "text/plain",
    "contentLength": 524288,
    "blobType": "BlockBlob",
    "url": "https://my-storage-account.blob.core.windows.net/testcontainer/new-file.txt",
    "sequencer": "00000000000004420000000000028963",
    "storageDiagnostics": {
      "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

### <a name="microsoftstorageblobcreated-event-data-lake-storage-gen2"></a>Microsoft.Storage.BlobCreated olay (Veri Gölü Depolama Gen2)

Blob depolama hesabı hiyerarşik bir ad alanı varsa, veri bu değişiklikler dışında önceki örneğe benzer görünüyor:

* Anahtar `dataVersion` bir değere `2`ayarlanır.

* Anahtar `data.api` dize `CreateFile` veya `FlushWithClose`.

* Anahtar `contentOffset` veri kümesine dahildir.

> [!NOTE]
> Uygulamalar hesaba yeni `PutBlockList` bir blob yüklemek için işlemi kullanırsa, veriler bu değişiklikleri içermez.

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/my-file-system/blobs/new-file.txt",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "CreateFile",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "eTag": "\"0x8D4BCC2E4835CD0\"",
    "contentType": "text/plain",
    "contentLength": 0,
    "contentOffset": 0,
    "blobType": "BlockBlob",
    "url": "https://my-storage-account.dfs.core.windows.net/my-file-system/new-file.txt",
    "sequencer": "00000000000004420000000000028963",  
    "storageDiagnostics": {
    "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "2",
  "metadataVersion": "1"
}]
```

### <a name="microsoftstorageblobdeleted-event"></a>Microsoft.Storage.BlobDeleted olay

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/testcontainer/blobs/file-to-delete.txt",
  "eventType": "Microsoft.Storage.BlobDeleted",
  "eventTime": "2017-11-07T20:09:22.5674003Z",
  "id": "4c2359fe-001e-00ba-0e04-58586806d298",
  "data": {
    "api": "DeleteBlob",
    "requestId": "4c2359fe-001e-00ba-0e04-585868000000",
    "contentType": "text/plain",
    "blobType": "BlockBlob",
    "url": "https://my-storage-account.blob.core.windows.net/testcontainer/file-to-delete.txt",
    "sequencer": "0000000000000281000000000002F5CA",
    "storageDiagnostics": {
      "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

### <a name="microsoftstorageblobdeleted-event-data-lake-storage-gen2"></a>Microsoft.Storage.BlobDeleted olay (Veri Gölü Depolama Gen2)

Blob depolama hesabı hiyerarşik bir ad alanı varsa, veri bu değişiklikler dışında önceki örneğe benzer görünüyor:

* Anahtar `dataVersion` bir değere `2`ayarlanır.

* Anahtar `data.api` dize `DeleteFile`ayarlanır.

* Anahtar `url` yolu `dfs.core.windows.net`içerir.

> [!NOTE]
> Uygulamalar hesaptan `DeleteBlob` bir blob silmek için işlemi kullanırsa, veriler bu değişiklikleri içermez.

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/my-file-system/blobs/file-to-delete.txt",
  "eventType": "Microsoft.Storage.BlobDeleted",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
    "data": {
    "api": "DeleteFile",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "contentType": "text/plain",
    "blobType": "BlockBlob",
    "url": "https://my-storage-account.dfs.core.windows.net/my-file-system/file-to-delete.txt",
    "sequencer": "00000000000004420000000000028963",  
    "storageDiagnostics": {
    "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "2",
  "metadataVersion": "1"
}]
```

### <a name="microsoftstorageblobrenamed-event"></a>Microsoft.Storage.BlobRenamed olay

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/my-file-system/blobs/my-renamed-file.txt",
  "eventType": "Microsoft.Storage.BlobRenamed",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "RenameFile",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "destinationUrl": "https://my-storage-account.dfs.core.windows.net/my-file-system/my-renamed-file.txt",
    "sourceUrl": "https://my-storage-account.dfs.core.windows.net/my-file-system/my-original-file.txt",
    "sequencer": "00000000000004420000000000028963",  
    "storageDiagnostics": {
    "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

### <a name="microsoftstoragedirectorycreated-event"></a>Microsoft.Storage.DirectoryCreated olay

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/my-file-system/blobs/my-new-directory",
  "eventType": "Microsoft.Storage.DirectoryCreated",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "CreateDirectory",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "url": "https://my-storage-account.dfs.core.windows.net/my-file-system/my-new-directory",
    "sequencer": "00000000000004420000000000028963",  
    "storageDiagnostics": {
    "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

### <a name="microsoftstoragedirectoryrenamed-event"></a>Microsoft.Storage.DirectoryRenamed olay

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/my-file-system/blobs/my-renamed-directory",
  "eventType": "Microsoft.Storage.DirectoryRenamed",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "RenameDirectory",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "destinationUrl": "https://my-storage-account.dfs.core.windows.net/my-file-system/my-renamed-directory",
    "sourceUrl": "https://my-storage-account.dfs.core.windows.net/my-file-system/my-original-directory",
    "sequencer": "00000000000004420000000000028963",  
    "storageDiagnostics": {
    "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

### <a name="microsoftstoragedirectorydeleted-event"></a>Microsoft.Storage.DirectoryDeleted olay

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/my-file-system/blobs/directory-to-delete",
  "eventType": "Microsoft.Storage.DirectoryDeleted",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "DeleteDirectory",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "url": "https://my-storage-account.dfs.core.windows.net/my-file-system/directory-to-delete",
    "recursive": "true", 
    "sequencer": "00000000000004420000000000028963",  
    "storageDiagnostics": {
    "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

## <a name="event-properties"></a>Olay özellikleri

Bir olay aşağıdaki üst düzey verilere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| konu başlığı | string | Olay kaynağına tam kaynak yolu. Bu alan yazılamaz. Event Grid bu değeri sağlar. |
| Konu | string | Olay konusunun yayımcı tarafından tanımlanan yolu. |
| Eventtype | string | Bu olay kaynağı için kayıtlı olay türlerinden biri. |
| eventTime | string | Olayın sağlayıcının UTC zamanına bağlı olarak oluşturulan süre. |
| id | string | Etkinlik için benzersiz tanımlayıcı. |
| veri | object | Blob depolama olay verileri. |
| dataVersion | string | Veri nesnesinin şema sürümü. Şema sürümünü yayımcı tanımlar. |
| metadataVersion | string | Olay meta verilerinin şema sürümü. Event Grid en üst düzey özelliklerin şemasını tanımlar. Event Grid bu değeri sağlar. |

Veri nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| API | string | Olayı tetikleyen işlem. |
| clientRequestId | string | depolama API işlemi için istemci tarafından sağlanan istek kimliği. Bu kimlik, günlüklerde "istemci-istek-id" alanını kullanarak Azure Depolama tanı günlükleri ile ilişkilendirmek için kullanılabilir ve "x-ms-client-request-id" üstbilgisini kullanarak istemci isteklerinde sağlanabilir. [Bkz. Günlük Biçimi](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-log-format). |
| Requestıd | string | Depolama API işlemi için hizmet tarafından oluşturulan istek kimliği. Günlüklerde "istek-id-üstbilgi" alanını kullanarak Azure Depolama tanı günlükleri ile ilişkilendirmek için kullanılabilir ve 'x-ms-request-id' üstbilgisinde API çağrısını başlatmaktan döndürülür. [Bkz. Günlük Biçimi](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-log-format). |
| Etag | string | İşlemleri koşullu olarak gerçekleştirmek için kullanabileceğiniz değer. |
| Contenttype | string | Blob için belirtilen içerik türü. |
| Contentlength | integer | Bayttaki lekenin boyutu. |
| blobType | string | Leke türü. Geçerli değerler "BlockBlob" veya "PageBlob" dır. |
| içerikOffset | number | Olay tetikleyici uygulamanın dosyaya yazmayı tamamladığı noktada alınan bir yazma işleminin baytlarında ofset. <br>Yalnızca hiyerarşik ad alanına sahip blob depolama hesaplarında tetiklenen olaylar için görünür.|
| hedefUrl |string | İşlem tamamlandıktan sonra var olacak dosyanın url'si. Örneğin, bir dosya nın adı `destinationUrl` yeniden adlandırıldıysa, özellik yeni dosya adının url'sini içerir. <br>Yalnızca hiyerarşik ad alanına sahip blob depolama hesaplarında tetiklenen olaylar için görünür.|
| kaynakUrl |string | İşlemden önce var olan dosyanın url'si. Örneğin, bir dosya yeniden adlandırılmışsa, yeniden adlandırma işleminden önce özgün dosya adının url'sini `sourceUrl` içerir. <br>Yalnızca hiyerarşik ad alanına sahip blob depolama hesaplarında tetiklenen olaylar için görünür. |
| url | string | Blob'a giden yol. <br>İstemci Blob REST API kullanıyorsa, url bu yapıya sahiptir: * \<depolama-hesap adı\>.blob.core.windows.net/\<kapsayıcı\>/\<adı dosya adı.\>* <br>İstemci bir Veri Gölü Depolama REST API kullanıyorsa, url bu yapıya sahiptir: * \<depolama-hesap adı\>.dfs.core.windows.net/\<dosya-sistem adı\>/\<\>dosya adı.* |
| Özyinelemeli | string | `True`tüm alt dizinlerde işlemi gerçekleştirmek için; aksi `False`takdirde . <br>Yalnızca hiyerarşik ad alanına sahip blob depolama hesaplarında tetiklenen olaylar için görünür. |
| Sequencer | string | Belirli bir blob adı için olayların mantıksal dizisini temsil eden opak bir dize değeri.  Kullanıcılar, aynı blob adındaki iki olayın göreli dizisini anlamak için standart dize karşılaştırmasını kullanabilir. |
| depolamaTanılama | object | Azure Depolama hizmeti tarafından zaman zaman ekteki tanılama verileri. Mevcut olduğunda, olay tüketiciler tarafından göz ardı edilmelidir. |

## <a name="next-steps"></a>Sonraki adımlar

* Azure Etkinlik Izgarasına giriş için [olay ızgarası nedir?](overview.md)
* Azure Olay Ağı aboneliği oluşturma hakkında daha fazla bilgi için [Olay Ağı abonelik şemasına](subscription-creation-schema.md)bakın.
* Blob depolama etkinlikleri ile çalışmaya giriş için [Bkz.](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) 
