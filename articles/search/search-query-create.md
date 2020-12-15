---
title: Temel bir sorgu oluşturma
titleSuffix: Azure Cognitive Search
description: Bilişsel Arama, test ve kod için kullanılacak araçlar ve API 'Leri ve sorgu kararlarının Dizin tasarımıyla nasıl başlatılacağını öğrenin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 12/14/2020
ms.openlocfilehash: db36a77d93735b151ad893b7e25ba86f104e7b90
ms.sourcegitcommit: 63d0621404375d4ac64055f1df4177dfad3d6de6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2020
ms.locfileid: "97510473"
---
# <a name="create-a-query-in-azure-cognitive-search"></a>Azure Bilişsel Arama bir sorgu oluşturma

İlk kez bir sorgu oluşturuyorsanız, bu makalede ihtiyacınız olacak araçlar ve API 'Ler, bir sorgu oluşturmak için kullanılan yöntemler ve dizin yapısı ile içeriğinin sorgu sonuçlarını nasıl etkileyebileceği açıklanmaktadır. Sorgu isteğinin neye benzediklerine giriş için [sorgu türleri ve kompozisyonlarla](search-query-overview.md)başlayın.

## <a name="choose-tools-and-apis"></a>Araçlar ve API 'Ler seçin

Sorgu oluşturmak için bir araç veya API gerekir. Aşağıdaki önerilerden herhangi biri, test ve üretim iş yükleri için yararlıdır.

| Yöntem | Açıklama |
|-------------|-------------|
| Portal| [Arama Gezgini (portal)](search-explorer.md) , temel arama hizmetindeki dizinlerde sorgular çalıştıran Azure Portal bir sorgu arabirimidir. Portal, arka plan, öneri veya belge [arama işlemleri için](/rest/api/searchservice/search-documents) arka planda REST API çağrısı yapar.<br/><br/> Önizleme dahil olmak üzere herhangi bir dizini ve REST API sürümünü seçebilirsiniz. Sorgu dizesi, tüm sorgu parametreleri (filtre, seçme, searchFields vb.) desteğiyle basit veya tam sözdizimini kullanabilir. Portalda, bir dizini açtığınızda, alan özniteliklerine kolay erişim için yan yana sekmelerde JSON tanımının yanı sıra arama Gezgini ile çalışabilirsiniz. Sorguları test ederken hangi alanların aranabilir, sıralanabilir, filtrelenebilir ve çok yönlü tablo olduğunu denetleyin. <br/>Erken araştırma, test ve doğrulama için önerilir. [Daha fazla bilgi edinin.](search-explorer.md) |
| Web testi araçları| [Postman veya Visual Studio Code](search-get-started-rest.md) , bir arama belgesi isteğini ve diğer tüm ISTEKLERI, bekleyen bir şekilde [formülleyen](/rest/api/searchservice/search-documents) güçlü seçimlerdir. REST API 'Leri, Azure Bilişsel Arama 'daki olası her bir programlı işlemi destekler ve Postman veya Visual Studio Code gibi bir araç kullandığınızda, özelliğin kod içinde yatırım yapmadan önce nasıl çalıştığını anlamak için istekleri etkileşimli bir şekilde verebilirsiniz. Azure portal bir Web testi aracı, katkıda bulunan veya yönetici haklarına sahip olmadığınız durumlarda iyi bir seçenektir. Bir arama URL 'SI ve sorgu API 'SI anahtarınız olduğu sürece, mevcut bir dizine yönelik sorguları çalıştırmak için araçları kullanabilirsiniz. |
| Azure SDK | Kod yazmaya hazırsanız, .NET, Python, JavaScript veya Java için Azure SDK 'larında Azure.Search.Document istemci kitaplıklarını kullanabilirsiniz. Her SDK kendi yayın zamanlamasında bulunur, ancak bunların tümünde dizinler oluşturabilir ve sorgulayabilirsiniz. <br/><br/>[Searchclient (.net)](/dotnet/api/azure.search.documents.searchclient) , C# içindeki bir arama dizinini sorgulamak Için kullanılabilir.  [Daha fazla bilgi edinin.](search-howto-dotnet-sdk.md)<br/><br/>[Searchclient (Python)](/dotnet/api/azure.search.documents.searchclient) , Python 'da bir arama dizinini sorgulamak için kullanılabilir. [Daha fazla bilgi edinin.](search-get-started-python.md)<br/><br/>[Searchclient (JavaScript)](/dotnet/api/azure.search.documents.searchclient) , JavaScript 'te bir arama dizinini sorgulamak için kullanılabilir. [Daha fazla bilgi edinin.](search-get-started-javascript.md) |

## <a name="set-up-a-search-client"></a>Arama İstemcisi ayarlama

Arama İstemcisi arama hizmetinde kimlik doğrular, istek gönderir ve yanıtları işler. Kullandığınız araç veya API 'yi ne olursa olsun, bir arama istemcisinde aşağıdakiler olmalıdır:

| Özellikler | Açıklama |
|------------|-------------|
| Uç Nokta | Bir arama hizmeti şu biçimde bir URL adreslenebilir: `https://[service-name].search.windows.net` . |
| API erişim anahtarı (yönetici veya sorgu) | Arama hizmetine yönelik isteğin kimliğini doğrular. |
| Dizin adı | Sorgular her zaman tek bir dizinin belgeler koleksiyonuna yönlendirilir. Bir sorgu hedefi olarak dizinlere katılamaz veya özel veya geçici veri yapıları oluşturamazsınız. |
| API sürümü | REST çağrıları `api-version` istek üzerinde açıkça gerektirir. Buna karşılık, Azure SDK 'sindeki istemci kitaplıkları belirli bir REST API sürümüne karşı sürümlüdür. SDK 'lar için `api-version` örtük bir. |

### <a name="in-the-portal"></a>Portalda

Arama Gezgini ve diğer portal araçları, Portal sayfalarından doğrudan erişimli dizinler ve diğer nesnelerle birlikte yerleşik bir istemci bağlantısına sahiptir. Araçlar, sihirbazlar ve nesneler erişimi, hizmette katkıda bulunan rolünde veya üzerinde üyelik gerektirir. 

### <a name="using-rest"></a>REST kullanma

REST çağrıları için, bir [Arama belgesi](/rest/api/searchservice/search-documents) isteği belirtmek Için [Postman veya benzer araçları](search-get-started-rest.md) istemci olarak kullanabilirsiniz. Her istek tek başına yapılır, bu nedenle her istekte uç nokta, dizin adı ve API sürümü sağlamanız gerekir. Diğer özellikler, Içerik türü ve API anahtarı istek üstbilgisine geçirilir. 

Bir dizini sorgulamak için POST veya GET kullanabilirsiniz. İstek gövdesinde belirtilen parametrelerin bulunduğu POST ile çalışmak daha kolay. POST kullanırsanız, URL 'ye dahil ettiğinizden emin olun `docs/search` :

```http
POST https://myservice.search.windows.net/indexes/hotels-sample-index/docs/search?api-version=2020-06-30
{
    "count": true,
    "queryType": "simple",
    "search": "*"
}
```

### <a name="using-azure-sdks"></a>Azure SDK 'larını kullanma

Bir Azure SDK kullanıyorsanız, istemcisini kodda oluşturacaksınız. Tüm SDK 'lar, yeniden bağlantı sağlamak için durumu kalıcı hale getirebileceği arama istemcilerine olanak sağlar. Sorgu işlemleri için bir oluşturur **`SearchClient`** ve şu özellikler için değerler verirsiniz: uç nokta, anahtar, dizin. Sonra **`Search method`** sorgu dizesinde geçirilecek öğesini çağırabilirsiniz. 

| Dil | İstemci | Örnek |
|----------|--------|---------|
| C# ve .NET | [SearchClient](/dotnet/api/azure.search.documents.searchclient) | [İlk arama sorgunuzu C 'de gönderin #](/dotnet/api/overview/azure/search.documents-readme#send-your-first-search-query) |
| Python      | [SearchClient](/python/api/azure-search-documents/azure.search.documents.searchclient) | [Python 'da ilk arama sorgunuzu gönderin](/python/api/overview/azure/search-documents-readme#send-your-first-search-request) |
| Java        | [SearchClient](/java/api/com.azure.search.documents.searchclient) | [Java 'da ilk arama sorgunuzu gönderin](/java/api/overview/azure/search-documents-readme#send-your-first-search-query)  |
| JavaScript  | [SearchClient](/javascript/api/@azure/search-documents/searchclient) | [JavaScript 'te ilk arama sorgunuzu gönderin](/javascript/api/overview/azure/search-documents-readme#send-your-first-search-query)  |

## <a name="choose-a-parser-simple--full"></a>Ayrıştırıcı seçin: basit | tümünü

Sorgunuz tam metin arama ise, arama parametresinin içeriğini işlemek için bir Ayrıştırıcı kullanılır. Azure Bilişsel Arama iki sorgu ayrıştırıcıları sunmaktadır. Basit ayrıştırıcı [basit sorgu söz dizimini](query-simple-syntax.md)anlamıştır. Bu ayrıştırıcı, ücretsiz form metin sorgularındaki hız ve verimlilik için varsayılan olarak seçilmiştir. Söz dizimi, terim ve tümcecik aramaları için ortak arama işleçlerini (ve veya, DEĞIL) ve ön ek ( `*` ) araması (Seattle ve mevsimin için "Sea *" içinde olduğu gibi) destekler. Genel bir öneri öncelikle basit ayrıştırıcı yapmayı denemenize ve uygulama gereksinimleri daha güçlü sorgular için çağrı yaparsanız tam ayrıştırıcıya geçiş yapmak.

İsteğe eklediğinizde etkinleştirilen [tam Lucene sorgu söz dizimi](query-Lucene-syntax.md#bkmk_syntax), `queryType=full` [Apache Lucene ayrıştırıcısına](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/classic/package-summary.html)dayalıdır.

Tam sözdizimi ve basit sözdizimi, her ikisi de aynı öneki ve Boole işlemlerini destekledikleri, ancak tam sözdizimi daha fazla işleç sağladığı ölçüde çakışıyor. Tam olarak, Boolean ifadelerine yönelik daha fazla işleç ve benzer arama, joker karakter arama, yakınlık araması ve normal ifadeler gibi gelişmiş sorgular için daha fazla işleç vardır.

## <a name="choose-query-methods"></a>Sorgu yöntemlerini seçin

Arama, hüküm veya deyimlerin bir arama kutusundan toplandığı ya da bir sayfadaki tıklama olaylarından oluşan Kullanıcı odaklı bir alıştırmadır. Aşağıdaki tabloda, Kullanıcı girişini, beklenen arama deneyimiyle birlikte toplayabilmeniz için gereken mekanizmalar özetlenmektedir.

| Giriş | Deneyim |
|-------|---------|
| [Arama yöntemi](/rest/api/searchservice/search-documents) | Kullanıcı, işleç içeren veya olmayan bir arama kutusuna hüküm veya ifadeler ve isteği göndermek için ara ' yı tıklatır. Arama, aynı istekteki filtrelerle birlikte kullanılabilir, ancak otomatik tamamlama veya önerilere sahip değildir. |
| [AutoComplete yöntemi](/rest/api/searchservice/autocomplete) | Bir Kullanıcı birkaç karakter ve her yeni karakter yazıldıktan sonra sorgular başlatılır. Yanıt, dizinden tamamlanan bir dizedir. Belirtilen dize geçerliyse, Kullanıcı bu sorguyu hizmete göndermek için ara ' yı tıklatır. |
| [Öneriler yöntemi](/rest/api/searchservice/suggestions) | Otomatik tamamlama işleminde olduğu gibi, Kullanıcı birkaç karakter ve artımlı sorgular oluşturulur. Yanıt, genellikle birkaç benzersiz veya açıklayıcı alana göre temsil edilen eşleşen belgelerin açılan bir listesidir. Seçimlerden herhangi biri geçerliyse Kullanıcı bir tane tıklamasına ve eşleşen belge döndürülür. |
| [Çok yönlü gezinme](/rest/api/searchservice/search-documents#query-parameters) | Sayfada, aramanın kapsamını daraltıp tıklanabilir gezinti bağlantıları veya içerik haritaları gösterilir. Çok yönlü bir gezinti yapısı, ilk sorguya göre dinamik olarak oluşturulur. Örneğin, `search=*` her olası kategoriden oluşan, çok yönlü bir gezinti ağacını doldurmak için. Bir sorgu yanıtından çok yönlü bir gezinti yapısı oluşturulur, ancak bir sonraki sorguyu ifade etmek için de bir mekanizmadır. n REST API başvuru, `facets` bir arama belgeleri işleminin sorgu parametresi olarak belgelenmiştir, ancak parametresi olmadan kullanılabilir `search` .|
| [Filter yöntemi](/rest/api/searchservice/search-documents#query-parameters) | Filtreler, sonuçları daraltmak için modellerle birlikte kullanılır. Ayrıca sayfanın arkasında bir filtre uygulayabilirsiniz, örneğin, dile özgü alanlarla sayfayı başlatmak için. REST API Başvurusu ' nda, `$filter` bir arama belgeleri işleminin sorgu parametresi olarak belgelenmiştir, ancak parametresi olmadan kullanılabilir `search` .|

## <a name="know-your-field-attributes"></a>Alan öznitelerinizi öğrenin

Daha önce [bir sorgu isteğinin temellerini](search-query-overview.md)gözden geçirdiyseniz, sorgu isteğindeki parametrelerin, alanların bir dizinde nasıl ilişkilendirilebilmesine bağlı olduğunu unutmayın. Örneğin, bir sorgu, filtre veya sıralama düzeninde kullanılmak üzere, bir alan *aranabilir*, *filtrelenebilir* ve *sıralanabilir* olmalıdır. Benzer şekilde, sonuçlarda yalnızca *alınabilir* olarak işaretlenen alanlar görünebilir. `search` `filter` Talebinizdeki, ve parametrelerini belirtmeye başladığınızda `orderby` , beklenmedik sonuçlara engel olmak için öznitelikleri giderek kontrol ettiğinizden emin olun.

[Oteller örnek dizininin](search-get-started-portal.md)altındaki Portal ekran görüntüsünde, yalnızca son Iki "LastRenovationDate" ve "derecelendirme" alanları `"$orderby"` tek bir yan tümcesinde kullanılabilir.

![Otel örneği için Dizin tanımı](./media/search-query-overview/hotel-sample-index-definition.png "Otel örneği için Dizin tanımı")

Alan özniteliklerinin açıklaması için bkz. [Dizin oluşturma (REST API)](/rest/api/searchservice/create-index).

## <a name="know-your-tokens"></a>Belirteçlerinizi öğrenin

Sorgu altyapısı, dizin oluşturma sırasında, dizelerde metin analizi gerçekleştirmek için bir çözümleyici kullanır ve sorgu sırasında eşleşme potansiyelini en üst düzeye çıkarır. En azından, dizeler daha düşük düzeylerdir, ancak aynı zamanda bir yandan da daha fazla hareket edebilir ve Word kaldırma işlemini durdurabilir. Daha büyük dizeler veya bileşik sözcükler genellikle boşluk, kısa çizgi veya tire ile ayrılır ve ayrı belirteçler olarak dizinlenir. 

Burada ele almanız gereken nokta, dizininizin neleri içerdiğini düşündüğünüzden farklı olabilir. Sorgular beklenen sonuçları döndürmezse, çözümleyici tarafından oluşturulan belirteçleri [Çözümle metni (REST API)](/rest/api/searchservice/test-analyzer)aracılığıyla inceleyebilirsiniz. Simgeleştirme ve sorgulara etkisi hakkında daha fazla bilgi için, bkz. [kısmi terim arama ve özel karakterlerle desenler](search-query-partial-matching.md).

## <a name="next-steps"></a>Sonraki adımlar

Bir sorgu isteğinin nasıl oluşturulduğunu daha iyi kavradığınıza göre, uygulamalı deneyim için aşağıdaki hızlı başlangıçlara deneyin.

+ [Arama Gezgini](search-explorer.md)
+ [REST 'te sorgulama](search-get-started-rest.md)
+ [.NET 'te sorgulama](search-get-started-dotnet.md)
+ [Python 'da sorgulama](search-get-started-python.md)
+ [JavaScript 'te sorgulama](search-get-started-javascript.md)