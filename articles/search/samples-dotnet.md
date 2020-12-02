---
title: .NET örnekleri
titleSuffix: Azure Cognitive Search
description: .NET istemci kitaplıklarını kullanan Azure Bilişsel Arama demo C# kod örneklerini bulun.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/17/2020
ms.openlocfilehash: ab6408621616a4be62631391456f73e90fced752
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96499008"
---
# <a name="net-c-code-samples-for-azure-cognitive-search"></a>.NET (C#) Azure Bilişsel Arama için kod örnekleri

Azure Bilişsel Arama özelliklerini ve işlevselliğini gösteren C# kod örnekleri hakkında bilgi edinin. Birincil depolar aşağıdaki gibidir:

| Depo | Açıklama |
|------------|-------------|
| [Azure-SDK-for-NET/SDK/Search/Azure.Search.Documstaları/örnekleri/](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/search/Azure.Search.Documents/samples) | Azure SDK ekibi tarafından oluşturulan ve SDK 'daki Azure.Search.Documstalar istemci kitaplığı ile birlikte gelen örnekler. Ayrıca, çeşitli API 'Lerin nasıl çağrıldığını görmek için istemci kitaplığı için [birim testlerini](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/search/Azure.Search.Documents/tests) gözden geçirebilirsiniz. |
| [Azure-Samples/Azure-Search-DotNet-Samples](https://github.com/Azure-Samples/azure-search-dotnet-samples) | [.NET istemci kitaplığının nasıl kullanılacağı](search-howto-dotnet-sdk.md)da dahil olmak üzere belgelerde nasıl yapılır makalelerine eşlik eden örnekler.|
| [Azure-Samples/Search-DotNet-Başlarken](https://github.com/Azure-Samples/search-dotnet-getting-started) | Belgelerde hızlı başlangıçlara ve öğreticilere eşlik eden örnekler.|

> [!Tip]
> GitHub 'da ürün, hizmet ve dile göre filtrelenmiş Microsoft kod örneklerini aramak için [örnekler tarayıcısını](/samples/browse/?languages=csharp&products=azure-cognitive-search) deneyin.

## <a name="net-sdk-samples"></a>.NET SDK örnekleri

.NET için Azure SDK 'Sı birçok örnek ve her birini açıklayan bir [örnek Benioku dosyası](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/samples/README.md) içerir. Bu liste kolaylık sağlaması için aşağıda verilmiştir.

| Örnekler | Açıklama |
|---------|-------------|
| ["Merhaba Dünya", zaman uyumlu](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/samples/Sample01a_HelloWorld.md) | Zaman uyumlu yöntemleri kullanarak bir istemci oluşturma, kimlik doğrulama ve hata işleme işlemlerinin nasıl yapılacağını gösterir.|
| ["Merhaba Dünya", zaman uyumsuz](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/samples/Sample01b_HelloWorldAsync.md) | Zaman uyumsuz yöntemler kullanarak istemci oluşturma, kimlik doğrulama ve hata işleme işlemlerinin nasıl yapılacağını gösterir.  |
| [Hizmet düzeyi işlemleri](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/samples/Sample02_Service.md) | Dizinler, Dizin oluşturucular, veri kaynakları, becerileri ve eş anlamlı haritalar oluşturmayı gösterir. Bu örnek ayrıca hizmet istatistiklerini ve bir dizinin nasıl sorgulanalınacağını da gösterir.  |
| [Dizin işlemleri](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/samples/Sample03_Index.md) | Bu örnekte, dizinde depolanan belge sayısını almak için mevcut dizin üzerinde bir eylemin nasıl gerçekleştirileceğini gösterir.  |
| [Fieldbuilderıgnore](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/samples/Sample04_FieldBuilderIgnore.md) | Desteklenmeyen veri türleriyle çalışmak için bir teknik gösterir.  |
| [Belgeleri dizinleme (gönderim modeli)](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/samples/Sample05_IndexingDocuments.md) | Bir hizmette bir dizine JSON yükü gönderdiğiniz "Push" model dizin oluşturma.   |
| [Şifreleme anahtarı örneği](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/samples/Sample06_EncryptedIndex.md) | Gizli içeriğe ek bir koruma katmanı eklemek için müşteri tarafından yönetilen bir şifreleme anahtarı kullanmayı gösterir.  |

## <a name="documentation-samples"></a>Belge örnekleri

Aşağıdaki örneklerde [Azure bilişsel arama belgelerinde](./index.yml)ilişkili bir makale bulunur.

| Örnekler | Açıklama |
|---------|-------------|
| [hızlı başlangıç](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/quickstart) | Hızlı başlangıç için kaynak kodu [: arama dizini oluşturun ](search-get-started-dotnet.md).  |
| [DotNetHowTo](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo)  | [.NET istemci kitaplığının kullanımı](search-howto-dotnet-sdk.md) için kaynak kodu |
| [Dotnethowtoeşanlamlı](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToSynonyms)  | Eş anlamlı listeler, sorgu genişletmesi için kullanılır ve bu, bir dizinin dışında eşleşen terimler sağlar. Bu örnek örnek [: C# dilinde eşanlamlılar ekleme](search-synonyms-tutorial-sdk.md). |
| [Dotnettodizinleyicileri](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToIndexers) | Çeşitli makalelerde Dizin Oluşturucu ile ilgili kod parçacıklarının arkasındaki kaynak kodu. Bu örnek, bir zamanlama, alan eşlemeleri ve parametreleri olan bir dizin oluşturucunun nasıl yapılandırılacağını gösterir.  |
| [DotNetHowToEncryptionUsingCMK](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToEncryptionUsingCMK)  | [Veri şifreleme için müşteri tarafından yönetilen anahtarların nasıl yapılandırılacağı hakkında](search-security-manage-encryption-keys.md) kaynak kodu |
| [C 'de ilk uygulamanızı oluşturma #](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/create-first-app/v11) |  Öğretici için kaynak kodu [: ilk arama uygulamanızı oluşturun](tutorial-csharp-create-first-app.md). Çoğu örnek konsol uygulamaları olsa da, bu MVC örneği örnek oteller dizinini ön temel arama, sayfalandırma, otomatik tamamlama ve önerilen sorguları, modelleri ve filtreleri gösteren bir Web sayfası kullanır. |
| [çoklu veri kaynakları](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/multiple-data-sources)  | Öğretici için kaynak kodu [: birden çok veri kaynağından Dizin](tutorial-multiple-data-sources.md). |
|  [en iyileştirme-veri dizini oluşturma](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/optimize-data-indexing) | Öğretici için kaynak kodu [: gönderme API 'si ile dizin oluşturmayı iyileştirin](tutorial-optimize-indexing-push-api.md).  |
| [öğretici-AI-zenginleştirme](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/tutorial-ai-enrichment)  | Öğretici için kaynak kodu [: .NET SDK kullanarak Azure Bloblarından AI tarafından oluşturulan aranabilir içerik](cognitive-search-tutorial-blob-dotnet.md).  |

## <a name="standalone-samples-and-solutions"></a>Tek başına örnekler ve çözümler

| Örnekler | Açıklama |
|---------|-------------|
| [Azure-Arama-güç-beceriler](https://github.com/Azure-Samples/azure-search-power-skills)  | Kazanılmış çözümlerinizde birleştirebileceğiniz, tüketilebilir özel yetenekler için kaynak kodu.  |
| [Bilgi Madenciliği Çözüm Hızlandırıcısı](/samples/azure-samples/azure-search-knowledge-mining/azure-search-knowledge-mining/) | , Bir uçtan uca bilgi araştırma çözümü prototipinizde yardımcı olacak şablonlar, destek dosyaları ve analitik raporlar içerir.  |
| [Covıd-19 uygulama deposunu ara](https://github.com/liamca/covid19search) | Bilişsel Arama tabanlı [Covıd-19 arama uygulaması](https://covid19search.azurewebsites.net/) için kaynak kodu deposu |
| [JFK](https://github.com/Microsoft/AzureSearch_JFK_Files) | [JFK çözümü](https://www.microsoft.com/ai/ai-lab-jfk-files)hakkında daha fazla bilgi edinin. |