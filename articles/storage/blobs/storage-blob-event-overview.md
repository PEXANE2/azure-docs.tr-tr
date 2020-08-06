---
title: Azure Blob depolama olaylarına yeniden davranıma | Microsoft Docs
description: Abone olmak ve BLOB depolama olaylarına tepki vermek için Azure Event Grid kullanın. Olay modelini anlayın, olayları filtreleme ve olayları tüketen uygulamalar.
author: normesta
ms.author: normesta
ms.date: 04/06/2020
ms.topic: conceptual
ms.service: storage
ms.subservice: blobs
ms.reviewer: dineshm
ms.openlocfilehash: 62cd31ab6f63aec5ddeb675bca3621a329ab1f2b
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87826577"
---
# <a name="reacting-to-blob-storage-events"></a>Blob depolama olaylarına yanıt verme

Azure depolama olayları, uygulamaların blob oluşturma ve silme gibi olaylara tepki vermesini sağlar. Bu, karmaşık kod veya pahalı ve verimsiz yoklama Hizmetleri gereksinimini ortadan kaldırmaz. En iyi bölüm yalnızca kullandığınız kadar ödersiniz.

BLOB depolama olayları, Azure Işlevleri, Azure Logic Apps gibi abonelere veya kendi http dinleyicinize hatta [Azure Event Grid](https://azure.microsoft.com/services/event-grid/) kullanılarak gönderilir. Event Grid zengin yeniden deneme ilkeleri ve atılacak uygulamalar aracılığıyla uygulamalarınıza güvenilir olay teslimi sağlar.

Blob Storage 'ın desteklediği olayların tam listesini görüntülemek için [BLOB depolama olayları şeması](../../event-grid/event-schema-blob-storage.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) makalesine bakın.

Ortak BLOB depolama olay senaryolarında görüntü veya video işleme, arama dizini oluşturma veya herhangi bir dosya odaklı iş akışı bulunur. Zaman uyumsuz dosya yüklemeleri, olaylara yönelik harika bir uyum. Değişiklikler seyrek olduğunda, ancak senaryonuz anında yanıt vermeyi gerektirdiğinde, olay tabanlı mimari özellikle etkili olabilir.

Blob Storage olaylarını denemek istiyorsanız, bu hızlı başlangıç makalelerinden herhangi birine bakın:

|Bu aracı kullanmak istiyorsanız:    |Şu makaleye bakın: |
|--|-|
|Azure portal    |[Hızlı başlangıç: Blob Depolama olaylarını Azure portal web uç noktasına yönlendirme](https://docs.microsoft.com/azure/event-grid/blob-event-quickstart-portal?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|PowerShell    |[Hızlı başlangıç: PowerShell ile Depolama olaylarını Web uç noktasına yönlendirme](https://docs.microsoft.com/azure/storage/blobs/storage-blob-event-quickstart-powershell?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Azure CLI    |[Hızlı başlangıç: Azure CLı ile Depolama olaylarını Web uç noktasına yönlendirme](https://docs.microsoft.com/azure/storage/blobs/storage-blob-event-quickstart?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|

Azure işlevleri 'ni kullanarak BLOB depolama olaylarına yeniden işlem hakkında ayrıntılı örnekler görüntülemek için şu makalelere bakın:

- [Öğretici: Databricks Delta tablosunu güncelleştirmek için Azure Data Lake Storage 2. olaylarını kullanın](data-lake-storage-events.md).
- [Öğretici: Event Grid kullanarak karşıya yüklenen görüntüleri yeniden boyutlandırmayı otomatikleştirme](https://docs.microsoft.com/azure/event-grid/resize-images-on-storage-blob-upload-event?tabs=dotnet)

>[!NOTE]
> Yalnızca **StorageV2 (genel amaçlı v2)**, **blok Blobstorage**ve **blobstorage** türündeki depolama hesapları olay tümleştirmesini destekler. **Depolama (genel amaçlı v1)** Event Grid ile *tümleştirmeyi desteklemez.*

## <a name="the-event-model"></a>Olay modeli

Event Grid, olay iletilerini abonelere yönlendirmek için [olay abonelikleri](../../event-grid/concepts.md#event-subscriptions) kullanır. Bu görüntüde olay yayımcıları, olay abonelikleri ve olay işleyicileri arasındaki ilişki gösterilmektedir.

![Event Grid modeli](./media/storage-blob-event-overview/event-grid-functional-model.png)

İlk olarak, bir uç noktayı olaya abone olun. Ardından, bir olay tetiklendiğinde Event Grid hizmeti bu olayla ilgili verileri uç noktaya gönderir.

Görüntülemek için [BLOB depolama olayları şeması](../../event-grid/event-schema-blob-storage.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) makalesine bakın:

> [!div class="checklist"]
> * BLOB depolama olaylarının ve her olayın nasıl tetiklendiğine ilişkin kapsamlı bir liste.
> * Event Grid bu olayların her biri için gönderebilecek verilere bir örnek.
> * Verilerde görüntülenen her anahtar değer çiftinin amacı.

## <a name="filtering-events"></a>Olayları filtreleme

Blob olayları olay türüne, kapsayıcı adına veya oluşturulan/Silinen nesnenin adına göre [filtrelenebilir](/cli/azure/eventgrid/event-subscription?view=azure-cli-latest) . Event Grid filtreler, konunun başından veya sonuna göre eşleşir, böylece eşleşen bir konu olan olaylar aboneye gider.

Filtrelerin nasıl uygulanacağı hakkında daha fazla bilgi edinmek için bkz. [Event Grid olayları filtreleme](https://docs.microsoft.com/azure/event-grid/how-to-filter-events).

Blob Storage olaylarının konusu şu biçimi kullanır:

```
/blobServices/default/containers/<containername>/blobs/<blobname>
```

Bir depolama hesabının tüm olaylarını eşleştirmek için, konu filtrelerini boş bırakabilirsiniz.

Bir öneki paylaşan bir kapsayıcı kümesi içinde oluşturulan Blobların olaylarını eşleştirmek için şöyle bir filtre kullanın `subjectBeginsWith` :

```
/blobServices/default/containers/containerprefix
```

Belirli bir kapsayıcıda oluşturulan Blobların olaylarını eşleştirmek için `subjectBeginsWith` şöyle bir filtre kullanın:

```
/blobServices/default/containers/containername/
```

Blob adı ön ekini paylaşan belirli bir kapsayıcıda oluşturulan Blobların olaylarını eşleştirmek için `subjectBeginsWith` şöyle bir filtre kullanın:

```
/blobServices/default/containers/containername/blobs/blobprefix
```

Blob sonekini paylaşan belirli bir kapsayıcıda oluşturulan Blobların olaylarını eşleştirmek için `subjectEndsWith` ". log" veya ". jpg" gibi bir filtre kullanın. Daha fazla bilgi için bkz. [Event Grid kavramları](../../event-grid/concepts.md#event-subscriptions).

## <a name="practices-for-consuming-events"></a>Olayları tüketen uygulamalar

Blob Depolama olaylarını işleyen uygulamalar birkaç önerilen uygulamayı izlemelidir:
> [!div class="checklist"]
> * Birden çok abonelik olayları aynı olay işleyicisine yönlendirmek üzere yapılandırılabildiğiniz için, olayların belirli bir kaynaktan olduğunu varsaymamak, ancak beklediğiniz depolama hesabından geldiğinden emin olmak için iletinin konusunu denetlemek önemlidir.
> * Benzer şekilde, eventType için hazırlanmakta olan bir olay olduğunu ve aldığınız tüm olayların istediğiniz tür olacağını kabul edin.
> * İletiler bir gecikmeden sonra gelebileceğinden, nesneler hakkındaki bilgilerinizin hala güncel olup olmadığını anlamak için ETag alanlarını kullanın. ETag alanını nasıl kullanacağınızı öğrenmek için bkz. [BLOB depolamada eşzamanlılık yönetimi](https://docs.microsoft.com/azure/storage/common/storage-concurrency?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#managing-concurrency-in-blob-storage). 
> * İletiler sıra dışına gelebileceğinden, belirli bir nesne üzerindeki olayların sırasını anlamak için sıralayıcı alanlarını kullanın. Sıralayıcı alanı, belirli bir blob adı için olayların mantıksal dizisini temsil eden bir dize değeridir. Aynı blob adı üzerinde iki olayın göreli sırasını anlamak için standart dize karşılaştırmayı kullanabilirsiniz.
> * Depolama olayları, abonelere en az bir kez gönderim garantisi sağlar ve bu da tüm iletilerin kaydedilmesini sağlar. Ancak, yeniden denemeler veya aboneliklerin kullanılabilirliği nedeniyle, yinelenen iletiler zaman zaman oluşabilir. İleti teslimi ve yeniden deneme hakkında daha fazla bilgi edinmek için bkz. [Event Grid ileti teslimi ve yeniden deneme](../../event-grid/delivery-and-retry.md).
> * Blob üzerinde ne tür işlemlere izin verileceğini ve Blobun erişmek için hangi istemci kitaplığı türlerini kullanacağınızı anlamak için blobType alanını kullanın. Geçerli değerler ya da `BlockBlob` `PageBlob` . 
> * `CloudBlockBlob` `CloudAppendBlob` BLOB 'a erişmek için ve oluşturucularla birlikte URL alanını kullanın.
> * Anladığınızı alanları yoksayın. Bu uygulama, gelecekte eklenebilecek yeni özelliklere dayanıklı tutmaya yardımcı olur.
> * **Microsoft. Storage. blobcreated** olayının yalnızca bir Blok Blobu tamamen yürütüldüğü zaman tetiklendiğinden emin olmak istiyorsanız,, `CopyBlob` `PutBlob` `PutBlockList` veya `FlushWithClose` REST API çağrılarının olayını filtreleyin. Bu API çağrıları, **Microsoft. Storage. BlobCreated** olayını yalnızca veriler bir blok blobuna tam olarak kaydedildikten sonra tetikler. Filtre oluşturmayı öğrenmek için bkz. [Event Grid olayları filtreleme](https://docs.microsoft.com/azure/event-grid/how-to-filter-events).


## <a name="next-steps"></a>Sonraki adımlar

Event Grid hakkında daha fazla bilgi edinin ve BLOB depolama olaylarına bir deneme verin:

- [Event Grid Hakkında](../../event-grid/overview.md)
- [BLOB depolama olayları şeması](../../event-grid/event-schema-blob-storage.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
- [Blob Depolama olaylarını özel bir Web uç noktasına yönlendirme](storage-blob-event-quickstart.md)
