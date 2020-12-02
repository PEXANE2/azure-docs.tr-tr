---
title: Java örnekleri
titleSuffix: Azure Cognitive Search
description: Java için Azure .NET SDK 'sını kullanan Azure Bilişsel Arama demo Java kod örneklerini bulun.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/17/2020
ms.openlocfilehash: 10dff18f7b9db7273fcd6ec92bcca5970bb83b08
ms.sourcegitcommit: df66dff4e34a0b7780cba503bb141d6b72335a96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96510378"
---
# <a name="java-code-samples-for-azure-cognitive-search"></a>Azure Bilişsel Arama için Java kod örnekleri

Azure Bilişsel Arama özelliklerini ve işlevselliğini gösteren Java kod örnekleri hakkında bilgi edinin. Birincil depolar aşağıdaki gibidir:

| Depo | Açıklama |
|------------|-------------|
| [Azure-SDK-for-Java/Tree/Master/SDK/arama/Azure-Arama-belgeler/src/Samples](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/search/azure-search-documents/src/samples) | Azure SDK ekibi tarafından oluşturulan ve SDK 'daki Azure.Search.Documstalar istemci kitaplığı ile birlikte gelen örnekler. Ayrıca, çeşitli API 'Lerin nasıl çağrıldığını görmek için istemci kitaplığı için [birim testlerini](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/search/azure-search-documents/src/test) gözden geçirebilirsiniz. |
| [Azure-Samples/Azure-Search-Java-Samples](https://github.com/Azure-Samples/azure-search-java-samples) | Nasıl yapılır makalelerine eşlik eden kod örnekleri. **Bu depodaki örnekler henüz Java Için Azure SDK 'sını kullanacak şekilde güncelleştirilmedi**. Şu anda bu örnekler, Java kodundaki REST API 'Lerini çağırır.|

> [!Tip]
> GitHub 'da ürün, hizmet ve dile göre filtrelenmiş Microsoft kod örneklerini aramak için [örnekler tarayıcısını](/samples/browse/?languages=java&products=azure-cognitive-search) deneyin.

## <a name="java-sdk-samples"></a>Java SDK örnekleri

Java için Azure SDK, çok sayıda örnek ve paket yüklemesini kapsayan bir başlangıç [sayfası](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/search/azure-search-documents/src/samples) içerir. Sayfada Ayrıca çok sayıda örnek listelenir. Daha yaygın işlemlerden bazıları aşağıda verilmiştir.

| Örnekler | Açıklama |
|---------|-------------|
| [Arama dizini oluşturma](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/search/azure-search-documents/src/samples/java/com/azure/search/documents/indexes/CreateIndexExample.java) | [Arama dizinlerinin](search-what-is-an-index.md)nasıl oluşturulacağını gösterir. |
| [Eş anlamlı oluşturma](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/search/azure-search-documents/src/samples/java/com/azure/search/documents/SynonymMapsCreateExample.java) | [Eş anlamlı eşlemelerin](search-synonyms.md)nasıl oluşturulacağını gösterir.  |
| [Dizin Oluşturucu oluşturmayı ara](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/search/azure-search-documents/src/samples/java/com/azure/search/documents/indexes/CreateIndexerExample.java) | [Dizin oluşturucular](search-indexer-overview.md)oluşturmayı gösterir. |
| [Dizin Oluşturucu veri kaynağı oluşturmayı ara](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/search/azure-search-documents/src/samples/java/com/azure/search/documents/indexes/DataSourceExample.java) | [Desteklenen Azure veri kaynaklarının](search-indexer-overview.md#supported-data-sources)Dizin Oluşturucu tabanlı dizin oluşturma için gerekli olan Dizin Oluşturucu veri kaynakları oluşturma işleminin nasıl yapılacağını gösterir. |
| [Beceri oluşturma](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/search/azure-search-documents/src/samples/java/com/azure/search/documents/indexes/CreateSkillsetExample.java) |  Bağlı Dizin oluşturucular olan [becerileri](cognitive-search-working-with-skillsets.md) oluşturmayı ve dizin oluşturma sırasında AI tabanlı zenginleştirme yapmayı gösterir. |
| [Belge yükleme](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/search/azure-search-documents/src/samples/java/com/azure/search/documents/IndexContentManagementExample.java) | Bir [veri içeri aktarma](search-what-is-data-import.md) işleminde bulunan bir dizine belge yükleme veya birleştirme işlemini gösterir. |
| [Sorgu söz dizimi](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/search/azure-search-documents/src/samples/java/com/azure/search/documents/SearchAsyncWithFullyTypedDocumentsExample.java) | [Temel bir sorgunun](search-query-overview.md)nasıl ayarlanacağını gösterir. |

## <a name="documentation-samples"></a>Belge örnekleri

Aşağıdaki örneklerde [Azure bilişsel arama belgelerinde](./index.yml)ilişkili bir makale bulunur.

| Örnekler | Açıklama | 
|---------|-------------|
| [hızlı başlangıç](https://github.com/Azure-Samples/azure-search-java-samples/tree/master/quickstart) | Hızlı başlangıç için kaynak kodu [: Java 'da bir arama dizini oluşturun](search-get-started-java.md). Bu örnek REST API 'Lerini çağırır. |
| [Search-Java-Indexer-demo](https://github.com/Azure-Samples/azure-search-java-samples/tree/master/search-java-indexer-demo) | Java 'da bir Azure Cosmos DB Dizin Oluşturucu gösterir. Bu örnek REST API 'Lerini çağırır. |