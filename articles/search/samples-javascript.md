---
title: JavaScript örnekleri
titleSuffix: Azure Cognitive Search
description: JavaScript için Azure .NET SDK 'sını kullanan Azure Bilişsel Arama demo JavaScript kod örneklerini bulun.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/17/2020
ms.openlocfilehash: 6bcdb4a48f71e28514229116c10bd25747b55616
ms.sourcegitcommit: e2dc549424fb2c10fcbb92b499b960677d67a8dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2020
ms.locfileid: "94701832"
---
# <a name="javascript-code-samples-for-azure-cognitive-search"></a>Azure Bilişsel Arama için JavaScript kod örnekleri

Azure Bilişsel Arama özelliklerini ve işlevselliğini gösteren JavaScript kod örnekleri hakkında bilgi edinin. Birincil depolar aşağıdaki gibidir:

| Depo | Description |
|------------|-------------|
| [Azure-SDK-for-js/Tree/Master/SDK/arama/arama-belgeler](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents) | Azure SDK ekibi tarafından oluşturulan ve SDK 'daki Azure.Search.Documstalar istemci kitaplığı ile birlikte gelen örnekler. Ayrıca, çeşitli API 'Lerin nasıl çağrıldığını görmek için istemci kitaplığı için [birim testlerini](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/test) gözden geçirebilirsiniz. |
| [Azure-Samples/Azure-Search-JavaScript-Samples](https://github.com/Azure-Samples/azure-search-javascript-samples) | [Hızlı başlangıç: JavaScript 'te bir arama dizini oluşturma](search-get-started-javascript.md)da dahil olmak üzere nasıl yapılır makalelerine eşlik eden kod örnekleri.|

> [!Tip]
> GitHub 'da ürün, hizmet ve dile göre filtrelenmiş Microsoft kod örneklerini aramak için [örnekler tarayıcısını](/samples/browse/?languages=csharp&products=azure-cognitive-search) deneyin.

## <a name="javascript-sdk-samples"></a>JavaScript SDK örnekleri

Java için Azure SDK 'Sı, paket yükleme, istemci kurulumu ve sorun gidermeyi kapsayan çok sayıda örnek ve [Başlarken sayfası](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/search/azure-search-documents/README.md#getting-started) içerir. Bu sayfada, sizin rahatınız için aşağıda listelenen aşağıdaki örnek kategoriler de açıklanmaktadır.

| Örnekler | Description |
|---------|-------------|
| [dizinlerde](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples/javascript/src/indexes) | [Arama dizinlerini](search-what-is-an-index.md)oluşturma, güncelleştirme, alma, listeleme ve silme işlemlerinin nasıl yapılacağını gösterir. Bu örnek kategori Ayrıca bir hizmet istatistiği örneği içerir. |
| [dataSourceConnections (Dizin oluşturucular için)](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples/javascript/src/dataSourceConnections) | [Desteklenen Azure veri kaynaklarının](search-indexer-overview.md#supported-data-sources)Dizin Oluşturucu tabanlı dizin oluşturma için gerekli olan Dizin Oluşturucu veri kaynaklarını oluşturma, güncelleştirme, alma, listeleme ve silme işlemlerinin nasıl yapılacağını gösterir. |
| [Dizinleyiciler](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples/javascript/src/indexers) |  [Dizin oluşturucular](search-indexer-overview.md)oluşturma, güncelleştirme, alma, listeleme, sıfırlama ve silme işlemlerinin nasıl yapılacağını gösterir.|
| [Beceri](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples/javascript/src/skillSets) |   Bağlı Dizin oluşturucular olan ve dizin oluşturma sırasında AI tabanlı zenginleştirme gerçekleştiren [becerileri](cognitive-search-working-with-skillsets.md) oluşturma, güncelleştirme, alma, listeleme ve silme işlemlerinin nasıl yapılacağını gösterir. |
| [Eş anlamlı eşlemeler](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples/javascript/src/synonymMaps) | [Eş anlamlı haritalar](search-synonyms.md)oluşturma, güncelleştirme, alma, listeleme ve silme işlemlerinin nasıl yapılacağını gösterir.  |
| [Sorgular](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/search/search-documents/samples/javascript/src/readonlyQuery.js) | Microsoft tarafından barındırılan salt okunurdur bir ortak dizine karşı sorgu yürütmeyi gösterir.  |

## <a name="typescript-samples"></a>TypeScript örnekleri

Ayrıca, SDK size kolaylık sağlaması için aşağıda listelenen TypeScript örnekleri de sağlar.

| Örnekler | Description |
|---------|-------------|
| [dizinlerde](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples/typescript/src/indexes) | [Arama dizinlerini](search-what-is-an-index.md)oluşturma, güncelleştirme, alma, listeleme ve silme işlemlerinin nasıl yapılacağını gösterir. Bu örnek kategori Ayrıca bir hizmet istatistiği örneği içerir. |
| [dataSourceConnections (Dizin oluşturucular için)](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples/typescript/src/dataSourceConnections) | [Desteklenen Azure veri kaynaklarının](search-indexer-overview.md#supported-data-sources)Dizin Oluşturucu tabanlı dizin oluşturma için gerekli olan Dizin Oluşturucu veri kaynaklarını oluşturma, güncelleştirme, alma, listeleme ve silme işlemlerinin nasıl yapılacağını gösterir. |
| [Dizinleyiciler](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples/typescript/src/indexers) |  [Dizin oluşturucular](search-indexer-overview.md)oluşturma, güncelleştirme, alma, listeleme, sıfırlama ve silme işlemlerinin nasıl yapılacağını gösterir.|
| [Beceri](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples/typescript/src/skillSets) |   Bağlı Dizin oluşturucular olan ve dizin oluşturma sırasında AI tabanlı zenginleştirme gerçekleştiren [becerileri](cognitive-search-working-with-skillsets.md) oluşturma, güncelleştirme, alma, listeleme ve silme işlemlerinin nasıl yapılacağını gösterir. |
| [Eş anlamlı eşlemeler](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/search/search-documents/samples/typescript/src/synonymMaps) | [Eş anlamlı haritalar](search-synonyms.md)oluşturma, güncelleştirme, alma, listeleme ve silme işlemlerinin nasıl yapılacağını gösterir.  |
| [Sorgular](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/search/search-documents/samples/typescript/src/readonlyQuery.ts) | Microsoft tarafından barındırılan salt okunurdur bir ortak dizine karşı sorgu yürütmeyi gösterir.  |

## <a name="documentation-samples"></a>Belge örnekleri

Aşağıdaki örneklerde [Azure bilişsel arama belgelerinde](https://docs.microsoft.com/azure/search/)ilişkili bir makale bulunur.

| Örnekler | Description | 
|---------|-------------|
| [hızlı başlangıç](https://github.com/Azure-Samples/azure-search-javascript-samples/tree/master/quickstart/v11) | Hızlı başlangıç için kaynak kodu [: JavaScript 'te bir arama dizini oluşturun](search-get-started-javascript.md).  |

## <a name="standalone-samples"></a>Tek başına örnekler

| Örnekler | Description |
|---------|-------------|
| [Azure-Arama-tepki-şablon](https://github.com/dereklegenzoff/azure-search-react-template) | Azure Bilişsel Arama için tepki şablonu (github.com) |