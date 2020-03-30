---
title: Arama dizinlerinde alma ve veri alma
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama'da harici veri kaynaklarından verileri doldurup bir dizine yükleyin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: cc3f38e9bb96ce76263a3124f8bfdc49dc638bfd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79282763"
---
# <a name="data-import-overview---azure-cognitive-search"></a>Veri alma genel bakış - Azure Bilişsel Arama

Azure Bilişsel Arama'da sorgular, [bir arama dizinine](search-what-is-an-index.md)yüklenen ve kaydedilen içeriğiniz üzerinden yürütülür. Bu makalede, bir dizin doldurmak için iki temel yaklaşım incelenir: verilerinizi dizine programlı olarak *itin* veya verileri *çekmek* için desteklenen bir veri kaynağına bir [Azure Bilişsel Arama dizinleyicisi](search-indexer-overview.md) işaretleyin.

Her iki yaklaşımla da amaç, harici bir veri kaynağından gelen verileri Azure Bilişsel Arama dizinine *yüklemektir.* Azure Bilişsel Arama boş bir dizin oluşturmanıza izin verecektir, ancak verileri içine itene veya çekene kadar sorgulanabilir değildir.

## <a name="pushing-data-to-an-index"></a>Verileri dizine gönderme
Verilerinizi programlı olarak Azure Bilişsel Arama'ya göndermek için kullanılan itme modeli en esnek yaklaşımdır. Birincisi, veri kaynağı türüne hiçbir kısıtlama getirmez. JSON belgelerinden oluşan herhangi bir veri kümesi, veri kümesindeki her belgenin dizin şemanızda tanımlanan alanlara eşleme alanı olduğunu varsayarak Azure Bilişsel Arama dizinine itilebilir. İkincisi, yürütme frekansı üzerinde hiçbir kısıtlaması yoktur. Değişiklikleri istediğiniz sıklıkta dizine gönderebilirsiniz. Çok düşük gecikme süresi gereksinimlerine sahip uygulamalar için (örneğin, arama işlemlerinin dinamik stok veritabanlarıyla eşitlenmiş olması gerekiyorsa), tek seçeneğiniz gönderme modelidir.

Belgeleri tek tek veya toplu işlemle karşıya yükleyebileceğinizden (toplu işlem başına en fazla 1000 veya 16 MB sınırlarından hangisi önce gelirse), bu yaklaşım çekme modelinden daha esnektir. Itme modeli, verilerinizin nerede olduğuna bakılmaksızın belgeleri Azure Bilişsel Arama'ya yüklemenize de olanak tanır.

### <a name="how-to-push-data-to-an-azure-cognitive-search-index"></a>Verileri Azure Bilişsel Arama dizinine nasıl itilir?

Dizin bir tek veya birden çok belge yüklemek için şu API'leri kullanabilirsiniz:

+ [Belge Ekleme, Güncelleştirme veya Silme (REST API)](https://docs.microsoft.com/rest/api/searchservice/AddUpdate-or-Delete-Documents)
+ [indexAction sınıfı](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexaction?view=azure-dotnet) veya [indexBatch sınıfı](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexbatch?view=azure-dotnet) 

Şu an portal aracılığıyla veri gönderme için hiçbir araç desteği yoktur.

Her metodolojiye giriş için [Bkz. Quickstart: PowerShell](search-create-index-rest-api.md) veya C# Quickstart kullanarak Azure Bilişsel [Arama dizini oluşturma: .NET SDK'yı kullanarak Bir Azure Bilişsel Arama dizini oluşturun.](search-get-started-dotnet.md)

<a name="indexing-actions"></a>

### <a name="indexing-actions-upload-merge-mergeorupload-delete"></a>Eylemleri dizinleme: yükleme, birleştirme, birleştirmeOrUpload, silme

Belgenin tam olarak yüklenmesi, varolan belge içeriğiyle birleştirilmesi veya silinmesi gerekip gerekmediğini belirterek, dizin oluşturma eyleminin türünü belge başına olarak denetleyebilirsiniz.

REST API'sinde, JSON istek gövdeleriyle HTTP POST isteklerini Azure Bilişsel Arama dizininizin bitiş noktası URL'sine sorun. "Değer" dizisindeki her JSON nesnesi belgenin anahtarını içerir ve bir dizin oluşturma eyleminin belge içeriğini ekleyip eklemediğini, güncelleyip güncellemediğini veya silip silmediğini belirtir. Kod örneği için, [belgeleri yükleyin'](search-get-started-dotnet.md#load-documents)e bakın.

.NET SDK'da verilerinizi bir `IndexBatch` nesneye paketle. Bir `IndexBatch` nesne koleksiyonunu `IndexAction` kapsüller, her biri bir belge ve azure bilişsel arama bu belgede gerçekleştirmek için hangi eylem söyler bir özellik içerir. Kod örneği için [C# Quickstart'a](search-get-started-dotnet.md)bakın.


| @search.action | Açıklama | Her bir belge için gerekli alanlar | Notlar |
| -------------- | ----------- | ---------------------------------- | ----- |
| `upload` |Bir `upload` eylemi, belgenin yeni olması durumunda ekleneceği ve var olması durumunda güncelleştirileceği/değiştirileceği bir "upsert" ile benzerlik gösterir. |anahtar ve tanımlamak istediğiniz diğer alanlar |Var olan bir belgeyi güncelleştirirken/değiştirirken istekte belirtilmeyen herhangi bir alan `null` olarak ayarlanır. Bu durum, alan daha önce değersiz olmayan bir değere ayarlanmış olsa dahi gerçekleşir. |
| `merge` |Var olan belgeyi belirtilen alanlarla güncelleştirir. Belge dizinde mevcut değilse birleştirme işlemi başarısız olur. |anahtar ve tanımlamak istediğiniz diğer alanlar |Birleştirmede belirttiğiniz herhangi bir alan belgede var olan alanın yerini alır. .NET SDK'da bu tür `DataType.Collection(DataType.String)`alanları içerir. REST API'sinde, bu tür `Collection(Edm.String)`alanları içerir. Örneğin, belge `["budget"]` değerine sahip bir `tags` alanını içeriyorsa ve `tags` için `["economy", "pool"]` değeriyle bir birleştirme yürütürseniz `tags` alanının son değeri `["economy", "pool"]` olur. `["budget", "economy", "pool"]` olmayacaktır. |
| `mergeOrUpload` |Belirtilen anahtara sahip bir belge dizinde zaten mevcutsa bu eylem `merge` gibi davranır. Belge mevcut değilse yeni bir belgeyle `upload` gibi davranır. |anahtar ve tanımlamak istediğiniz diğer alanlar |- |
| `delete` |Belirtilen belgeyi dizinden kaldırır. |yalnızca anahtar |Anahtar alanı dışında belirttiğiniz tüm alanlar yoksayılır. Bir belgeden tek bir alanı kaldırmak istiyorsanız bunun yerine `merge` kullanıp alanı açık bir şekilde null olarak ayarlamanız yeterlidir. |

## <a name="decide-which-indexing-action-to-use"></a>Hangi dizin oluşturma eyleminin kullanılacağına karar verme
.NET SDK kullanarak veri almak için(yükleme, birleştirme, silme ve birleştirmeOrUpload). Yukarıdaki eylemlerden hangisini seçtiğinize bağlı olarak, her bir belgeye yalnızca belirli alanlar dahil edilmelidir:


### <a name="formulate-your-query"></a>Sorgunuzu düzenleme
[REST API kullanarak dizininizi aramanın](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) iki yolu bulunur. Bu yollardan biri, sorgu parametrelerinizin istek gövdesindeki bir JSON nesnesinde tanımlanacağı bir HTTP POST isteği göndermektir. Diğer yol ise sorgu parametrelerinizin istek URL'si içinde tanımlanacağı bir HTTP GET isteği göndermektir. POST, sorgu parametrelerinin boyutu açısından GET'ten daha [esnek sınırlara](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) sahiptir. Bu nedenle, GET'i kullanmanın daha kullanışlı olduğu özel durumlar olmadığı sürece POST kullanmanızı öneririz.

POST ve GET için *hizmet adınızı*, *dizin adını* ve uygun *API sürümünü* (bu belgenin yayımlandığı sırada geçerli API sürümü `2019-05-06`) istek URL'sinde sağlamanız gerekir. GET için sorgu parametrelerini URL'nin sonundaki *sorgu dizesine* sağlarsınız. URL biçimi için aşağıya bakın:

    https://[service name].search.windows.net/indexes/[index name]/docs?[query string]&api-version=2019-05-06

POST için biçim aynıdır ancak sorgu dizesi parametrelerinde yalnızca api sürümü olur.


## <a name="pulling-data-into-an-index"></a>Verileri dizine çekme
Çekme modeli, desteklenen veri kaynağında gezinir ve dizininize verileri otomatik olarak yükler. Azure Bilişsel Arama'da bu özellik, şu anda bu platformlar için kullanılabilen *dizin leyiciler*aracılığıyla uygulanır:

+ [Blob depolama](search-howto-indexing-azure-blob-storage.md)
+ [Tablo depolama](search-howto-indexing-azure-tables.md)
+ [Azure Cosmos DB](https://aka.ms/documentdb-search-indexer)
+ [Azure SQL veritabanı ve Azure VM'lerinde SQL Server](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)

Dizin oluşturucular bir dizini bir veri kaynağına (genelde tablo, görünüm veya eşdeğer bir yapı) bağlar ve kaynak alanları dizindeki eşdeğer alanlara eşler. Yürütme sırasında satır kümesi otomatik olarak JSON'a dönüştürülür ve belirtilen dizine yüklenir. Tüm dizin oluşturucular zamanlamayı destekler ve bu sayede verilerin yenilenme sıklığını belirleyebilirsiniz. Çoğu dizin oluşturucular veri kaynağının desteklemesi durumunda değişiklik izleme özelliği sunar. Dizin oluşturucular, var olan belgelerdeki değişiklikleri ve silmeleri takip etmenin yanı sıra yeni belgeleri tanıyarak, dizininizdeki verileri aktif şekilde yönetme ihtiyacını ortadan kaldırır. 


### <a name="how-to-pull-data-into-an-azure-cognitive-search-index"></a>Azure Bilişsel Arama dizinine veri çekme

Dizin oluşturucu işlevleri [Azure portalı](search-import-data-portal.md), [REST API'sı](/rest/api/searchservice/Indexer-operations) ve [.NET SDK'sında](/dotnet/api/microsoft.azure.search.indexersoperationsextensions) belirtilmiştir. 

Portalı kullanmanın bir avantajı, Azure Bilişsel Arama'nın genellikle kaynak veri kümesinin meta verilerini okuyarak sizin için varsayılan bir dizin şeması oluşturabiliyor olmasıdır. Oluşturulan dizini işlenene kadar değiştirebilirsiniz ancak işlendikten sonra yalnızca dizinin yeniden oluşturulmasını gerektirmeyen şema düzenlemelerine izin verilir. Yapmak istediğiniz değişikliklerin şemayı doğrudan etkilemesi halinde dizini yeniden oluşturmanız gerekir. 

## <a name="verify-data-import-with-search-explorer"></a>Veri alma alma işleminin Arama gezgini ile doğrula

Belge yüklemesi üzerinde ön denetim yapmanın hızlı bir yolu, portaldaki **Arama gezginini** kullanmaktır. Bu gezgin, bir dizini kod yazmadan sorgulamanızı sağlar. Arama deneyimi, [basit sözdizimi](/rest/api/searchservice/simple-query-syntax-in-azure-search) ve varsayılan [aramaModu sorgu parametresi](/rest/api/searchservice/search-documents)gibi varsayılan ayarları temel alır. Belgenin tamamını inceleyebilmeniz için sonuçlar JSON biçiminde döndürülür.

> [!TIP]
> Çok sayıda [Azure Bilişsel Arama kodu örneği,](https://github.com/Azure-Samples/?utf8=%E2%9C%93&query=search) başlamak için kolay bir yol sunan gömülü veya hazır kullanılabilir veri kümelerini içerir. Portalda ayrıca örnek dizin oluşturucu ve küçük bir emlak veri kümesini ("realestate-us-sample" adlı) içeren veri kaynağı mevcuttur. Örnek veri kaynağında önceden yapılandırılmış dizin oluşturucuyu çalıştırdığınızda, bir dizin oluşturulur ve daha sonra Arama gezgininde veya yazdığınız kodla sorgulanabilecek belgelerle yüklenir.

## <a name="see-also"></a>Ayrıca bkz.

+ [Dizin Oluşturucu’ya genel bakış](search-indexer-overview.md)
+ [Portal kılavuzu: dizini oluşturma, yükleme, sorgulama](search-get-started-portal.md)
