---
title: CSV lekeleri üzerinde arama yapın
titleSuffix: Azure Cognitive Search
description: Sınırlı Metin ayrıştma modunu kullanarak Azure Blob depolamadan CSV ayıklayın ve içe aktarın.
manager: nitinme
author: mgottein
ms.author: magottei
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: bf600890bfed570e712a159005b8ef5267298cc0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76122330"
---
# <a name="how-to-index-csv-blobs-using-delimitedtext-parsing-mode-and-blob-indexers-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da sınırlı Metin ayrıştma modunu ve Blob dizinleyicilerini kullanarak CSV bloblarını dizinleme

Varsayılan olarak, [Azure Bilişsel Arama blob dizinleyici](search-howto-indexing-azure-blob-storage.md) tek bir metin yığını olarak sınırlı metin lekeleri parses. Ancak, CSV verileri içeren lekelerle, genellikle blob'daki her satırı ayrı bir belge olarak ele almak istersiniz. Örneğin, aşağıdaki sınırlı metin göz önüne alındığında, her biri "id", "datePublished" ve "tags" alanlarını içeren iki belgeye ayrıştırmak isteyebilirsiniz: 

    id, datePublished, tags
    1, 2016-01-12, "azure-search,azure,cloud" 
    2, 2016-07-07, "cloud,mobile" 

Bu makalede, ayrıştırma modunu `delimitedText` ayarlayarak Azure Bilişsel Arama blob dizinleyicisi ile CSV lekeleri ayrıştırma öğreneceksiniz. 

> [!NOTE]
> Bir Azure blob'undan birden çok arama belgesi çıkarmak için [birden çok](search-howto-index-one-to-many-blobs.md) dizin oluşturmada dizin leyici yapılandırma önerilerini izleyin.

## <a name="setting-up-csv-indexing"></a>CSV dizin oluşturma
CSV blobs dizine dizin, oluşturmak `delimitedText` [dizinleyici](https://docs.microsoft.com/rest/api/searchservice/create-indexer) isteği üzerinde ayrıştırma modu ile bir dizinleyici tanımı oluşturmak veya güncelleştirmek için:

    {
      "name" : "my-csv-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "firstLineContainsHeaders" : true } }
    }

`firstLineContainsHeaders`her blob'un ilk (boş olmayan) satırının üstbilgi içerdiğini gösterir.
Blobs bir başlangıç üstbilgi satırı içermiyorsa, üstbilgi dizinleyici yapılandırmasında belirtilmelidir: 

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } } 

Yapılandırma ayarını `delimitedTextDelimiter` kullanarak sınır layıcı karakteri özelleştirebilirsiniz. Örnek:

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextDelimiter" : "|" } }

> [!NOTE]
> Şu anda yalnızca UTF-8 kodlaması desteklenir. Diğer kodlamalar için desteğe ihtiyacınız varsa, [UserVoice'ta](https://feedback.azure.com/forums/263029-azure-search)oy verin.

> [!IMPORTANT]
> Sınırlı metin ayrıştırma modunu kullandığınızda, Azure Bilişsel Arama, veri kaynağınızdaki tüm lekelerin CSV olacağını varsayar. Aynı veri kaynağında CSV ve CSV olmayan blobs bir karışımını desteklemek gerekiyorsa, [UserVoice](https://feedback.azure.com/forums/263029-azure-search)bunun için oy lütfen.
> 
> 

## <a name="request-examples"></a>Örnekler iste
Tüm bunları bir araya getirerek, işte tam yük örnekleri. 

Datasource: 

    POST https://[service name].search.windows.net/datasources?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "<optional, my-folder>" }
    }   

Dizinleyici:

    POST https://[service name].search.windows.net/indexers?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-csv-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } }
    }

## <a name="help-us-make-azure-cognitive-search-better"></a>Azure Bilişsel Arama'yı daha iyi hale getirmemize yardımcı olun
Geliştirmeler için özellik istekleriniz veya fikirleriniz varsa, [UserVoice'a](https://feedback.azure.com/forums/263029-azure-search/)girişinizi sağlayın.

