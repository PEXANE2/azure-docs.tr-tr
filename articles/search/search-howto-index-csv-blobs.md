---
title: CSV Blobları üzerinde arama
titleSuffix: Azure Cognitive Search
description: DelimitedText ayrıştırma modunu kullanarak CSV 'yi Azure Blob depolamadan ayıklayın ve içeri aktarın.
manager: nitinme
author: mgottein
ms.author: magottei
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 580c6294856145530e354b6e5cced955dbaa9f9c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85565567"
---
# <a name="how-to-index-csv-blobs-using-delimitedtext-parsing-mode-and-blob-indexers-in-azure-cognitive-search"></a>Azure Bilişsel Arama ile aynı Demitedmetin ayrıştırma modunu ve BLOB Dizin oluşturucularını kullanarak CSV bloblarını dizin oluşturma

Varsayılan olarak, [Azure bilişsel arama blob Indexer](search-howto-indexing-azure-blob-storage.md) , ayrılmış metin bloblarını tek bir metin öbeği olarak ayrıştırır. Ancak, CSV verileri içeren bloblarla, genellikle blobdaki her satırı ayrı bir belge olarak değerlendirmek istersiniz. Örneğin, aşağıdaki sınırlandırılmış metin verildiğinde, her biri "ID", "Dateyayınlanan" ve "Tags" alanlarını içeren iki belgeye ayrıştırın: 

    id, datePublished, tags
    1, 2016-01-12, "azure-search,azure,cloud" 
    2, 2016-07-07, "cloud,mobile" 

Bu makalede, ayrıştırma modunu ayarlayarak CSV bloblarını bir Azure Bilişsel Arama blob Dizin Oluşturucu ile ayrıştırmayı öğreneceksiniz `delimitedText` . 

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

Yapılandırma ayarını kullanarak sınırlayıcı karakteri özelleştirebilirsiniz `delimitedTextDelimiter` . Örneğin:

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextDelimiter" : "|" } }

> [!NOTE]
> Şu anda yalnızca UTF-8 kodlaması desteklenir. Diğer kodlamalar için desteğe ihtiyacınız varsa [UserVoice](https://feedback.azure.com/forums/263029-azure-search)üzerinde oy verin.

> [!IMPORTANT]
> Ayrılmış metin ayrıştırma modunu kullandığınızda Azure Bilişsel Arama, veri kaynağınızdaki tüm Blobların CSV olacağını varsayar. Aynı veri kaynağında CSV ve CSV olmayan Blobların bir karışımını desteketmeniz gerekiyorsa lütfen [UserVoice](https://feedback.azure.com/forums/263029-azure-search)üzerinde oy verin.
> 
> 

## <a name="request-examples"></a>İstek örnekleri
Tümünü bir araya getirmek, tüm yük örnekleri aşağıda verilmiştir. 

DataSource 

    POST https://[service name].search.windows.net/datasources?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "<optional, my-folder>" }
    }   

Dizinleyic

    POST https://[service name].search.windows.net/indexers?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-csv-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } }
    }

## <a name="help-us-make-azure-cognitive-search-better"></a>Azure Bilişsel Arama daha iyi hale getirmemize yardımcı olun
Geliştirmeler için özellik istekleriniz veya fikirler varsa, bu girişi [UserVoice](https://feedback.azure.com/forums/263029-azure-search/)üzerinde belirtin.

