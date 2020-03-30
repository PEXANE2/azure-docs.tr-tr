---
title: Alma sırasında verileri tarama için dizinleyiciler
titleSuffix: Azure Cognitive Search
description: Kullanılabilir verileri ayıklamak ve Azure Bilişsel Arama dizinini doldurmak için Azure SQL veritabanını, Azure Cosmos DB'yi veya Azure depolamayı tarayın.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.custom: fasttrack-edit
ms.openlocfilehash: 2719bba0e88ba3125bd5ba163804e31885b286a2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79282997"
---
# <a name="indexers-in-azure-cognitive-search"></a>Azure Bilişsel Arama'daki Dizin Oluşturucular

Azure Bilişsel Arama'daki *dizin leyici,* dış azure veri kaynağından aranabilir verileri ve meta verileri ayıklayan ve dizin le veri kaynağınız arasındaki alandan alana eşlemelere dayalı bir dizini dolduran bir tarayıcıdır. Hizmet, bir dizine veri ekleyen herhangi bir kod yazmak zorunda kalmadan verileri çektiğinden, bu yaklaşım bazen 'çekme modeli' olarak adlandırılır.

Dizin leyiciler, Azure, Cosmos DB, Azure Tablo Depolama ve Blob Depolama'da SQL Server için tek tek dizin leyiciler ile veri kaynağı türlerini veya platformlarını temel almaktadır. Blob depolama dizinleyicileri, blob içerik türlerine özgü ek özelliklere sahiptir.

Bir dizin oluşturucusunu yalnızca veri alımı amacıyla kullanabilir veya dizininize alanların yalnızca bazılarını yüklemek için bir dizin oluşturucu kullanımını içeren bir teknikler birleşimini kullanabilirsiniz.

Dizin işaretçilerini isteğe bağlı olarak veya her beş dakikada bir çalışan yinelenen bir veri yenileme zamanlamasında çalıştırabilirsiniz. Daha sık yapılan güncelleştirmeler, hem Azure Bilişsel Arama'daki hem de harici veri kaynağınızdaki verileri aynı anda güncelleyen bir itme modeli gerektirir.

## <a name="approaches-for-creating-and-managing-indexers"></a>Dizin oluşturucular oluşturma ve yönetme yaklaşımları

Dizin oluşturucuları aşağıdaki yaklaşımlarla oluşturabilir ve yönetebilirsiniz:

* [Portal > Alma Veri Sihirbazı](search-import-data-portal.md)
* [Hizmet REST API'si](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations)
* [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.iindexersoperations)

Başlangıçta, yeni bir dizin oluşturucu bir önizleme özelliği olarak duyurulur. Önizleme özellikleri API'lere (REST ve .NET) eklenir ve ardından genel kullanıma açık hale geldiklerinde portala entegre edilir. Yeni bir dizin oluşturucuyu değerlendiriyorsanız kod yazmayı planlamanız gerekir.

## <a name="permissions"></a>İzinler

Durum veya tanımlar için GET istekleri de dahil olmak üzere dizinleyicilerle ilgili tüm işlemler, [bir yönetici api anahtarı](search-security-api-keys.md)gerektirir. 

<a name="supported-data-sources"></a>

## <a name="supported-data-sources"></a>Desteklenen veri kaynakları

Dizin leyiciler Azure'da veri depolarını tarar.

* [Azure Blob Depolama](search-howto-indexing-azure-blob-storage.md)
* [Azure Veri Gölü Depolama Gen2](search-howto-index-azure-data-lake-storage.md) (önizlemede)
* [Azure Tablo Depolama](search-howto-indexing-azure-tables.md)
* [Azure Cosmos DB](search-howto-index-cosmosdb.md)
* [Azure SQL Veritabanı](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Azure Sanal Makinelerde SQL Server](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md)
* [Azure'da SQL Yönetilen örnekler](search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers.md)

## <a name="basic-configuration-steps"></a>Temel yapılandırma adımları
Dizin oluşturucular veri kaynağına özgü özellikler sunabilir. Bu bakımdan, dizin oluşturucu veya veri kaynağı yapılandırmasının bazı boyutları dizin oluşturucu türüne göre farklılık gösterir. Bununla birlikte, tüm dizin oluşturucuların temel birleşimi ve gereksinimleri aynıdır. Tüm dizin oluşturucularda ortak olan adımlar aşağıda ele alınmıştır.

### <a name="step-1-create-a-data-source"></a>1. Adım: Veri kaynağı oluşturma
Dizinleyici, bir *veri kaynağı* nesnesinden veri kaynağı bağlantısı elde eder. Veri kaynağı tanımı bir bağlantı dizesi ve büyük olasılıkla kimlik bilgileri sağlar. Kaynağı oluşturmak için [Veri Kaynağı OLUŞTURMA](https://docs.microsoft.com/rest/api/searchservice/create-data-source) API veya [DataSource sınıfını](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasource) arayın.

Veri kaynakları, bunları kullanan dizin oluşturuculardan bağımsız olarak yapılandırılır ve yönetilir. Bu da bir veri kaynağının, bir seferde birden çok dizin yüklemek amacıyla birden çok dizin oluşturucu tarafından kullanılabileceği anlamına gelir.

### <a name="step-2-create-an-index"></a>2. Adım: Dizin oluşturma
Dizin oluşturucu veri alımıyla ilgili bazı görevleri otomatikleştirir, ancak dizin oluşturma genellikle bu görevlerden biri değildir. Bir önkoşul olarak dış veri kaynağınızdaki alanlarla eşleşen alanlara sahip önceden tanımlı bir dizininiz olmalıdır. Alanların ada ve veri türüne göre eşleşmesi gerekir. Bir dizin yapılandırma hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/rest/api/searchservice/Create-Index) [Index class](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index) Alan ilişkilendirmeleriyle ilgili yardım için Azure [Bilişsel Arama dizinleyicilerinde Alan eşlemelerine](search-indexer-field-mappings.md)bakın.

> [!Tip]
> Dizin oluşturucular sizin için dizin oluşturamasa da, portaldaki **Verileri içeri aktarma** sihirbazı bu işlem için size yardımcı olabilir. Çoğu durumda, sihirbaz, kaynaktaki mevcut meta verilerden dizin şeması çıkarsayarak, sihirbaz etkin olduğunda satır içinde düzenleyebileceğiniz geçici bir dizin şeması sunar. Hizmet için sihirbaz oluşturulduğunda, portalda yapılabilecek ayrıntılı düzenlemeler, genellikle yeni alanlar eklemeyle sınırlıdır. Sihirbaz dizin oluşturmak için uygun olsa da, düzenlemek için uygun değildir. Uygulama yaparak öğrenmek için, [portal kılavuzundaki](search-get-started-portal.md) adımları izleyin.

### <a name="step-3-create-and-schedule-the-indexer"></a>3. Adım: Dizin oluşturucuyu oluşturma ve zamanlama
Dizinleyici tanımı, veri alımıyla ilgili tüm öğeleri bir araya getiren bir yapıdır. Gerekli öğeler bir veri kaynağı ve dizin içerir. İsteğe bağlı öğeler bir zamanlama ve alan eşlemeleri içerir. Alan eşleme yalnızca kaynak alanları ve dizin alanları açıkça karşılık gelirse isteğe bağlıdır. Bir dizin oluşturucu su çıktığı hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/rest/api/searchservice/Create-Indexer)

<a id="RunIndexer"></a>

## <a name="run-indexers-on-demand"></a>Dizin işaretçilerini isteğe bağlı çalıştırma

Dizin oluşturmayı zamanlama yaygın olsa da, Bir dizinleyici de [Run komutunu](https://docs.microsoft.com/rest/api/searchservice/run-indexer)kullanarak isteğe bağlı çağrılabilir:

    POST https://[service name].search.windows.net/indexers/[indexer name]/run?api-version=2019-05-06
    api-key: [Search service admin key]

> [!NOTE]
> Run API başarıyla döndüğünde, dizinleyici çağırması zamanlanmış, ancak gerçek işlem eşzamanlı olarak gerçekleşir. 

Portaldaki dizinleyici durumunu izleyebilir veya Dizinleyici Durum API'sını Al aracılığıyla. 

<a name="GetIndexerStatus"></a>

## <a name="get-indexer-status"></a>Dizinleyici durumunu alma

Dizinleyici durumu ve yürütme geçmişini Al [Dizinleyici Durumu komutu](https://docs.microsoft.com/rest/api/searchservice/get-indexer-status)ile alabilirsiniz:


    GET https://[service name].search.windows.net/indexers/[indexer name]/status?api-version=2019-05-06
    api-key: [Search service admin key]

Yanıt, genel dizinleyici durumunu, son (veya devam eden) dizinleyici çağrısını ve son dizinleyici çağrılarının geçmişini içerir.

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

Yürütme geçmişi, ters kronolojik sırada sıralanmış en son tamamlanan 50 yürütmeyi içerir (bu nedenle yanıtta ilk önce en son yürütme gelir).

## <a name="next-steps"></a>Sonraki adımlar
Artık temel fikri anladığınıza göre, atmanız gereken bir sonraki adım her bir veri kaynağı türüne özgü gereksinimleri ve görevleri incelemektir.

* [Bir Azure sanal makinesinde Azure SQL Veritabanı veya SQL Server](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Azure Cosmos DB](search-howto-index-cosmosdb.md)
* [Azure Blob Depolama](search-howto-indexing-azure-blob-storage.md)
* [Azure Tablo Depolama](search-howto-indexing-azure-tables.md)
* [Azure Bilişsel Arama Blob dizinleyicisini kullanarak CSV bloblarını dizine alma](search-howto-index-csv-blobs.md)
* [Azure Bilişsel Arama Blob dizinleyici ile JSON blobs dizinleme](search-howto-index-json-blobs.md)
