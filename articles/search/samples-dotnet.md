---
title: .NET örnekleri
titleSuffix: Azure Cognitive Search
description: .NET istemci kitaplıklarını kullanan Azure Bilişsel Arama demo C# kod örneklerini bulun.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/27/2021
ms.openlocfilehash: cbb84a4934eed4d258cf07772753315785f98019
ms.sourcegitcommit: 54e1d4cdff28c2fd88eca949c2190da1b09dca91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2021
ms.locfileid: "99218171"
---
# <a name="net-c-code-samples-for-azure-cognitive-search"></a>.NET (C#) Azure Bilişsel Arama için kod örnekleri

Azure Bilişsel Arama çözümünün işlevselliğini ve iş akışını gösteren C# kod örnekleri hakkında bilgi edinin. Bu örnekler, aşağıdaki bağlantılardan keşfedebileceğiniz [**.net Için Azure SDK**](/dotnet/azure/)için [**Azure bilişsel arama istemci kitaplığını**](/dotnet/api/overview/azure/search) kullanır.

| Hedef | Bağlantı |
|--------|------|
| Paket indirme | [www.nuget.org/packages/Azure.Search.Documtalar/](https://www.nuget.org/packages/Azure.Search.Documents/) |
| API başvurusu | [azure.search.documlar](/dotnet/api/azure.search.documents)  |
| API test çalışmaları | [github.com/Azure/azure-sdk-for-net/tree/master/sdk/search/Azure.Search.Documlar/testler](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/search/Azure.Search.Documents/tests) |
| Kaynak kod | [github.com/Azure/azure-sdk-for-net/tree/master/sdk/search/Azure.Search.Documtalar/src](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/search/Azure.Search.Documents/src)  |

## <a name="sdk-samples"></a>SDK örnekleri

Azure SDK geliştirme ekibinin kod örnekleri API kullanımını gösterir. Bu örnekleri [**Azure/Azure-SDK-for-net/Tree/Master/SDK/Search/Azure.Search.Documstaları/örnekleri Için**](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/samples/) GitHub 'da bulabilirsiniz.

| Örnekler | Description |
|---------|-------------|
| ["Merhaba Dünya", zaman uyumlu](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/samples/Sample01a_HelloWorld.md) | Zaman uyumlu yöntemleri kullanarak bir istemci oluşturma, kimlik doğrulama ve hata işleme işlemlerinin nasıl yapılacağını gösterir.|
| ["Merhaba Dünya", zaman uyumsuz](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/samples/Sample01b_HelloWorldAsync.md) | Zaman uyumsuz yöntemler kullanarak istemci oluşturma, kimlik doğrulama ve hata işleme işlemlerinin nasıl yapılacağını gösterir.  |
| [Hizmet düzeyi işlemleri](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/samples/Sample02_Service.md) | Dizinler, Dizin oluşturucular, veri kaynakları, becerileri ve eş anlamlı haritalar oluşturmayı gösterir. Bu örnek ayrıca hizmet istatistiklerini ve bir dizinin nasıl sorgulanalınacağını da gösterir.  |
| [Dizin işlemleri](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/samples/Sample03_Index.md) | Bu örnekte, dizinde depolanan belge sayısını almak için mevcut dizin üzerinde bir eylemin nasıl gerçekleştirileceğini gösterir.  |
| [Fieldbuilderıgnore](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/samples/Sample04_FieldBuilderIgnore.md) | Desteklenmeyen veri türleriyle çalışmak için bir teknik gösterir.  |
| [Belgeleri dizinleme (gönderim modeli)](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/samples/Sample05_IndexingDocuments.md) | Bir hizmette bir dizine JSON yükü gönderdiğiniz "Push" model dizin oluşturma.   |
| [Şifreleme anahtarı örneği](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/samples/Sample06_EncryptedIndex.md) | Gizli içeriğe ek bir koruma katmanı eklemek için müşteri tarafından yönetilen bir şifreleme anahtarı kullanmayı gösterir.  |

## <a name="doc-samples"></a>Belge örnekleri

Bilişsel Arama ekibinin kod örnekleri, özellikleri ve iş akışlarını gösterir. Bu örneklerin çoğuna öğreticiler, hızlı başlangıçler ve nasıl yapılır makalelerinde başvurulur. Bu örnekleri [**Azure-Samples/Azure-Search-DotNet-Samples**](https://github.com/Azure-Samples/azure-search-dotnet-samples) ve [**Azure-Samples/Search-DotNet-Başlarken-**](https://github.com/Azure-Samples/search-dotnet-getting-started/) GitHub ' da bulabilirsiniz.

| Örnekler | Makale  |
|---------|-------------|
| [hızlı başlangıç](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/quickstart) | Hızlı başlangıç için kaynak kodu [: arama dizini oluşturun ](search-get-started-dotnet.md). Bu makale, örnek verileri kullanarak bir arama dizini oluşturma, yükleme ve sorgulama için temel iş akışını ele almaktadır. |
| [DotNetHowTo](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo)  | [.NET istemci kitaplığının kullanımı](search-howto-dotnet-sdk.md)için kaynak kodu. Bu makalede, temel iş akışında, ancak API kullanımının daha ayrıntılı ve tartışmasındaki adımları izleyin.  |
| [Dotnethowtoeşanlamlı](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToSynonyms)  | Kaynak kodu [Örneğin: C# dilinde eşanlamlılar ekleyin](search-synonyms-tutorial-sdk.md). Eş anlamlı listeler, sorgu genişletmesi için kullanılır ve bu, bir dizinin dışında eşleşen terimler sağlar. |
| [Dotnettodizinleyicileri](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToIndexers) | Öğretici için kaynak kodu [: .NET SDK kullanarak Azure SQL verilerinin dizinini oluştur](search-indexer-tutorial.md). Bu makalede, bir zamanlama, alan eşlemeleri ve parametreleri olan bir Azure SQL dizin oluşturucunun nasıl yapılandırılacağı gösterilmektedir.  |
| [DotNetHowToEncryptionUsingCMK](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToEncryptionUsingCMK)  | [Veri şifreleme için müşteri tarafından yönetilen anahtarların nasıl yapılandırılacağı hakkında](search-security-manage-encryption-keys.md)kaynak kodu. |
| [C 'de ilk uygulamanızı oluşturma #](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/create-first-app/v11) |  Öğretici için kaynak kodu [: ilk arama uygulamanızı oluşturun](tutorial-csharp-create-first-app.md). Çoğu örnek konsol uygulamaları olsa da, bu MVC örneği örnek oteller dizinini ön temel arama, sayfalandırma, otomatik tamamlama ve önerilen sorguları, modelleri ve filtreleri gösteren bir Web sayfası kullanır. |
| [çoklu veri kaynakları](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/multiple-data-sources)  | Öğretici için kaynak kodu [: birden çok veri kaynağından Dizin](tutorial-multiple-data-sources.md). |
|  [en iyileştirme-veri dizini oluşturma](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/optimize-data-indexing) | Öğretici için kaynak kodu [: gönderme API 'si ile dizin oluşturmayı iyileştirin](tutorial-optimize-indexing-push-api.md).  |
| [öğretici-AI-zenginleştirme](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/tutorial-ai-enrichment)  | Öğretici için kaynak kodu [: .NET SDK kullanarak Azure Bloblarından AI tarafından oluşturulan aranabilir içerik](cognitive-search-tutorial-blob-dotnet.md).  |

> [!Tip]
> GitHub 'da ürün, hizmet ve dile göre filtrelenmiş Microsoft kod örneklerini aramak için [örnekler tarayıcısını](/samples/browse/?languages=csharp&products=azure-cognitive-search) deneyin.

## <a name="other-samples"></a>Diğer örnekler

Aşağıdaki örnekler de Bilişsel Arama ekibi tarafından yayımlanır, Ancak belgelerde başvurulmaz. İlişkili Benioku dosyaları Kullanım yönergeleri sağlar.

| Örnekler | Description |
|---------|-------------|
| [Azure-Arama-güç-beceriler](https://github.com/Azure-Samples/azure-search-power-skills)  | Kazanılmış çözümlerinizde birleştirebileceğiniz, tüketilebilir özel yetenekler için kaynak kodu.  |
| [Bilgi Madenciliği Çözüm Hızlandırıcısı](/samples/azure-samples/azure-search-knowledge-mining/azure-search-knowledge-mining/) | , Bir uçtan uca bilgi araştırma çözümü prototipinizde yardımcı olacak şablonlar, destek dosyaları ve analitik raporlar içerir.  |
| [Covıd-19 uygulama deposunu ara](https://github.com/liamca/covid19search) | Bilişsel Arama tabanlı [Covıd-19 arama uygulaması](https://covid19search.azurewebsites.net/) için kaynak kodu deposu |
| [JFK](https://github.com/Microsoft/AzureSearch_JFK_Files) | [JFK çözümü](https://www.microsoft.com/ai/ai-lab-jfk-files)hakkında daha fazla bilgi edinin. |
| [Arama + Soru-Cevap Oluşturma Hızlandırıcısı](https://github.com/Azure-Samples/search-qna-maker-accelerator) | Arama ve Soru-Cevap Oluşturma gücünü birleştiren bir [çözüm](https://techcommunity.microsoft.com/t5/azure-ai/qna-with-azure-cognitive-search/ba-p/2081381) . Bkz. canlı [tanıtım sitesi](https://aka.ms/qnaWithAzureSearchDemo). |