---
title: İçeri aktarma sırasında verileri gezinmek için Dizin oluşturucular
titleSuffix: Azure Cognitive Search
description: Aranabilir verileri ayıklamak ve bir Azure Bilişsel Arama dizinini doldurmak için Azure SQL veritabanı, SQL yönetilen örneği, Azure Cosmos DB veya Azure Storage 'ı gezin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/11/2020
ms.custom: fasttrack-edit
ms.openlocfilehash: 0405db2b68abefbfdc424def9e35e363e45043cd
ms.sourcegitcommit: c136985b3733640892fee4d7c557d40665a660af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98180141"
---
# <a name="indexers-in-azure-cognitive-search"></a>Azure Bilişsel Arama'daki Dizin Oluşturucular

Azure Bilişsel Arama 'de bir *Dizin Oluşturucu* , bir dış Azure veri kaynağından aranabilir verileri ve meta verileri çıkaran ve kaynak verilerle dizininizdeki alanları alan eşlemelerini kullanarak bir arama dizinini dolduran bir gezgin. Bu yaklaşım bazen ' çekme modeli ' olarak adlandırılır, çünkü hizmet verileri bir dizine ekleyen herhangi bir kod yazmak zorunda kalmadan içine veri çeker.

Dizin oluşturucular, Azure SQL, Azure Cosmos DB, Azure Tablo depolama ve BLOB depolama için bireysel dizin oluşturucular ile yalnızca Azure ' dir. Bir Dizin Oluşturucu yapılandırdığınızda, bir veri kaynağı (Origin) ve bir dizin (hedef) belirtirsiniz. BLOB depolama Dizin oluşturucular gibi çeşitli veri kaynakları, bu içerik türüne özgü ek özelliklere sahiptir.

Dizin oluşturucularını isteğe bağlı olarak veya her beş dakikada bir çalışan yinelenen bir veri yenileme zamanlaması üzerinde çalıştırabilirsiniz. Daha sık güncellemeler, hem Azure Bilişsel Arama hem de dış veri kaynağınızdaki verileri eşzamanlı olarak güncelleştiren bir anında iletme modeli gerektirir.

## <a name="usage-scenarios"></a>Kullanım senaryoları

Bir dizin oluşturucuyu, veri alımı için tek bir yol olarak kullanabilir veya dizininizdeki alanların yalnızca bazılarını yüklemeyi, isteğe bağlı olarak içeriği dönüştürme veya zenginleştirme gibi tekniklerin birleşimini kullanabilirsiniz. Aşağıdaki tabloda ana senaryolar özetlenmektedir.

| Senaryo |Strateji |
|----------|---------|
| Tek kaynak | Bu model en basit: bir veri kaynağı, bir arama dizini için tek içerik sağlayıcıdır. Kaynaktan, arama dizininde belge anahtarı olarak kullanılacak benzersiz değerler içeren bir alan tanımlayacaksınız. Benzersiz değer bir tanımlayıcı olarak kullanılacaktır. Diğer tüm kaynak alanları, bir dizindeki ilgili alanlarla örtük olarak veya açıkça eşleştirilir. </br></br>Önemli bir kalkış, bir belge anahtarı değerinin kaynak verilerden kaynaklanabilme örneğidir. Arama hizmeti anahtar değerleri oluşturmaz. Sonraki çalışmalarda, yeni anahtarlara sahip gelen belgeler eklenir, ancak mevcut anahtarlara sahip gelen belgeler, Dizin alanlarının null ya da doldurulmuş olmasına bağlı olarak birleştirilir veya üzerine yazılır. |
| Birden çok kaynak| Bir dizin, her çalıştırmanın farklı bir kaynaktan yeni içerik aldığı birden çok kaynaktan içerik kabul edebilir. </br></br>Tek bir sonuç, her bir Dizin Oluşturucu çalıştıktan sonra belge elde eden bir dizin olabilir ve tüm belgeler her kaynaktan tam olarak oluşturulur. Bu senaryonun çekişmesi, tüm gelen veriler için ve arama dizininde bulunan bir belge anahtarı için kullanılan bir dizin şeması tasarlamada yer alır. Örneğin, bir belgeyi benzersiz bir şekilde tanımlayan değerler bir blob kapsayıcısında ve bir SQL tablosundaki birincil anahtar metadata_storage_path, içerik kaynağından bağımsız olarak, bir veya iki kaynağın anahtar değerlerini ortak bir biçimde sağlamak üzere belirlenmesi gerektiğini düşünebilirsiniz. Bu senaryoda, verileri tek bir dizine çekilecek şekilde eklemek için bir miktar ön işleme düzeyi gerçekleştirmeyi beklemeniz gerekir.</br></br>Alternatif bir sonuç, ilk çalıştırmada kısmen doldurulmuş ve ardından sonraki çalışmalarla daha sonra diğer kaynaklardan değer getirmek için daha sonra doldurulmuş belgelerde arama yapmak olabilir. Bu düzenin çekişmesi, her bir dizinleme çalıştırmasının aynı belgeyi hedeflediğinden emin olmanızı sağlamak. Alanların mevcut bir belgede birleştirilmesi için belge anahtarında bir eşleşme olması gerekir. Bu senaryonun bir gösterimi için bkz. [öğretici: birden çok veri kaynağından Dizin](tutorial-multiple-data-sources.md). |
| İçerik dönüştürme | Bilişsel Arama, yeni aranabilir içerik ve yapı oluşturmak için görüntü analizi ve doğal dil işleme ekleyen isteğe bağlı [AI zenginleştirme](cognitive-search-concept-intro.md) davranışlarını destekler. AI zenginleştirme bir dizin oluşturucuya iliştirilmiş bir [beceri](cognitive-search-working-with-skillsets.md)tarafından tanımlanır. AI zenginleştirme işlemini gerçekleştirmek için, dizin oluşturucunun yine bir dizin ve veri kaynağı olması gerekir, ancak bu senaryoda Dizin Oluşturucu yürütmeye beceri işlem ekler. |

## <a name="approaches-for-creating-and-managing-indexers"></a>Dizin oluşturucular oluşturma ve yönetme yaklaşımları

Dizin oluşturucuları aşağıdaki yaklaşımlarla oluşturabilir ve yönetebilirsiniz:

+ [Portal > verileri Içeri aktarma Sihirbazı](search-import-data-portal.md)
+ [Hizmet REST API'si](/rest/api/searchservice/Indexer-operations)
+ [.NET SDK](/dotnet/api/azure.search.documents.indexes.models.searchindexer)

SDK kullanıyorsanız, Dizin oluşturucular, veri kaynakları ve becerileri ile çalışmak için bir [SearchIndexerClient](/dotnet/api/azure.search.documents.indexes.searchindexerclient) oluşturun. Yukarıdaki bağlantı .NET SDK içindir, ancak tüm SDK 'Lar SearchIndexerClient ve benzer API 'Ler sağlar.

Başlangıçta yeni veri kaynakları Önizleme özellikleri olarak duyurulur ve yalnızca REST ' dir. Genel kullanılabilirliğe mezun olduktan sonra, tam destek portalda ve her biri kendi yayın zamanlamalarında olan çeşitli SDK 'larda yerleşiktir.

## <a name="permissions"></a>İzinler

Durum ve tanımlar için istekleri al da dahil olmak üzere dizin oluşturucularla ilgili tüm işlemler, [yönetici API anahtarı](search-security-api-keys.md)gerektirir.

<a name="supported-data-sources"></a>

## <a name="supported-data-sources"></a>Desteklenen veri kaynakları

Dizin oluşturucular Azure 'da veri depolarında gezinme.

+ [Azure Blob Depolama](search-howto-indexing-azure-blob-storage.md)
+ [Azure Data Lake Storage 2.](search-howto-index-azure-data-lake-storage.md) (önizlemede)
+ [Azure Table Storage](search-howto-indexing-azure-tables.md)
+ [Azure Cosmos DB](search-howto-index-cosmosdb.md)
+ [Azure SQL Veritabanı](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
+ [SQL Yönetilen Örnek](search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers.md)
+ [Azure Sanal Makineler’de SQL Server](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md)

## <a name="stages-of-indexing"></a>Dizin oluşturma aşamaları

Bir başlangıç çalıştırmasında, dizin boş olduğunda bir Dizin Oluşturucu, tabloda veya kapsayıcıda belirtilen tüm verilerde okunacaktır. Sonraki çalışmalarda, Dizin Oluşturucu genellikle değişmiş olan verileri algılayabilir ve alabilir. Blob verileri için değişiklik algılama otomatik olarak belirlenir. Azure SQL veya Cosmos DB gibi diğer veri kaynakları için değişiklik algılama özelliğinin etkinleştirilmesi gerekir.

Bir Dizin Oluşturucu, belge almadan dizin oluşturma için "iletim" nihai arama motoruna birden çok adımı uygular veya düzenler. İsteğe bağlı olarak, bir Dizin Oluşturucu, bir beceri tanımlandığından beceri yürütme ve çıkışları çalıştırmaya da benzer.

:::image type="content" source="media/search-indexer-overview/indexer-stages.png" alt-text="Dizin Oluşturucu aşamaları" border="false":::

### <a name="stage-1-document-cracking"></a>1. Aşama: belge çözme

Belge çözme, dosyaları açma ve içeriği ayıklama işlemidir. Veri kaynağının türüne bağlı olarak, Dizin Oluşturucu, Dizin oluşturamayacak olabilecek içeriği ayıklamak için farklı işlemler gerçekleştirmeye çalışır.  

Örnekler:  

+ Belge, [Azure SQL veri kaynağındaki](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)bir kayıt olduğunda, Dizin Oluşturucu kayıt için her bir alanı ayıklar.
+ Belge, [Azure Blob depolama veri kaynağındaki](search-howto-indexing-azure-blob-storage.md)bir PDF dosyası olduğunda, Dizin Oluşturucu metin, resim ve meta verileri ayıklar.
+ Belge, [Cosmos DB veri kaynağındaki](search-howto-index-cosmosdb.md)bir kayıt olduğunda, Dizin Oluşturucu alanları ve alt alanları Cosmos DB belgesinden ayıklar.

### <a name="stage-2-field-mappings"></a>2. Aşama: alan eşlemeleri 

Dizin Oluşturucu bir kaynak alanından metin ayıklar ve bir dizin veya bilgi deposundaki hedef alana gönderir. Alan adları ve türleri kesişyolduğunda yol net olur. Ancak, çıktıda farklı adlar veya türler isteyebilirsiniz. Bu durumda, dizin oluşturucuya alanı nasıl eşleyeceğinizi bildirmeniz gerekir. Bu adım, belge çözme işleminden sonra, ancak Dizin Oluşturucu kaynak belgelerden okurken dönüşümlerden önce oluşur. Bir [alan eşlemesi](search-indexer-field-mappings.md)tanımladığınızda, kaynak alanın değeri, hedef alana değişiklik olmadan olarak gönderilir. Alan eşlemeleri isteğe bağlıdır.

### <a name="stage-3-skillset-execution"></a>3. Aşama: beceri yürütme

Beceri yürütme, yerleşik veya özel AI işlemesini çağıran isteğe bağlı bir adımdır. Görüntü analizi formunda optik karakter tanıma (OCR) için ihtiyacınız olabilir veya dil çevirisine ihtiyacınız olabilir. Dönüştürme ne olursa olsun, Beceri yürütmesi nerede meydana gelir. Bir Dizin Oluşturucu bir işlem hattıyla, bir [beceri](cognitive-search-defining-skillset.md) "ardışık düzen içinde işlem hattı" olarak düşünebilirsiniz. Bir beceri, yetenekler adlı kendi adım dizisine sahiptir.

### <a name="stage-4-output-field-mappings"></a>4. Aşama: çıkış alanı eşlemeleri

Bir beceri çıkışı aslında zenginleştirilmiş belge olarak adlandırılan bir bilgi ağacıdır. Çıkış alanı eşlemeleri, bu ağacın hangi bölümlerinin dizininizdeki alanlarla eşlendiğini seçmenizi sağlar. [Çıkış alanı eşlemelerini nasıl tanımlayacağınızı](cognitive-search-output-field-mapping.md)öğrenin.

Tam değerleri kaynaktan hedef alanlarla ilişkilendiren alan eşlemeleri gibi, çıkış alanı eşlemeleri, dizin oluşturucunun, zenginleştirilmiş belgedeki dönüştürülmüş değerleri dizindeki hedef alanlarla nasıl ilişkilendirileceğini söyler. İsteğe bağlı olarak kabul edilen alan eşlemelerinin aksine, her zaman bir dizinde bulunması gereken dönüştürülmüş içerikler için bir çıkış alanı eşlemesi tanımlamanız gerekir.

Sonraki görüntüde, Dizin Oluşturucu aşamaları için bir örnek Dizin Oluşturucu [hata ayıklama oturumu](cognitive-search-debug-session.md) temsili gösterilmektedir: belge çözme, alan eşlemeleri, Beceri yürütme ve çıkış alanı eşlemeleri.

:::image type="content" source="media/search-indexer-overview/sample-debug-session.png" alt-text="örnek hata ayıklama oturumu" lightbox="media/search-indexer-overview/sample-debug-session.png":::

## <a name="basic-configuration-steps"></a>Temel yapılandırma adımları

Dizin oluşturucular veri kaynağına özgü özellikler sunabilir. Bu bakımdan, dizin oluşturucu veya veri kaynağı yapılandırmasının bazı boyutları dizin oluşturucu türüne göre farklılık gösterir. Bununla birlikte, tüm dizin oluşturucuların temel birleşimi ve gereksinimleri aynıdır. Tüm dizin oluşturucularda ortak olan adımlar aşağıda ele alınmıştır.

### <a name="step-1-create-a-data-source"></a>1. Adım: Veri kaynağı oluşturma

Bir dizin *Oluşturucu veri kaynağı nesnesinden veri* kaynağı bağlantısı edinir. Veri kaynağı tanımı, bir bağlantı dizesi ve muhtemelen kimlik bilgileri sağlar. Kaynağı oluşturmak için [Create Datasource](/rest/api/searchservice/create-data-source) REST API veya [SearchIndexerDataSourceConnection sınıfını](/dotnet/api/azure.search.documents.indexes.models.searchindexerdatasourceconnection) çağırın.

Veri kaynakları, bunları kullanan dizin oluşturuculardan bağımsız olarak yapılandırılır ve yönetilir. Bu da bir veri kaynağının, bir seferde birden çok dizin yüklemek amacıyla birden çok dizin oluşturucu tarafından kullanılabileceği anlamına gelir.

### <a name="step-2-create-an-index"></a>2. Adım: Dizin oluşturma

Dizin oluşturucu veri alımıyla ilgili bazı görevleri otomatikleştirir, ancak dizin oluşturma genellikle bu görevlerden biri değildir. Bir önkoşul olarak dış veri kaynağınızdaki alanlarla eşleşen alanlara sahip önceden tanımlı bir dizininiz olmalıdır. Alanların ad ve veri türüyle eşleşmesi gerekir. Dizini yapılandırma hakkında daha fazla bilgi için bkz. [Dizin oluşturma (Azure Bilişsel Arama REST API)](/rest/api/searchservice/Create-Index) veya [searchındex sınıfı](/dotnet/api/azure.search.documents.indexes.models.searchindex). Alan ilişkilendirmelerinde yardım için bkz. [Azure bilişsel arama Dizin oluşturucularda alan eşlemeleri](search-indexer-field-mappings.md).

> [!Tip]
> Dizin oluşturucular sizin için dizin oluşturamasa da, portaldaki **Verileri içeri aktarma** sihirbazı bu işlem için size yardımcı olabilir. Çoğu durumda, sihirbaz, kaynaktaki mevcut meta verilerden dizin şeması çıkarsayarak, sihirbaz etkin olduğunda satır içinde düzenleyebileceğiniz geçici bir dizin şeması sunar. Hizmet için sihirbaz oluşturulduğunda, portalda yapılabilecek ayrıntılı düzenlemeler, genellikle yeni alanlar eklemeyle sınırlıdır. Sihirbaz dizin oluşturmak için uygun olsa da, düzenlemek için uygun değildir. Uygulama yaparak öğrenmek için, [portal kılavuzundaki](search-get-started-portal.md) adımları izleyin.

### <a name="step-3-create-and-schedule-the-indexer"></a>3. Adım: Dizin oluşturucuyu oluşturma ve zamanlama

Dizin Oluşturucu tanımı, veri alımı ile ilgili tüm öğeleri birlikte getiren bir yapıdır. Gerekli öğeler bir veri kaynağı ve dizin içerir. İsteğe bağlı öğeler bir zamanlama ve alan eşlemeleri içerir. Alan eşlemeleri yalnızca kaynak alanları ve dizin alanları açıkça karşılık geliyorsa isteğe bağlıdır. Dizin oluşturucuyu yapılandırma hakkında daha fazla bilgi için bkz. [Dizin Oluşturucu oluşturma (Azure Bilişsel Arama REST API)](/rest/api/searchservice/Create-Indexer).

<a id="RunIndexer"></a>

## <a name="run-indexers-on-demand"></a>Dizin oluşturucuyu isteğe bağlı olarak çalıştır

Dizin oluşturma zamanlaması yaygın olsa da, [Çalıştır komutu](/rest/api/searchservice/run-indexer)kullanılarak bir Dizin Oluşturucu da isteğe bağlı olarak çağrılabilir:

```http
POST https://[service name].search.windows.net/indexers/[indexer name]/run?api-version=2020-06-30
api-key: [Search service admin key]
```

> [!NOTE]
> Run API bir başarı kodu döndürdüğünde, Dizin Oluşturucu çağrısı zamanlandı, ancak gerçek işlem zaman uyumsuz olur. 

Dizin Oluşturucu durumunu portalda veya [Dizin Oluşturucu durumunu Al API 'si](/rest/api/searchservice/get-indexer-status)aracılığıyla izleyebilirsiniz. 

<a name="GetIndexerStatus"></a>

## <a name="get-indexer-status"></a>Dizin Oluşturucu durumunu al

Bir dizin oluşturucunun durum ve yürütme geçmişini [Get Indexer durum komutuyla](/rest/api/searchservice/get-indexer-status)alabilirsiniz:

```http
GET https://[service name].search.windows.net/indexers/[indexer name]/status?api-version=2020-06-30
api-key: [Search service admin key]
```

Yanıt genel Dizin Oluşturucu durumunu, son (veya sürmekte olan) Dizin Oluşturucu çağrısını ve son Dizin Oluşturucu etkinleştirmeleri geçmişini içerir.

```output
{
    "status":"running",
    "lastResult": {
        "status":"success",
        "errorMessage":null,
        "startTime":"2018-11-26T03:37:18.853Z",
        "endTime":"2018-11-26T03:37:19.012Z",
        "errors":[],
        "itemsProcessed":11,
        "itemsFailed":0,
        "initialTrackingState":null,
        "finalTrackingState":null
     },
    "executionHistory":[ {
        "status":"success",
         "errorMessage":null,
        "startTime":"2018-11-26T03:37:18.853Z",
        "endTime":"2018-11-26T03:37:19.012Z",
        "errors":[],
        "itemsProcessed":11,
        "itemsFailed":0,
        "initialTrackingState":null,
        "finalTrackingState":null
    }]
}
```

Yürütme geçmişi, geriye doğru kronolojik düzende sıralanan en son tamamlanan yürütme sayısı olan 50 ' i içerir (Bu nedenle, en son yürütme yanıtta ilk kez gelir).

## <a name="next-steps"></a>Sonraki adımlar

Artık temel fikri anladığınıza göre, atmanız gereken bir sonraki adım her bir veri kaynağı türüne özgü gereksinimleri ve görevleri incelemektir.

+ [Azure SQL veritabanı, SQL yönetilen örneği veya bir Azure sanal makinesinde SQL Server](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
+ [Azure Cosmos DB](search-howto-index-cosmosdb.md)
+ [Azure Blob Depolama](search-howto-indexing-azure-blob-storage.md)
+ [Azure Table Storage](search-howto-indexing-azure-tables.md)
+ [Azure Bilişsel Arama blob Dizin oluşturucuyu kullanarak CSV bloblarını dizine ekleme](search-howto-index-csv-blobs.md)
+ [JSON bloblarını Azure Bilişsel Arama blob Indexer ile dizinleme](search-howto-index-json-blobs.md)