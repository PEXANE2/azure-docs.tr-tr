---
title: Azure Blob depolama etkinliklerine tepki verme | Microsoft Dokümanlar
description: Blob depolama olaylarına abone olmak için Azure Event Grid’i kullanın.
author: normesta
ms.author: normesta
ms.date: 04/06/2020
ms.topic: conceptual
ms.service: storage
ms.subservice: blobs
ms.reviewer: cbrooks
ms.openlocfilehash: e4dd6bab6198546dc5acab78ec59d92387328dbb
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80755010"
---
# <a name="reacting-to-blob-storage-events"></a>Blob depolama olaylarına yanıt verme

Azure Depolama olayları, uygulamaların blob oluşturma ve silme gibi olaylara tepki göstermesine olanak tanır. Bunu karmaşık kod veya pahalı ve verimsiz yoklama hizmetlerine gerek kalmadan yapar. En iyi kısmı sadece kullandığınız için ödeme.

Blob depolama etkinlikleri [Azure Etkinlik Ağıtı](https://azure.microsoft.com/services/event-grid/) kullanılarak Azure Fonksiyonları, Azure Mantık Uygulamaları ve hatta kendi http dinleyiciniz gibi abonelere itilir. Olay Grid zengin yeniden deneme ilkeleri ve ölü harflerle uygulamalarınıza güvenilir olay teslimi sağlar.

Blob depolamanın desteklediği olayların tam listesini görüntülemek için [Blob depolama olayları şema](../../event-grid/event-schema-blob-storage.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) makalesine bakın.

Sık karşılaşılan Blob depolama olayı senaryoları görüntü veya video işleme, arama dizini oluşturma veya dosya yönelimli iş akışını içerir. Asynchronous dosya yüklemeleri olaylar için harika bir uyum vardır. Değişiklikler seyrek olduğunda, ancak senaryonuz anında yanıt verme gerektiriyorsa, olay tabanlı mimari özellikle verimli olabilir.

Blob depolama olaylarını denemek istiyorsanız, şu hızlı başlangıç makalelerinden herhangi birini görün:

|Bu aracı kullanmak istiyorsanız:    |Bu makaleye bakın: |
|--|-|
|Azure portal    |[Quickstart: Azure portalı ile Blob depolama olaylarını web bitiş noktasına yönlendirin](https://docs.microsoft.com/azure/event-grid/blob-event-quickstart-portal?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|PowerShell    |[Quickstart: PowerShell ile depolama olaylarını web bitiş noktasına yönlendirin](https://docs.microsoft.com/azure/storage/blobs/storage-blob-event-quickstart-powershell?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Azure CLI    |[Hızlı başlangıç: Azure CLI ile depolama etkinliklerini web bitiş noktasına yönlendirin](https://docs.microsoft.com/azure/storage/blobs/storage-blob-event-quickstart?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|

Azure işlevlerini kullanarak Blob depolama olaylarına tepki vermenin ayrıntılı örneklerini görüntülemek için aşağıdaki makalelere bakın:

- [Öğretici: Databricks Delta tablosunu güncelleştirmek için Azure Veri Gölü Depolama Gen2 olaylarını kullanın.](data-lake-storage-events.md)
- [Öğretici: Olay Izgara'yı kullanarak yüklenen görüntüleri yeniden boyutlandırmayı otomatikleştirin](https://docs.microsoft.com/azure/event-grid/resize-images-on-storage-blob-upload-event?tabs=dotnet)

>[!NOTE]
> Tür **StorageV2 (genel amaçlı v2)** sadece depolama hesapları, **BlockBlobStorage**ve **BlobStorage** destek olay entegrasyonu. **Depolama (genral amaçlı v1)** Olay Grid ile tümleştirme *desteklemez.*

## <a name="the-event-model"></a>Olay modeli

Olay Grid, olay iletilerini abonelere yönlendirmek için [olay aboneliklerini](../../event-grid/concepts.md#event-subscriptions) kullanır. Bu resim, olay yayımcılar, olay abonelikleri ve olay işleyicileri arasındaki ilişkiyi göstermektedir.

![Olay Izgara Modeli](./media/storage-blob-event-overview/event-grid-functional-model.png)

İlk olarak, bir olayın bitiş noktasına abone olun. Daha sonra, bir olay tetiklendiğinde, Olay Izgara hizmeti bu olayla ilgili verileri bitiş noktasına gönderir.

Görüntülemek için [Blob depolama olayları şema](../../event-grid/event-schema-blob-storage.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) makalesine bakın:

> [!div class="checklist"]
> * Blob depolama olaylarının tam listesi ve her olayın nasıl tetiklendirildi
> * Olay Izgarası'nın bu olayların her biri için göndereceği verilerin bir örneği.
> * Verilerde görünen her anahtar değer çiftinin amacı.

## <a name="filtering-events"></a>Olayları filtreleme

Blob olayları, oluşturulan/silinen nesnenin olay türüne, kapsayıcı adına veya adına [göre filtrelenebilir.](/cli/azure/eventgrid/event-subscription?view=azure-cli-latest) Olay Izgara'daki filtreler, eşleşen bir özneyle ilgili olayların aboneye gitmesi için konunun başlangıcıveya sonuyla eşleşir.

Filtrelerin nasıl uygulanacağı hakkında daha fazla bilgi edinmek [için Olay Izgarası için Filtre olayları'na](https://docs.microsoft.com/azure/event-grid/how-to-filter-events)bakın.

Blob depolama olaylarının konusu biçimi kullanır:

```
/blobServices/default/containers/<containername>/blobs/<blobname>
```

Bir depolama hesabının tüm olaylarını eşleştirmek için konu filtrelerini boş bırakabilirsiniz.

Önek paylaşan bir kapsayıcı kümesinde oluşturulan blobs olayları `subjectBeginsWith` eşleştirmek için aşağıdaki gibi bir filtre kullanın:

```
/blobServices/default/containers/containerprefix
```

Belirli bir kapta oluşturulan lekelerdeki olayları `subjectBeginsWith` eşleştirmek için aşağıdaki gibi bir filtre kullanın:

```
/blobServices/default/containers/containername/
```

Blob ad önekini paylaşan belirli bir kapsayıcıda oluşturulan lekelerdeki olayları eşleştirmek için aşağıdaki gibi bir `subjectBeginsWith` filtre kullanın:

```
/blobServices/default/containers/containername/blobs/blobprefix
```

Blob soneki paylaşan belirli bir kapsayıcıda oluşturulan lekelerdeki `subjectEndsWith` olayları eşleştirmek için ".log" veya ".jpg" gibi bir filtre kullanın. Daha fazla bilgi için [Olay Izgara Kavramları'na](../../event-grid/concepts.md#event-subscriptions)bakın.

## <a name="practices-for-consuming-events"></a>Etkinlikleri tüketme uygulamaları

Blob depolama olaylarını işleyen uygulamalar önerilen birkaç uygulamayı izlemelidir:
> [!div class="checklist"]
> * Birden çok abonelik olayları aynı olay işleyicisine yönlendirecek şekilde yapılandırılabildiği için, olayların belirli bir kaynaktan geldiğini varsaymak değil, beklediğiniz depolama hesabından geldiğinden emin olmak için iletinin konusunu denetlemek önemlidir.
> * Benzer şekilde, eventType'ın işlemeye hazır olup olmadığını denetleyin ve aldığınız tüm olayların beklediğiniz türler olacağını varsaymayın.
> * İletiler biraz gecikmeden sonra gelebileceğinden, nesneler hakkındaki bilgilerinizin hala güncel olup olmadığını anlamak için etag alanlarını kullanın. Etag alanını nasıl kullanacağınızı öğrenmek için [Blob depolamabiriminde eşzamanlılık yönetme'ye](https://docs.microsoft.com/azure/storage/common/storage-concurrency?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#managing-concurrency-in-blob-storage)bakın. 
> * İletiler sıradışı gelebildiği için, belirli bir nesnedeki olayların sırasını anlamak için sıralayıcı alanlarını kullanın. Sıralayıcı alanı, belirli bir blob adı için olayların mantıksal dizisini temsil eden bir dize değeridir. Aynı blob adı üzerinde iki olayın göreli dizisini anlamak için standart dize karşılaştırması kullanabilirsiniz.
> Depolama olayları, abonelere en az bir kez teslimatı garanti eder ve bu da tüm iletilerin çıktılanmasını sağlar. Ancak yeniden denemeler veya aboneliklerin kullanılabilirliği nedeniyle, yinelenen iletiler bazen oluşabilir.
> * Blob'da ne tür işlemlere izin verildiğini ve blob'a erişmek için hangi istemci kitaplık türlerini kullanmanız gerektiğini anlamak için blobType alanını kullanın. Geçerli değerler `BlockBlob` ya `PageBlob`da . 
> * Blob erişmek için `CloudBlockBlob` `CloudAppendBlob` url alanı ve yapıcılar ile kullanın.
> * Anlamadığınız alanları yoksay. Bu uygulama, gelecekte eklenebilir yeni özelliklere karşı esnek tutmanıza yardımcı olacaktır.
> * **Microsoft.Storage.BlobCreated** olayının yalnızca Bir Blok Blob tamamen işlendiğinde tetiklendiğinden emin olmak `CopyBlob`istiyorsanız, olayı , yani `PutBlob`, `PutBlockList` veya `FlushWithClose` REST API çağrıları için filtreleyin. Bu API **çağrıları, Microsoft.Storage.BlobCreated** olayını yalnızca veriler Bir Blok Blob'a tam olarak bağlandıktan sonra tetikler. Filtre oluşturmayı öğrenmek için [Olay Izgarası için Filtre olayları'na](https://docs.microsoft.com/azure/event-grid/how-to-filter-events)bakın.


## <a name="next-steps"></a>Sonraki adımlar

Olay Izgarası hakkında daha fazla bilgi edinin ve Blob depolama etkinliklerini deneyin:

- [Event Grid Hakkında](../../event-grid/overview.md)
- [Blob depolama etkinlikleri şema](../../event-grid/event-schema-blob-storage.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
- [Blob depolama Olaylarını özel bir web bitiş noktasına yönlendirin](storage-blob-event-quickstart.md)
