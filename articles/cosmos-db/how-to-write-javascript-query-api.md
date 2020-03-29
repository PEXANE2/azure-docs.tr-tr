---
title: Azure Cosmos DB'deki JavaScript sorgu API'sini kullanarak depolanan yordamları ve tetikleyicileri yazma
description: Azure Cosmos DB'deki JavaScript Sorgu API'sini kullanarak depolanmış yordamları ve tetikleyicileri nasıl yazarak yazacaklarınız öğrenin
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: mjbrown
ms.openlocfilehash: 221a3118808a044ef1b1b822b9c95772bf792f34
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75441720"
---
# <a name="how-to-write-stored-procedures-and-triggers-in-azure-cosmos-db-by-using-the-javascript-query-api"></a>JavaScript sorgu API'sini kullanarak Azure Cosmos DB'de depolanmış yordamlar ve tetikleyiciler nasıl yazılır?

Azure Cosmos DB, depolanan yordamları veya tetikleyicileri yazmak için kullanılabilecek SQL dili hakkında hiçbir bilgi sahibi olmadan akıcı bir JavaScript arabirimi kullanarak en iyi duruma getirilmiş sorguları gerçekleştirmenize olanak tanır. Azure Cosmos DB'deki JavaScript Sorgu API desteği hakkında daha fazla bilgi edinmek için Azure [Cosmos DB makalesinde JavaScript dili tümleşik sorgu API'siyle çalışma](javascript-query-api.md) başlıklı makaleye bakın.

## <a name="stored-procedure-using-the-javascript-query-api"></a><a id="stored-procedures"></a>JavaScript sorgu API'sini kullanarak depolanan yordam

Aşağıdaki kod örneği, JavaScript sorgu API'sinin depolanan yordam bağlamında nasıl kullanıldığına bir örnektir. Depolanan yordam, giriş parametresi tarafından belirtilen bir Azure Cosmos öğesi ekler ve giriş `__.filter()` öğesinin boyutuna bağlı olarak minSize, maxSize ve totalSize ile yöntemi kullanarak bir meta veri belgesini güncelleştirir.

> [!NOTE]
> `__`(çift alt çizgi) JavaScript `getContext().getCollection()` sorgu API'sını kullanırken bir diğer addır.

```javascript
/**
 * Insert an item and update metadata doc: minSize, maxSize, totalSize based on item.size.
 */
function insertDocumentAndUpdateMetadata(item) {
  // HTTP error codes sent to our callback function by CosmosDB server.
  var ErrorCode = {
    RETRY_WITH: 449,
  }

  var isAccepted = __.createDocument(__.getSelfLink(), item, {}, function(err, item, options) {
    if (err) throw err;

    // Check the item (ignore items with invalid/zero size and metadata itself) and call updateMetadata.
    if (!item.isMetadata && item.size > 0) {
      // Get the metadata. We keep it in the same container. it's the only item that has .isMetadata = true.
      var result = __.filter(function(x) {
        return x.isMetadata === true
      }, function(err, feed, options) {
        if (err) throw err;

        // We assume that metadata item was pre-created and must exist when this script is called.
        if (!feed || !feed.length) throw new Error("Failed to find the metadata item.");

        // The metadata item.
        var metaItem = feed[0];

        // Update metaDoc.minSize:
        // for 1st document use doc.Size, for all the rest see if it's less than last min.
        if (metaItem.minSize == 0) metaItem.minSize = item.size;
        else metaItem.minSize = Math.min(metaItem.minSize, item.size);

        // Update metaItem.maxSize.
        metaItem.maxSize = Math.max(metaItem.maxSize, item.size);

        // Update metaItem.totalSize.
        metaItem.totalSize += item.size;

        // Update/replace the metadata item in the store.
        var isAccepted = __.replaceDocument(metaItem._self, metaItem, function(err) {
          if (err) throw err;
          // Note: in case concurrent updates causes conflict with ErrorCode.RETRY_WITH, we can't read the meta again
          //       and update again because due to Snapshot isolation we will read same exact version (we are in same transaction).
          //       We have to take care of that on the client side.
        });
        if (!isAccepted) throw new Error("replaceDocument(metaItem) returned false.");
      });
      if (!result.isAccepted) throw new Error("filter for metaItem returned false.");
    }
  });
  if (!isAccepted) throw new Error("createDocument(actual item) returned false.");
}
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB'de depolanan yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevler hakkında bilgi edinmek için aşağıdaki makalelere bakın:

* [Azure Cosmos DB'de depolanan yordamlar, tetikleyiciler, kullanıcı tanımlı işlevlerle nasıl çalışılı?](how-to-use-stored-procedures-triggers-udfs.md)

* [Azure Cosmos DB'de depolanan yordamları kaydetme ve kullanma](how-to-use-stored-procedures-triggers-udfs.md#stored-procedures)

* Azure Cosmos DB'de [ön tetikleyiciler](how-to-use-stored-procedures-triggers-udfs.md#pre-triggers) ve [tetikleyiciler](how-to-use-stored-procedures-triggers-udfs.md#post-triggers) kaydetme ve kullanma

* [Azure Cosmos DB'de kullanıcı tanımlı işlevleri kaydetme ve kullanma](how-to-use-stored-procedures-triggers-udfs.md#udfs)

* [Azure Cosmos DB'de yapay bölümleme anahtarları](synthetic-partition-keys.md)
