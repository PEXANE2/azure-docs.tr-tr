---
title: Temel bir sorgu oluşturma
titleSuffix: Azure Cognitive Search
description: Bilişsel Arama, test ve kod için kullanılacak araçlar ve API 'Leri ve sorgu kararlarının Dizin tasarımıyla nasıl başlatılacağını öğrenin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 12/12/2020
ms.openlocfilehash: ace887396bacf264f0ffbd186ef1349e96496786
ms.sourcegitcommit: 1bdcaca5978c3a4929cccbc8dc42fc0c93ca7b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2020
ms.locfileid: "97371224"
---
# <a name="create-a-basic-query-in-azure-cognitive-search"></a>Azure Bilişsel Arama 'de temel sorgu oluşturma

Bu makalede sorgu oluşturma adımı adım adım açıklanmaktadır. Örnekler, portalda **arama** Gezginine kopyalayabilmeniz veya sorguları etkileşimli olarak Postman veya Visual Studio Code kullanarak oluşturmak için bekleyen bir örnektir. Bu makaledeki örnekler için herhangi bir katmanı veya Bilişsel Arama kullanabilirsiniz.

## <a name="choose-a-tool-or-api"></a>Bir araç veya API seçin

Test veya üretim iş yükleri için sorgular oluşturmak üzere aşağıdaki araçlardan ve API 'lerden seçim yapın.

| Yöntem | Açıklama |
|-------------|-------------|
| Portal| [Arama Gezgini (portal)](search-explorer.md) , dizin ve API-sürüm seçimleri için bir arama çubuğu ve seçenekler sağlar. Sonuçlar JSON belgeleri olarak döndürülür. Erken araştırma, test ve doğrulama için önerilir. <br/>[Daha fazla bilgi edinin.](search-explorer.md) |
| Web testi araçları| [Postman veya Visual Studio Code](search-get-started-rest.md) , [arama belgelerinin Rest çağrılarını formülleyen](/rest/api/searchservice/search-documents) güçlü seçimlerdir. REST API, Azure Bilişsel Arama 'daki her programlı işlemi destekler; bu sayede, koda yatırım yapmadan önce nasıl çalıştığını anlamak için istekleri etkileşimli olarak verebilirsiniz.  |
| Azure SDK | [Searchclient (.net)](/dotnet/api/azure.search.documents.searchclient) , C# içindeki bir arama dizinini sorgulamak Için kullanılabilir.  [Daha fazla bilgi edinin.](search-howto-dotnet-sdk.md) <br/><br/>[Searchclient (Python)](/dotnet/api/azure.search.documents.searchclient) , Python 'da bir arama dizinini sorgulamak için kullanılabilir. [Daha fazla bilgi edinin.](search-get-started-python.md) <br/><br/> [Searchclient (JavaScript)](/dotnet/api/azure.search.documents.searchclient) , JavaScript 'te bir arama dizinini sorgulamak için kullanılabilir. [Daha fazla bilgi edinin.](search-get-started-javascript.md)  |

## <a name="set-up-a-search-client"></a>Arama İstemcisi ayarlama

Arama İstemcisi arama hizmetinde kimlik doğrular, istek gönderir ve yanıtları işler. Sorgular her zaman tek bir dizinin belgeler koleksiyonuna yönlendirilir. Bir sorgu hedefi olarak dizinlere katılamaz veya özel veya geçici veri yapıları oluşturamazsınız.

### <a name="in-the-portal"></a>Portalda

Arama Gezgini ve diğer portal araçları, Portal sayfalarından doğrudan erişimli dizinler ve diğer nesnelerle birlikte yerleşik bir istemci bağlantısına sahiptir. Araçlar, sihirbazlar ve nesneler erişimi, hizmette yönetici haklarına sahip olduğunu varsayar. Arama Gezgini ile, arama dizesinin ve diğer parametrelerin belirtilmesine odaklanırsınız. 

### <a name="using-rest"></a>REST kullanma

REST çağrıları için, bir [Arama belgesi](/rest/api/searchservice/search-documents) isteği belirtmek Için [Postman veya benzer araçları](search-get-started-rest.md) istemci olarak kullanabilirsiniz. Her istek tek başına olduğundan, erişim için uç noktayı (hizmetin URL 'SI) ve bir yönetici ya da sorgu API anahtarını sağlamanız gerekir. İsteğe bağlı olarak, URL dizin adı, belge koleksiyonu ve diğer özellikleri de içerebilir. İstek üst bilgisinde Içerik türü ve API anahtarı gibi birkaç özellik geçirilir. Diğer parametreler URL 'ye veya isteğin gövdesine geçirilebilir. Tüm REST çağrıları, kimlik doğrulaması için bir API anahtarı ve bir api sürümü gerektirir.

### <a name="using-azure-sdks"></a>Azure SDK 'larını kullanma

Azure SDK 'Ları, bağlantının yeniden kullanılmasına izin veren, durumu kalıcı hale getirebileceği arama istemcileri sağlar. Sorgu işlemleri için bir SearchClient örneği oluşturacak ve şu özellikler için değerler sağlayacaksınız: uç nokta, anahtar, dizin. Sonra sorgu dizesini sağlamak için arama yöntemini çağırabilirsiniz. 

| Dil | İstemci | Örnek |
|----------|--------|---------|
| C# ve .NET | [SearchClient](/dotnet/api/azure.search.documents.searchclient) | [İlk arama sorgunuzu C 'de gönderin #](/dotnet/api/overview/azure/search.documents-readme#send-your-first-search-query) |
| Python      | [SearchClient](/python/api/azure-search-documents/azure.search.documents.searchclient) | [Python 'da ilk arama sorgunuzu gönderin](/python/api/overview/azure/search-documents-readme#send-your-first-search-request) |
| Java        | [SearchClient](/java/api/com.azure.search.documents.searchclient) | [Java 'da ilk arama sorgunuzu gönderin](/java/api/overview/azure/search-documents-readme#send-your-first-search-query)  |
| JavaScript  | [SearchClient](/javascript/api/@azure/search-documents/searchclient) | [JavaScript 'te ilk arama sorgunuzu gönderin](/javascript/api/overview/azure/search-documents-readme#send-your-first-search-query)  |

## <a name="choose-a-parser-simple--full"></a>Ayrıştırıcı seçin: basit | tümünü

Azure Bilişsel Arama, tipik ve özelleştirilmiş sorguları işlemek için iki sorgu ayrıştırıcıları arasında seçim sağlar. Basit ayrıştırıcı kullanan istekler genellikle tam metin arama sorgundadır, [basit sorgu söz dizimi](query-simple-syntax.md)kullanılarak ifade edilir ve ücretsiz form metin sorgularındaki hız ve verimlilik için varsayılan olarak seçilidir. Bu sözdizimi, AND, OR, NOT, tümcecik, sonek ve öncelik işleçlerini içeren bir dizi yaygın arama işlecini destekler.

İsteğe eklerken etkinleştirilen [tam Lucene sorgu söz dizimi](query-Lucene-syntax.md#bkmk_syntax), `queryType=full` [Apache Lucene](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/classic/package-summary.html)'in bir parçası olarak geliştirilen, yaygın olarak benimsenen ve ifade eden sorgu dilini kullanıma sunar. Tam sözdizimi basit sözdizimini genişletir. Basit sözdizimi için yazdığınız herhangi bir sorgu, tam Lucene ayrıştırıcısı altında çalışır. 

Aşağıdaki örneklerde nokta: aynı sorgu, ancak farklı sonuçlar sağlayan farklı **`queryType`** ayarlarla gösterilmektedir. İlk sorguda, `^3` sonrasında `historic` arama teriminin bir parçası olarak işlenir. Bu sorgu için en üst dereceli sonuç, açıklamasında *okyanus* olan "Marquis plaza & paketleriniz" dır.

```http
POST /indexes/hotels-sample-index/docs/search?api-version=2020-06-30
{
    "count": true,
    "queryType": "simple",
    "search": "ocean historic^3",
    "searchFields": "Description",
    "select": "HotelId, HotelName, Tags, Description",
}
```

Tam Lucene ayrıştırıcısı kullanılarak aynı sorgu, `^3` alan terimi rampa olarak yorumlar. Çözümleyicileri değiştirmek, en üste *geçen dönemi içeren* sonuçlarla birlikte derecelendirmeyi değiştirir.

```http
POST /indexes/hotels-sample-index/docs/search?api-version=2020-06-30
{
    "count": true,
    "queryType": "full",
    "search": "ocean historic^3",
    "searchFields": "Description",
    "select": "HotelId, HotelName, Tags, Description",
}
```

## <a name="enable-query-behaviors-in-an-index"></a>Dizinde sorgu davranışlarını etkinleştir

Dizin tasarımı ve sorgu tasarımı Azure Bilişsel Arama sıkı bir şekilde bağlanmış. *Dizin şeması*, her bir alandaki özniteliklerle, oluşturabileceğiniz sorgu türünü belirler.

Bir alandaki dizin öznitelikleri, izin verilen işlemleri ayarlar; bir alanın dizinde *aranabilir* olup olmadığı, sonuçlarda *alınabilir* , *sıralanabilir*, *filtrelenebilir* ve benzeri. Örnek sorgularda, `"$orderby": "Rating desc"` yalnızca derecelendirme alanı dizin şemasında *sıralanabilir* olarak işaretlendiğinden geçerlidir.

![Otel örneği için Dizin tanımı](./media/search-query-overview/hotel-sample-index-definition.png "Otel örneği için Dizin tanımı")

Yukarıdaki ekran görüntüsü, [otel örnek dizini](search-get-started-portal.md)için dizin özniteliklerinin kısmi bir listesidir. Portalda tüm dizin şemasını oluşturabilir ve görüntüleyebilirsiniz. Dizin öznitelikleri hakkında daha fazla bilgi için bkz. [Dizin oluşturma (REST API)](/rest/api/searchservice/create-index).

## <a name="next-steps"></a>Sonraki adımlar

İsteğin nasıl oluşturulduğunu anladığınıza göre, hem basit hem de tam sözdizimini kullanarak örnekleri deneyin.

+ [Basit sorgu örnekleri](search-query-simple-examples.md)
+ [Lucene sözdizimi sorgu gelişmiş sorgular oluşturmaya yönelik örnekler](search-query-lucene-examples.md)
+ [Azure Bilişsel Arama’da tam metin araması nasıl çalışır?](search-lucene-query-architecture.md)