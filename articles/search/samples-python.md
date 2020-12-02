---
title: Python örnekleri
titleSuffix: Azure Cognitive Search
description: Python veya REST için Azure .NET SDK 'sını kullanan Azure Bilişsel Arama demo Python kod örneklerini bulun.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/17/2020
ms.openlocfilehash: 6e0f3d318cc462b03151d5a4935ae318df46e2c5
ms.sourcegitcommit: df66dff4e34a0b7780cba503bb141d6b72335a96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96510565"
---
# <a name="python-code-samples-for-azure-cognitive-search"></a>Azure Bilişsel Arama için Python kod örnekleri

Azure Bilişsel Arama özelliklerini ve işlevselliğini gösteren Python kodu örnekleri hakkında bilgi edinin. Birincil depolar aşağıdaki gibidir:

| Depo | Açıklama |
|------------|-------------|
| [Azure-SDK-for-Python/Tree/Master/SDK/arama/Azure-Arama-belgeler/örnekler/](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/search/azure-search-documents/samples) | Azure SDK ekibi tarafından oluşturulan ve SDK 'daki Azure.Search.Documstalar istemci kitaplığı ile birlikte gelen örnekler. Ayrıca, çeşitli API 'Lerin nasıl çağrıldığını görmek için istemci kitaplığı için [birim testlerini](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/search/azure-search-documents/tests) gözden geçirebilirsiniz. |
| [Azure-Samples/Azure-Search-Python-Samples](https://github.com/Azure-Samples/azure-search-python-samples) | [Hızlı başlangıç: Python 'da arama dizini oluşturma](search-get-started-python.md)dahil olmak üzere nasıl yapılır makalelerine eşlik eden kod örnekleri.|

> [!Tip]
> GitHub 'da ürün, hizmet ve dile göre filtrelenmiş Microsoft kod örneklerini aramak için [örnekler tarayıcısını](/samples/browse/?languages=python&products=azure-cognitive-search) deneyin.

## <a name="python-sdk-samples"></a>Python SDK örnekleri

Python için Azure SDK, çok sayıda örnek ve önkoşulları ve paket yüklemesi içeren bir başlangıç [sayfası](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/search/azure-search-documents/samples) içerir. Bu sayfa, size kolaylık sağlamak için aşağıda listelenen aşağıdaki örneklere bağlantılar da içerir.

| Örnekler | Açıklama |
|---------|-------------|
| [Kimlik doğrulaması](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/search/azure-search-documents/samples/sample_authentication.py) | Bir istemcinin nasıl yapılandırılacağını ve hizmette kimlik doğrulaması yapılacağını gösterir. | 
| [Dizin oluşturma-okuma-güncelleştirme-silme işlemleri](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/search/azure-search-documents/samples/sample_index_crud_operations.py) | [Arama dizinlerini](search-what-is-an-index.md)oluşturma, güncelleştirme, alma, listeleme ve silme işlemlerinin nasıl yapılacağını gösterir. |
| [Dizin Oluşturucu oluşturma-okuma-güncelleştirme-silme işlemleri](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/search/azure-search-documents/samples/sample_indexers_operations.py) | [Dizin oluşturucular](search-indexer-overview.md)oluşturma, güncelleştirme, alma, listeleme, sıfırlama ve silme işlemlerinin nasıl yapılacağını gösterir. |
| [Dizin Oluşturucu veri kaynaklarını ara](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/search/azure-search-documents/samples/sample_indexer_datasource_skillset.py) | [Desteklenen Azure veri kaynaklarının](search-indexer-overview.md#supported-data-sources)Dizin Oluşturucu tabanlı dizin oluşturma için gerekli olan Dizin Oluşturucu veri kaynaklarını oluşturma, güncelleştirme, alma, listeleme ve silme işlemlerinin nasıl yapılacağını gösterir. |
| [Eş anlamlılar](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/search/azure-search-documents/samples/sample_synonym_map_operations.py) | [Eş anlamlı haritalar](search-synonyms.md)oluşturma, güncelleştirme, alma, listeleme ve silme işlemlerinin nasıl yapılacağını gösterir.  |
| [Belge yükleme](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/search/azure-search-documents/samples/sample_crud_operations.py) | Bir [veri içeri aktarma](search-what-is-data-import.md) işleminde bulunan bir dizine belge yükleme veya birleştirme işlemini gösterir. |
| [Basit sorgu](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/search/azure-search-documents/samples/sample_simple_query.py) | [Temel bir sorgunun](search-query-overview.md)nasıl ayarlanacağını gösterir. |
| [Filtre sorgusu](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/search/azure-search-documents/samples/sample_filter_query.py) | [Filtre ifadesi](search-filters.md)ayarlamayı gösterir. |
| [Model sorgusu](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/search/azure-search-documents/samples/sample_facet_query.py) | [Modellerle](search-filters-facets.md)çalışmayı gösterir. |

## <a name="documentation-samples"></a>Belge örnekleri

Aşağıdaki örneklerde [Azure bilişsel arama belgelerinde](./index.yml)ilişkili bir makale bulunur.

| Örnekler | Açıklama | 
|---------|-------------|
| [hızlı başlangıç](https://github.com/Azure-Samples/azure-search-python-samples/tree/master/Quickstart) | Hızlı başlangıç için kaynak kodu [: Python 'da bir arama dizini oluşturun](search-get-started-python.md).  |
| [öğretici-AI-zenginleştirme](https://github.com/Azure-Samples/azure-search-python-samples/tree/master/Tutorial-AI-Enrichment)  | Öğretici için kaynak kodu [: Azure Bloblarından aranabilir içerik oluşturmak Için Python ve AI kullanın](cognitive-search-tutorial-blob-python.md).  |
| [AzureML-özel yetenek](https://github.com/Azure-Samples/azure-search-python-samples/tree/master/AzureML-Custom-Skill)  | Kaynak kodu [Örneğin: Python kullanarak özel bir yetenek oluşturma](cognitive-search-custom-skill-python.md).  |