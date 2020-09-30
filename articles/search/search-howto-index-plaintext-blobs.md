---
title: Düz metin Blobları üzerinde arama
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama 'de tam metin araması için Azure Bloblarından düz metin ayıklamak üzere bir arama Dizin Oluşturucu yapılandırın.
manager: nitinme
author: mgottein
ms.author: magottei
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 09/25/2020
ms.openlocfilehash: 417bdacc3ce8b619d5ec9618e6060ac071882471
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/29/2020
ms.locfileid: "91533934"
---
# <a name="how-to-index-plain-text-blobs-in-azure-cognitive-search"></a>Azure Bilişsel Arama düz metin bloblarını dizin oluşturma

Tam metin arama için aranabilir metin ayıklamak üzere bir [BLOB Dizin Oluşturucu](search-howto-indexing-azure-blob-storage.md) kullanırken, daha iyi dizin oluşturma sonuçları elde etmek için çeşitli ayrıştırma modlarını çağırabilirsiniz. Varsayılan olarak, Dizin Oluşturucu ayrılmış metin bloblarını tek bir metin öbeği olarak ayrıştırır. Ancak, bloblarınız aynı kodlamada düz metin içeriyorsa, **metin ayrıştırma modunu**kullanarak dizin oluşturma performansını önemli ölçüde artırabilirsiniz.

## <a name="set-up-plain-text-indexing"></a>Düz metin dizinlemeyi ayarlama

Düz metin bloblarını dizine eklemek için, bir dizin `parsingMode` oluşturma isteğinde yapılandırma özelliği ile bir Dizin Oluşturucu tanımı oluşturun veya güncelleştirin `text` : [Create Indexer](/rest/api/searchservice/create-indexer)

```http
    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "parsingMode" : "text" } }
    }
```

Varsayılan olarak, `UTF-8` kodlama varsayılır. Farklı bir kodlama belirtmek için `encoding` yapılandırma özelliğini kullanın: 

```http
    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "parsingMode" : "text", "encoding" : "windows-1252" } }
    }
```

## <a name="request-example"></a>İstek örneği

Ayrıştırma modları, Dizin Oluşturucu tanımında belirtilmiştir.

```http
    POST https://[service name].search.windows.net/indexers?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-plaintext-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } }
    }
```

## <a name="help-us-make-azure-cognitive-search-better"></a>Azure Bilişsel Arama daha iyi hale getirmemize yardımcı olun

Geliştirmeler için özellik istekleriniz veya fikirler varsa, bu girişi [UserVoice](https://feedback.azure.com/forums/263029-azure-search/)üzerinde belirtin. Mevcut özelliği kullanarak yardıma ihtiyacınız varsa sorunuzu [Stack Overflow](https://stackoverflow.microsoft.com/questions/tagged/18870)gönderin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Bilişsel Arama'daki Dizin Oluşturucular](search-indexer-overview.md)
* [Blob Dizin oluşturucuyu yapılandırma](search-howto-indexing-azure-blob-storage.md)
* [Blob dizinlemeye genel bakış](search-blob-storage-integration.md)