---
title: Azure SQL verileri üzerinden arama yapın
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama'da tam metin arama sı için dizin leyicileri kullanarak Azure SQL Veritabanından veri aktarın. Bu makalede bağlantılar, dizin oluşturma yapılandırması ve veri alımı ele ait.
manager: nitinme
author: mgottein
ms.author: magottei
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: c09727e8d92a449b41124eae6ad8381d66cb2619
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74113313"
---
# <a name="connect-to-and-index-azure-sql-database-content-using-an-azure-cognitive-search-indexer"></a>Azure Bilişsel Arama dizinleyicisini kullanarak Azure SQL Veritabanı içeriğine bağlanma ve dizine dizin oluşturma

[Bir Azure Bilişsel Arama dizinini](search-what-is-an-index.md)sorgulayamadan önce, verilerinizle doldurmanız gerekir. Veriler bir Azure SQL veritabanında yaşıyorsa, Azure SQL Veritabanı için bir **Azure Bilişsel Arama dizinleyicisi** (veya kısaca **Azure SQL dizinleyicisi)** dizin oluşturma işlemini otomatikleştirebilir, bu da yazmak için daha az kod ve daha az dikkat edilebilen altyapı anlamına gelir.

Bu makale, [dizin oluşturma](search-indexer-overview.md)mekaniğini kapsar, ancak yalnızca Azure SQL veritabanlarında kullanılabilen özellikleri (örneğin, tümleşik değişiklik izleme) açıklar. 

Azure SQL veritabanlarına ek olarak, Azure Bilişsel Arama, [Azure Cosmos DB,](search-howto-index-cosmosdb.md) [Azure Blob depolama](search-howto-indexing-azure-blob-storage.md)ve Azure tablo [depolama](search-howto-indexing-azure-tables.md)için dizin oluşturma bilgileri sağlar. Diğer veri kaynakları için destek istemek için Azure [Bilişsel Arama geri bildirimi forumu](https://feedback.azure.com/forums/263029-azure-search/)hakkında görüşlerinizi bildirin.

## <a name="indexers-and-data-sources"></a>Dizinleyiciler ve veri kaynakları

Bir **veri kaynağı,** hangi verilerin dizinlendirilecek, veri erişimi için kimlik bilgilerini ve verilerdeki değişiklikleri (yeni, değiştirilmiş veya silinmiş satırlar) verimli bir şekilde tanımlayan ilkeleri belirtir. Birden çok dizinleyici tarafından kullanılabilen bağımsız bir kaynak olarak tanımlanır.

**Dizinleyici,** hedeflenen bir arama dizinine tek bir veri kaynağını bağlayan bir kaynaktır. Bir dizinleyici aşağıdaki şekillerde kullanılır:

* Dizin doldurmak için verilerin bir defalık kopyasını gerçekleştirin.
* Bir zamanlamada veri kaynağındaki değişikliklerle bir dizini güncelleştirin.
* Bir dizini gerektiği gibi güncelleştirmek için isteğe bağlı çalıştırın.

Tek bir dizin oluşturucu yalnızca bir tablo veya görünüm tüketebilir, ancak birden çok arama dizinleri doldurmak istiyorsanız birden çok dizin oluşturabilirsiniz. Kavramlar hakkında daha fazla bilgi için [bkz: Dizin Oluşturma İşlemi: Tipik iş akışı.](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations#typical-workflow)

Şunları kullanarak bir Azure SQL dizinleyicisi ayarlayabilir ve yapılandırabilirsiniz:

* [Azure portalında](https://portal.azure.com) Veri Alma sihirbazı
* Azure Bilişsel Arama [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer?view=azure-dotnet)
* Azure Bilişsel Arama [REST API](https://docs.microsoft.com/rest/api/searchservice/indexer-operations)

Bu makalede, **dizin oluşturup** **veri kaynakları**oluşturmak için REST API'sını kullanacağız.

## <a name="when-to-use-azure-sql-indexer"></a>Azure SQL Indexer ne zaman kullanılır?
Verilerinizle ilgili çeşitli etkenlere bağlı olarak, Azure SQL dizinleyicisinin kullanımı uygun olabilir veya olmayabilir. Verileriniz aşağıdaki gereksinimlere uyuyorsa, Azure SQL dizinleyicisini kullanabilirsiniz.

| Ölçütler | Ayrıntılar |
|----------|---------|
| Veriler tek bir tablo veya görünümden kaynaklanır | Veriler birden çok tabloya dağılmışsa, verilerin tek bir görünümünü oluşturabilirsiniz. Ancak, bir görünüm kullanırsanız, bir dizini artımlı değişikliklerle yenilemek için SQL Server tümleşik değişiklik algılamasını kullanamazsınız. Daha fazla bilgi için aşağıdaki [Değiştirilen ve Silinen Satırları Yakalama'ya](#CaptureChangedRows) bakın. |
| Veri türleri uyumludur | Tüm SQL türleri Azure Bilişsel Arama dizininde desteklenir, ancak bunlar desteklenmez. Liste için [bkz.](#TypeMapping) |
| Gerçek zamanlı veri eşitlemesi gerekli değildir | Bir dizin leyici tablonuzu en fazla her beş dakikada bir yeniden dizine ekleyebilir. Verileriniz sık sık değişirse ve değişikliklerin saniye veya tek dakika içinde dizine yansıtılması gerekiyorsa, güncelleştirilmiş satırları doğrudan itmek için [REST API](https://docs.microsoft.com/rest/api/searchservice/AddUpdate-or-Delete-Documents) veya [.NET SDK'yı](search-import-data-dotnet.md) kullanmanızı öneririz. |
| Artımlı dizin oluşturma mümkündür | Büyük bir veri kümeniz varsa ve dizin leyiciyi bir zamanlamada çalıştırmayı planlıyorsanız, Azure Bilişsel Arama'nın yeni, değiştirilen veya silinmiş satırları verimli bir şekilde tanımlayabilmesi gerekir. Artımlı olmayan dizin oluşturma yalnızca isteğe bağlı dizine (zamanında değil) veya 100.000 satırdan az dizine izin verir. Daha fazla bilgi için aşağıdaki [Değiştirilen ve Silinen Satırları Yakalama'ya](#CaptureChangedRows) bakın. |

> [!NOTE] 
> Azure Bilişsel Arama yalnızca SQL Server kimlik doğrulamasını destekler. Azure Active Directory Password kimlik doğrulaması için destek istiyorsanız, lütfen bu [UserVoice önerisine](https://feedback.azure.com/forums/263029-azure-search/suggestions/33595465-support-azure-active-directory-password-authentica)oy verin.

## <a name="create-an-azure-sql-indexer"></a>Azure SQL Dizin Oluştur

1. Veri kaynağını oluşturun:

   ```
    POST https://myservice.search.windows.net/datasources?api-version=2019-05-06
    Content-Type: application/json
    api-key: admin-key

    {
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:<your server>.database.windows.net,1433;Database=<your database>;User ID=<your user name>;Password=<your password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "name of the table or view that you want to index" }
    }
   ```

   Bağlantı dizesini Azure [portalından](https://portal.azure.com)alabilirsiniz; `ADO.NET connection string` seçeneğini kullanın.

2. Zaten bir hedefiniz yoksa hedef Azure Bilişsel Arama dizinini oluşturun. [Portalı](https://portal.azure.com) veya [Create Index API'yi](https://docs.microsoft.com/rest/api/searchservice/Create-Index)kullanarak bir dizin oluşturabilirsiniz. Hedef dizininizin şemasının kaynak tablonun şemasıyla uyumlu olduğundan emin olun - [SQL ve Azure Bilişsel arama veri türleri arasındaki eşleme](#TypeMapping)bölümüne bakın.

3. Bir ad vererek ve veri kaynağı ve hedef dizini başvurarak dizin oluştur:

    ```
    POST https://myservice.search.windows.net/indexers?api-version=2019-05-06
    Content-Type: application/json
    api-key: admin-key

    {
        "name" : "myindexer",
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name"
    }
    ```

Bu şekilde oluşturulan bir dizin oluşturucunun bir zamanlaması yoktur. Oluşturulduğunda otomatik olarak bir kez çalışır. **Çalıştır dizinleyici** isteğini kullanarak istediğiniz zaman yeniden çalıştırabilirsiniz:

    POST https://myservice.search.windows.net/indexers/myindexer/run?api-version=2019-05-06
    api-key: admin-key

Işiçin oluşturma boyutu ve dizin leyici yürütme başarısız olmadan önce kaç belge atlanabileceği gibi dizinleyici davranışının çeşitli yönlerini özelleştirebilirsiniz. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/rest/api/searchservice/Create-Indexer)

Azure hizmetlerinin veritabanınıza bağlanmasına izin verebilirsiniz. Bunun nasıl yapılacağını anlatan yönergeler için [Azure'dan Bağlanma'ya](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) bakın.

Dizinleyici durumunu ve yürütme geçmişini (dizine bindirilen öğe sayısı, hatalar vb.) izlemek için **dizinleyici durum** isteğini kullanın:

    GET https://myservice.search.windows.net/indexers/myindexer/status?api-version=2019-05-06
    api-key: admin-key

Yanıt aşağıdakilere benzer olmalıdır:

    {
        "\@odata.context":"https://myservice.search.windows.net/$metadata#Microsoft.Azure.Search.V2015_02_28.IndexerExecutionInfo",
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2015-02-21T00:23:24.957Z",
            "endTime":"2015-02-21T00:36:47.752Z",
            "errors":[],
            "itemsProcessed":1599501,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        },
        "executionHistory":
        [
            {
                "status":"success",
                "errorMessage":null,
                "startTime":"2015-02-21T00:23:24.957Z",
                "endTime":"2015-02-21T00:36:47.752Z",
                "errors":[],
                "itemsProcessed":1599501,
                "itemsFailed":0,
                "initialTrackingState":null,
                "finalTrackingState":null
            },
            ... earlier history items
        ]
    }

Yürütme geçmişi, ters kronolojik sırada sıralanmış en son tamamlanan yürütmelerden en fazla 50'sini içerir (böylece en son yürütme yanıtta önce gelir).
Yanıt hakkında ek bilgiler [Al Dizinleyici Durumu'nda](https://go.microsoft.com/fwlink/p/?LinkId=528198) bulunabilir

## <a name="run-indexers-on-a-schedule"></a>Dizin işaretlerini zamanlamada çalıştırma
Dizinleyiciyi, programda düzenli olarak çalışacak şekilde de ayarlayabilirsiniz. Bunu yapmak için, dizin oluşturup güncellerken **zamanlama** özelliğini ekleyin. Aşağıdaki örnek, dizinleyiciyi güncelleştirmek için bir PUT isteği gösterir:

    PUT https://myservice.search.windows.net/indexers/myindexer?api-version=2019-05-06
    Content-Type: application/json
    api-key: admin-key

    {
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name",
        "schedule" : { "interval" : "PT10M", "startTime" : "2015-01-01T00:00:00Z" }
    }

**Aralık** parametresi gereklidir. Aralık, iki ardışık dizinleyici yürütmenin başlangıcı arasındaki süreyi ifade eder. İzin verilen en küçük aralık 5 dakikadır; en uzunu bir gündür. XSD "dayTimeDuration" değeri [(ISO 8601 süre](https://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) değerinin sınırlı bir alt kümesi) olarak biçimlendirilmelidir. Bunun için desen: `P(nD)(T(nH)(nM))`. Örnekler: `PT15M` her 15 `PT2H` dakikada bir, her 2 saat için.

Dizin oluşturma zamanlamaları hakkında daha fazla bilgi için Azure Bilişsel Arama için dizin oluşturma yı [nasıl zamanlayınız'](search-howto-schedule-indexers.md)a bakın.

<a name="CaptureChangedRows"></a>

## <a name="capture-new-changed-and-deleted-rows"></a>Yeni, değiştirilmiş ve silinmiş satırları yakalama

Azure Bilişsel Arama, tüm tabloyu yeniden dizine ekolarak dizine ekolarak veya bir dizin leyici çalıştırınher görüntülemek zorunda kalmamak için **artımlı dizin oluşturma** kullanır. Azure Bilişsel Arama, artımlı dizin oluşturmayı desteklemek için iki değişiklik algılama ilkeleri sağlar. 

### <a name="sql-integrated-change-tracking-policy"></a>SQL Entegre Değişim İzleme İlkesi
SQL veritabanınız [değişiklik izlemeyi](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server)destekliyorsa, **SQL Tümleşik Değişim İzleme İlkesi'ni**kullanmanızı öneririz. Bu en verimli ilkedir. Ayrıca, Azure Bilişsel Arama'nın tablonuza açık bir "yumuşak silme" sütunu eklemenize gerek kalmadan silinen satırları tanımlamasına olanak tanır.

#### <a name="requirements"></a>Gereksinimler 

+ Veritabanı sürüm gereksinimleri:
  * SQL Server 2012 SP3 ve sonrası, Sql Server'ı Azure VM'lerde kullanıyorsanız.
  * Azure SQL Veritabanı kullanıyorsanız Azure SQL Veritabanı V12.
+ Yalnızca tablolar (görünüm yok). 
+ Veritabanında, tablo için [değişiklik izlemeyi etkinleştirin.](https://docs.microsoft.com/sql/relational-databases/track-changes/enable-and-disable-change-tracking-sql-server) 
+ Tabloda bileşik birincil anahtar (birden fazla sütun içeren birincil anahtar) yok.  

#### <a name="usage"></a>Kullanım

Bu ilkeyi kullanmak için veri kaynağınızı şu şekilde oluşturun veya güncelleştirin:

    {
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "connection string" },
        "container" : { "name" : "table or view name" },
        "dataChangeDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.SqlIntegratedChangeTrackingPolicy"
      }
    }

SQL tümleşik değişiklik izleme ilkesini kullanırken, ayrı bir veri silme algılama ilkesi belirtmeyin - bu ilke silinen satırları tanımlamak için yerleşik desteke sahiptir. Ancak, silmelerin "otomatik olarak" algılanabilmesi için, arama dizininizdeki belge anahtarının SQL tablosundaki birincil anahtarla aynı olması gerekir. 

> [!NOTE]  
> Bir SQL tablosundan çok sayıda satır kaldırmak için [TRUNCATE TABLE](https://docs.microsoft.com/sql/t-sql/statements/truncate-table-transact-sql) kullanırken, satır silme almak için değişiklik izleme durumunu sıfırlamak için dizinleyicinin [sıfırlanması](https://docs.microsoft.com/rest/api/searchservice/reset-indexer) gerekir.

<a name="HighWaterMarkPolicy"></a>

### <a name="high-water-mark-change-detection-policy"></a>Yüksek Su İşareti Değişimi Algılama ilkesi

Bu değişiklik algılama ilkesi, bir satırın en son güncelleştirildiğinde sürümü veya zamanı yakalayan "yüksek su işareti" sütununa dayanır. Bir görünüm kullanıyorsanız, yüksek su işareti ilkesi kullanmanız gerekir. Yüksek su işareti sütunu aşağıdaki gereksinimleri karşılamalıdır.

#### <a name="requirements"></a>Gereksinimler 

* Tüm ekler sütun için bir değer belirtir.
* Bir öğedeki tüm güncelleştirmeler sütunun değerini de değiştirir.
* Bu sütunun değeri her ekleme veya güncelleştirme yle artar.
* Aşağıdaki WHERE ve ORDER BY yan tümcelerini içeren sorgular verimli bir şekilde yürütülebilir:`WHERE [High Water Mark Column] > [Current High Water Mark Value] ORDER BY [High Water Mark Column]`

> [!IMPORTANT] 
> Yüksek su işareti sütunu için [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) veri türünü kullanmanızı şiddetle öneririz. Başka bir veri türü kullanılırsa, bir dizinleyici sorgusuyla aynı anda yürüten hareketlerin varlığındaki tüm değişiklikleri yakalamak için değişiklik izleme garantisi vermez. Salt okunur yinelemeleri olan bir yapılandırmada **satır çevirme** kullanırken, dizinleyiciyi birincil yinelemeye doğrultmalısınız. Veri eşitleme senaryoları için yalnızca birincil yineleme kullanılabilir.

#### <a name="usage"></a>Kullanım

Yüksek su işareti ilkesini kullanmak için veri kaynağınızı şu şekilde oluşturun veya güncelleştirin:

    {
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "connection string" },
        "container" : { "name" : "table or view name" },
        "dataChangeDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
           "highWaterMarkColumnName" : "[a rowversion or last_updated column name]"
      }
    }

> [!WARNING]
> Kaynak tablonun yüksek su işareti sütununda bir dizin yoksa, SQL dizinleyici tarafından kullanılan sorgular zaman dışarı çıkabilir. Özellikle, `ORDER BY [High Water Mark Column]` yan tümce, tablo çok satır içerdiğinde verimli çalışması için bir dizin gerektirir.
>
>

Zaman ayarı hatalarıyla karşılaşırsanız, `queryTimeout` sorgu zaman ayarı varsayılan 5 dakikalık zaman ayarından daha yüksek bir değere ayarlamak için dizin oluşturma ayarını kullanabilirsiniz. Örneğin, zaman aşmasını 10 dakikaolarak ayarlamak için dizin oluşturveya aşağıdaki yapılandırmayla güncelleştirin:

    {
      ... other indexer definition properties
     "parameters" : {
            "configuration" : { "queryTimeout" : "00:10:00" } }
    }

Yan tümceyi `ORDER BY [High Water Mark Column]` de devre dışı kullanabilirsiniz. Ancak, dizinleyici yürütme bir hata tarafından kesilirse, dizinleyici daha sonra çalışırsa tüm satırları yeniden işlemek zorunda olduğu için bu önerilmez - dizinleyici zaten kesildi zaman hemen hemen tüm satırları işlenmiş olsa bile. Yan tümceyi `ORDER BY` devre dışı `disableOrderByHighWaterMarkColumn` kalmak için dizinleyici tanımındaki ayarı kullanın:  

    {
     ... other indexer definition properties
     "parameters" : {
            "configuration" : { "disableOrderByHighWaterMarkColumn" : true } }
    }

### <a name="soft-delete-column-deletion-detection-policy"></a>Yumuşak Silme Sütun Silme Algılama ilkesi
Satırlar kaynak tablodan silindiğinde, büyük olasılıkla bu satırları arama dizininden de silmek istersiniz. SQL tümleşik değişim izleme ilkesini kullanırsanız, bu sizin için halledilir. Ancak, yüksek su işareti değişikliği izleme ilkesi silinen satırlar size yardımcı olmaz. Ne yapmalı?

Satırlar fiziksel olarak tablodan kaldırılırsa, Azure Bilişsel Arama'nın artık var olmayan kayıtların varlığını ortaya çıkarmanın bir yolu yoktur.  Ancak, satırları tablodan çıkarmadan mantıksal olarak silmek için "yumuşak silme" tekniğini kullanabilirsiniz. Tablonuza bir sütun ekleyin veya görüntüleyin ve satırları bu sütunu kullanarak silinmiş olarak işaretleyin.

Yumuşak silme tekniğini kullanırken, veri kaynağını oluştururken veya güncellerken yumuşak silme ilkesini aşağıdaki gibi belirtebilirsiniz:

    {
        …,
        "dataDeletionDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
           "softDeleteColumnName" : "[a column name]",
           "softDeleteMarkerValue" : "[the value that indicates that a row is deleted]"
        }
    }

**softDeleteMarkerValue** bir dize olmalıdır – gerçek değerinizin dize temsilini kullanın. Örneğin, silinen satırların 1 değeriyle işaretlendiği bir bir sonda `"1"`sütununuz varsa, bunu kullanın. Silinen satırların Boolean gerçek değeriyle işaretlendiği bir BIT sütununuz `True` `true`varsa, dizeyi literal veya kullanım örneğini kullanın.

<a name="TypeMapping"></a>

## <a name="mapping-between-sql-and-azure-cognitive-search-data-types"></a>SQL ve Azure Bilişsel Arama veri türleri arasında eşleme
| SQL veri türü | İzin verilen hedef dizin alanı türleri | Notlar |
| --- | --- | --- |
| bit |Edm.Boolean, Edm.String | |
| int, küçük, minicik |Edm.Int32, Edm.Int64, Edm.String | |
| bigint |Edm.Int64, Edm.String | |
| gerçek, float |Edm.Double, Edm.String | |
| küçük para, para ondalık sayısal |Edm.String |Azure Bilişsel Arama, ondalık türlerin Edm.Double'a dönüştürülmesini desteklemez, çünkü bu kesinlik kaybeder |
| char, nchar, varchar, nvarchar |Edm.String<br/>Collection(Edm.String) |Bir SQL dizesi, string bir JSON dize dizisini temsil ediyorsa, Bir Koleksiyon(Edm.String) alanını doldurmak için kullanılabilir:`["red", "white", "blue"]` |
| smalldatetime, datetime, datetime2, tarih, datetimeoffset |Edm.DateTimeOffset, Edm.String | |
| uniqueidentifer |Edm.String | |
| Coğrafya |Edm.GeographyPoint |Yalnızca SRID 4326 (varsayılan olan) ile POINT türündeki coğrafya örnekleri desteklenir |
| Rowversion |Yok |Satır sürüm sütunları arama dizininde depolanamaz, ancak değişiklik izleme için kullanılabilir |
| zaman, zaman dilimi, ikili, varbinary, görüntü, xml, geometri, CLR türleri |Yok |Desteklenmiyor |

## <a name="configuration-settings"></a>Yapılandırma Ayarları
SQL dizinleyici sayar:

| Ayar | Veri türü | Amaç | Varsayılan değer |
| --- | --- | --- | --- |
| Querytimeout |string |SQL sorgu yürütmesi için zaman arasını ayarlar |5 dakika ("00:05:00") |
| devre dışı, OrderByHighWaterMarkColumn |bool |ORDER BY yan tümcesini atlamak için yüksek su işareti ilkesi tarafından kullanılan SQL sorgusuna neden olur. [Yüksek Su İşareti politikasına](#HighWaterMarkPolicy) bakın |yanlış |

Bu ayarlar dizinleyici tanımındaki `parameters.configuration` nesnede kullanılır. Örneğin, sorgu zaman aşmasını 10 dakikaolarak ayarlamak için dizin oluşturveya aşağıdaki yapılandırmayla dizin oluşturun veya güncelleştirin:

    {
      ... other indexer definition properties
     "parameters" : {
            "configuration" : { "queryTimeout" : "00:10:00" } }
    }

## <a name="faq"></a>SSS

**S: Azure'da IaaS VM'lerde çalışan SQL veritabanlarıyla Azure SQL dizinleyicisini kullanabilir miyim?**

Evet. Ancak, arama hizmetinizin veritabanınıza bağlanmasına izin vermeniz gerekir. Daha fazla bilgi için bkz. Azure [Bilişsel Arama dizinleyicisinden SQL Server'a bir Azure VM'de bağlantı yapılandırma](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md).

**S: Şirket içinde çalışan SQL veritabanları ile Azure SQL dizinleyici kullanabilir miyim?**

Doğrudan değil. Veritabanlarınızı Internet trafiğine açmanızı gerektirdiği için doğrudan bir bağlantı önermiyoruz veya destekliyoruz. Müşteriler, Azure Veri Fabrikası gibi köprü teknolojilerini kullanarak bu senaryoda başarılı oldu. Daha fazla bilgi için, [Azure Veri Fabrikası'nı kullanarak verileri Azure Bilişsel Arama dizinine İdet'e](https://docs.microsoft.com/azure/data-factory/data-factory-azure-search-connector)bakın.

**S: Azure'da IaaS'da çalışan SQL Server dışındaki veritabanlarıyla Azure SQL dizinleyicisini kullanabilir miyim?**

Hayır. Dizinleyiciyi SQL Server dışında herhangi bir veritabanıyla test etmediğimiz için bu senaryoyu desteklemeyiz.  

**S: Bir zamanlamada çalışan birden çok dizin oluşturabilirsiniz?**

Evet. Ancak, aynı anda tek bir düğüm üzerinde yalnızca bir dizinleyici çalışıyor olabilir. Aynı anda çalışan birden çok dizin leyiciye ihtiyacınız varsa, arama hizmetinizi birden fazla arama birimine ölçeklemeyi düşünün.

**S: Dizin oluşturma sorgu iş yükümü etkiler mi?**

Evet. Dizinleyici, arama hizmetinizdeki düğümlerden birinde çalışır ve düğümün kaynaklarıdizin oluşturma ve sorgu trafiği ile diğer API istekleri arasında paylaşılır. Yoğun dizin oluşturma ve sorgu iş yükleri çalıştırırsanız ve 503 hata veya artan yanıt süreleri yüksek bir oranda karşılaşırsanız, [arama hizmeti ölçekleme](search-capacity-planning.md)düşünün.

**S: Bir veri kaynağı olarak [bir failover kümesinde](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview) ikincil bir yineleme kullanabilir miyim?**

Duruma göre değişir. Bir tablo veya görünümün tam dizinoluşturma için ikincil bir yineleme kullanabilirsiniz. 

Artımlı dizin oluşturma için Azure Bilişsel Arama iki değişiklik algılama ilkesini destekler: SQL tümleşik değişiklik izleme ve Yüksek Su İşareti.

Salt okunur yinelemelerde, SQL veritabanı tümleşik değişiklik izlemeyi desteklemez. Bu nedenle, Yüksek Su İşareti ilkesi kullanmanız gerekir. 

Standart tavsiyemiz, yüksek su işareti sütunu için rowversion veri türünü kullanmaktır. Ancak, rowversion kullanarak SQL Veritabanı `MIN_ACTIVE_ROWVERSION` işlevi, hangi okunur yinelemeler üzerinde desteklenmeyen dayanır. Bu nedenle, rowversion kullanıyorsanız bir birincil yineleme için dizinleyici işaret gerekir.

Salt okunur yinelemede rowversion'u kullanmaya çalışırsanız, aşağıdaki hatayı görürsünüz: 

    "Using a rowversion column for change tracking is not supported on secondary (read-only) availability replicas. Please update the datasource and specify a connection to the primary availability replica.Current database 'Updateability' property is 'READ_ONLY'".

**S: Yüksek su işareti değişikliği izleme için alternatif, satır dışı bir sürüm sütunu kullanabilir miyim?**

Tavsiye edilmez. Yalnızca **rowversion** güvenilir veri eşitleme sağlar. Ancak, uygulama mantığınıza bağlı olarak, aşağıdakiler varsa güvenli olabilir:

+ Dizin leyici çalıştığında, tabloda dizine eklenmiş bekleyen hareketler olmadığından emin olabilirsiniz (örneğin, tüm tablo güncelleştirmeleri bir zamanlamada toplu iş olarak gerçekleşir ve Azure Bilişsel Arama dizin leyici zamanlaması tabloyla çakışmasını önlemek için ayarlanmıştır zamanlamayı güncelleştirin).  

+ Kaçırılan satırları almak için düzenli olarak tam bir reindex yaparsınız. 