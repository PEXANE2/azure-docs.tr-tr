---
title: CSV bloblarını Azure Search blob Indexer ile dizinle Azure Search
description: Azure Search bir dizin kullanarak tam metin araması için Azure Blob depolamada CSV bloblarını gezin. Dizin oluşturucular, Azure Blob depolama gibi seçili veri kaynakları için veri alımını otomatik hale getirir.
ms.date: 05/02/2019
author: mgottein
manager: nitinme
ms.author: magottei
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: b135fd1a0758567a7b504996bf442a913741fe59
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69656767"
---
# <a name="indexing-csv-blobs-with-azure-search-blob-indexer"></a>Azure Search blob Indexer ile CSV bloblarını dizine ekleme

> [!Note]
> delimitedText ayrıştırma modu önizlemededir ve üretim kullanımı için tasarlanmamıştır. [REST API sürüm 2019-05-06-önizleme](search-api-preview.md) bu özelliği sağlar. Şu anda .NET SDK desteği yok.
>

Varsayılan olarak, [Azure Search blob Indexer](search-howto-indexing-azure-blob-storage.md) , ayrılmış metin bloblarını tek bir metin öbeği olarak ayrıştırır. Ancak, CSV verileri içeren bloblarla, genellikle blobdaki her satırı ayrı bir belge olarak değerlendirmek istersiniz. Örneğin, aşağıdaki sınırlandırılmış metin verildiğinde, her biri "ID", "Dateyayınlanan" ve "Tags" alanlarını içeren iki belgeye ayrıştırın: 

    id, datePublished, tags
    1, 2016-01-12, "azure-search,azure,cloud" 
    2, 2016-07-07, "cloud,mobile" 

Bu makalede `delimitedText` ayrıştırma modunu ayarlama indexerby blob Azure Search ile CSV bloblarını ayrıştırmanın nasıl yapılacağını öğreneceksiniz. 

> [!NOTE]
> Birden çok arama belgesini bir Azure blobundan çıkarmak için [bire çok dizin oluşturma](search-howto-index-one-to-many-blobs.md) içindeki Dizin Oluşturucu yapılandırma önerilerini izleyin.

## <a name="setting-up-csv-indexing"></a>CSV dizinlemeyi ayarlama
CSV bloblarını indekslemek için, bir Dizin Oluşturucu `delimitedText` [oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-indexer) isteğinde ayrıştırma moduyla bir Dizin Oluşturucu tanımı oluşturun veya güncelleştirin:

    {
      "name" : "my-csv-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "firstLineContainsHeaders" : true } }
    }

`firstLineContainsHeaders`Her Blobun ilk (boş olmayan) satırın üstbilgiler içerdiğini belirtir.
Blob 'lar bir başlangıç üst bilgisi satırı içermiyorsa, üst bilgiler Dizin Oluşturucu yapılandırmasında belirtilmelidir: 

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } } 

`delimitedTextDelimiter` Yapılandırma ayarını kullanarak sınırlayıcı karakteri özelleştirebilirsiniz. Örneğin:

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextDelimiter" : "|" } }

> [!NOTE]
> Şu anda yalnızca UTF-8 kodlaması desteklenir. Diğer kodlamalar için desteğe ihtiyacınız varsa [UserVoice](https://feedback.azure.com/forums/263029-azure-search)üzerinde oy verin.

> [!IMPORTANT]
> Sınırlandırılmış metin ayrıştırma modunu kullandığınızda Azure Search, veri kaynağınızdaki tüm Blobların CSV olacağını varsayar. Aynı veri kaynağında CSV ve CSV olmayan Blobların bir karışımını desteketmeniz gerekiyorsa lütfen [UserVoice](https://feedback.azure.com/forums/263029-azure-search)üzerinde oy verin.
> 
> 

## <a name="request-examples"></a>İstek örnekleri
Tümünü bir araya getirmek, tüm yük örnekleri aşağıda verilmiştir. 

DataSource 

    POST https://[service name].search.windows.net/datasources?api-version=2019-05-06-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "<optional, my-folder>" }
    }   

Dizinleyic

    POST https://[service name].search.windows.net/indexers?api-version=2019-05-06-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-csv-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } }
    }

## <a name="help-us-make-azure-search-better"></a>Azure Search daha iyi hale getirmemize yardımcı olun
Geliştirmeler için özellik istekleriniz veya fikirler varsa, bu girişi [UserVoice](https://feedback.azure.com/forums/263029-azure-search/)üzerinde belirtin.

