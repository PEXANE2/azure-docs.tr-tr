---
title: Azure Blob depolama içeriği üzerinden arama yapın
titleSuffix: Azure Cognitive Search
description: Azure Blob Depolama'yı nasıl dizine ekleyip, Azure Bilişsel Arama ile belgelerden metin ayıklamayı öğrenin.
manager: nitinme
author: mgottein
ms.author: magottei
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.custom: fasttrack-edit
ms.openlocfilehash: 5df1198e6681431738f886eb7c3ad549936eab1a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80067654"
---
# <a name="how-to-index-documents-in-azure-blob-storage-with-azure-cognitive-search"></a>Azure Bilişsel Arama ile Azure Blob Depolama'da belgeleri dizin oluşturma

Bu makalede, Azure Blob depolama alanında depolanan belgeleri (PDF'ler, Microsoft Office belgeleri ve diğer bazı yaygın biçimler gibi) dizinlemek için Azure Bilişsel Arama'nın nasıl kullanılacağı gösterilmektedir. İlk olarak, bir blob dizinleyici kurma ve yapılandırma temellerini açıklar. Daha sonra, karşılaşabileceğiniz davranışlar ve senaryolar hakkında daha derin bir keşif sunar.

<a name="SupportedFormats"></a>

## <a name="supported-document-formats"></a>Desteklenen belge biçimleri
Blob dizinleyici aşağıdaki belge biçimlerinden metin ayıklayabilirsiniz:

[!INCLUDE [search-blob-data-sources](../../includes/search-blob-data-sources.md)]

## <a name="setting-up-blob-indexing"></a>Blob dizin oluşturma
Şunları kullanarak bir Azure Blob Depolama dizinleyicisi ayarlayabilirsiniz:

* [Azure portalında](https://ms.portal.azure.com)
* Azure Bilişsel Arama [REST API](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations)
* Azure Bilişsel Arama [.NET SDK](https://aka.ms/search-sdk)

> [!NOTE]
> Bazı özellikler (örneğin, alan eşlemeleri) henüz portalda mevcut değildir ve programlı olarak kullanılması gerekir.
>

Burada, REST API kullanarak akışı göstermek.

### <a name="step-1-create-a-data-source"></a>1. Adım: Veri kaynağı oluşturma
Bir veri kaynağı, hangi verileri dizine ekleyip, verilere erişmek için gereken kimlik bilgilerini ve verilerdeki değişiklikleri (yeni, değiştirilmiş veya silinmiş satırlar) verimli bir şekilde tanımlamak için ilkeleri belirtir. Bir veri kaynağı, aynı arama hizmetinde birden çok dizinleyici tarafından kullanılabilir.

Blob dizinoluşturma için, veri kaynağının aşağıdaki gerekli özelliklere sahip olması gerekir:

* **adı,** arama hizmetinizdeki veri kaynağının benzersiz adıdır.
* **türü** olmalıdır. `azureblob`
* **kimlik bilgileri** parametre olarak `credentials.connectionString` depolama hesabı bağlantı dizesini sağlar. Ayrıntılar için aşağıdaki [kimlik bilgilerini nasıl belirtin.](#Credentials)
* **kapsayıcı** depolama hesabınızda bir kapsayıcı belirtir. Varsayılan olarak, kapsayıcı içindeki tüm lekeler geri alınır. Yalnızca belirli bir sanal dizinde blobs dizine dizini yapmak istiyorsanız, isteğe bağlı **sorgu** parametresini kullanarak bu dizini belirtebilirsiniz.

Bir veri kaynağı oluşturmak için:

    POST https://[service name].search.windows.net/datasources?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "<optional-virtual-directory-name>" }
    }   

Veri Kaynağı OLUŞTUR API'si hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/rest/api/searchservice/create-data-source)

<a name="Credentials"></a>
#### <a name="how-to-specify-credentials"></a>Kimlik bilgileri nasıl belirtilir? ####

Blob kapsayıcısının kimlik bilgilerini aşağıdaki yollardan biriyle sağlayabilirsiniz:

- **Tam erişimli depolama hesabı bağlantı dizesi**: `DefaultEndpointsProtocol=https;AccountName=<your storage account>;AccountKey=<your account key>` Ayarlar > Ayarlar > Anahtarları'na (Klasik depolama hesapları için) veya Ayarlar > Erişim anahtarlarına (Azure Kaynak Yöneticisi depolama hesapları için) > depolama hesabı bıçağına giderek bağlantı dizesini Azure portalından alabilirsiniz.
- **Depolama hesabı paylaşılan erişim imzası** (SAS) bağlantı dizesi: `BlobEndpoint=https://<your account>.blob.core.windows.net/;SharedAccessSignature=?sv=2016-05-31&sig=<the signature>&spr=https&se=<the validity end time>&srt=co&ss=b&sp=rl` SAS listeye sahip olmalı ve kapsayıcılar ve nesneler (bu durumda blobs) üzerinde izinleri okuma.
-  **Konteyner paylaşılan**erişim `ContainerSharedAccessUri=https://<your storage account>.blob.core.windows.net/<container name>?sv=2016-05-31&sr=c&sig=<the signature>&se=<the validity end time>&sp=rl` imzası : SAS listeye sahip olmalı ve kapsayıcıdaki izinleri okumalıdır.

Depolama paylaşılan erişim imzaları hakkında daha fazla bilgi için [bkz.](../storage/common/storage-dotnet-shared-access-signature-part-1.md)

> [!NOTE]
> SAS kimlik bilgilerini kullanıyorsanız, son kullanma sürelerini önlemek için veri kaynağı kimlik bilgilerini düzenli aralıklarla yenilenen imzalarla güncelleştirmeniz gerekir. SAS kimlik bilgilerinin süresi dolursa, dizinleyici `Credentials provided in the connection string are invalid or have expired.`' ye benzer bir hata iletisi ile başarısız olur.  

### <a name="step-2-create-an-index"></a>2. Adım: Dizin oluşturma
Dizin, bir belgedeki alanları, öznitelikleri ve arama deneyimini şekillendiren diğer yapıları belirtir.

Lekelerden çıkarılan metni depolamak için `content` aranabilir alana sahip bir dizin şu şekilde oluşturulur:   

    POST https://[service name].search.windows.net/indexes?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
          "name" : "my-target-index",
          "fields": [
            { "name": "id", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "content", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false }
          ]
    }

Dizin oluşturma hakkında daha fazla şey için [bkz.](https://docs.microsoft.com/rest/api/searchservice/create-index)

### <a name="step-3-create-an-indexer"></a>Adım 3: Bir dizin oluştur
Dizin leyici, bir veri kaynağını hedef arama dizinine bağlar ve veri yenilemesini otomatikleştirmek için bir zamanlama sağlar.

Dizin ve veri kaynağı oluşturulduktan sonra dizin oluşturabilirsiniz:

    POST https://[service name].search.windows.net/indexers?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "blob-indexer",
      "dataSourceName" : "blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }

Bu dizinleyici her iki saatte bir çalışır (zamanlama aralığı "PT2H" olarak ayarlanır). Her 30 dakikada bir dizinleyici çalıştırmak için aralığı "PT30M" olarak ayarlayın. En kısa desteklenen aralık 5 dakikadır. Zamanlama isteğe bağlıdır - atlanırsa, dizin oluşturucu oluşturulduğunda yalnızca bir kez çalışır. Ancak, istediğiniz zaman isteğe bağlı bir dizinleyici çalıştırabilirsiniz.   

Create Indexer API hakkında daha fazla bilgi [için, Oluştur Dizinleyici'ye](https://docs.microsoft.com/rest/api/searchservice/create-indexer)göz atın.

Dizin oluşturma zamanlamaları hakkında daha fazla bilgi için Azure Bilişsel Arama için dizin oluşturma yı [nasıl zamanlayınız'](search-howto-schedule-indexers.md)a bakın.

<a name="how-azure-search-indexes-blobs"></a>

## <a name="how-azure-cognitive-search-indexes-blobs"></a>Azure Bilişsel Arama dizinleri blobs nasıl

[Dizinleyici yapılandırmasına](#PartsOfBlobToIndex)bağlı olarak, blob dizinleyici depolama meta verilerini yalnızca (yalnızca meta verileri önemsediğiniz ve blobların içeriğini dizine almanız gerekmediğinde yararlı), depolama ve içerik meta verileri veya hem meta veriler hem de metin içeriği dizini yapabilir. Varsayılan olarak, dizinleyici hem meta verileri hem de içeriği ayıklar.

> [!NOTE]
> Varsayılan olarak, JSON veya CSV gibi yapılandırılmış içeriğe sahip blob'lar tek bir metin yığını olarak dizine eklenir. JSON ve CSV blobs'u yapılandırılmış bir şekilde dizine almak istiyorsanız, daha fazla bilgi için [Dizin oluşturma JSON blobs](search-howto-index-json-blobs.md) ve [Dizin oluşturma CSV blobs'a](search-howto-index-csv-blobs.md) bakın.
>
> Bileşik veya katıştırılmış belge (zip arşivi veya ekleri içeren gömülü Outlook e-postası içeren Word belgesi gibi) da tek bir belge olarak dizine eklenir.

* Belgenin metin içeriği adlı `content`bir dize alanına ayıklanır.

> [!NOTE]
> Azure Bilişsel Arama, fiyatlandırma katmanına bağlı olarak ne kadar metin ayıklar: Ücretsiz katman için 32.000 karakter, Temel için 64.000, Standart için 4 milyon, Standart S2 için 8 milyon ve Standart S3 için 16 milyon. Kesilen belgeler için dizinleyici durum yanıtına bir uyarı dahildir.  

* Blob üzerinde bulunan kullanıcı tarafından belirtilen meta veri özellikleri, varsa, tam olarak ayıklanır. Bunun, dizinde blobun meta veri anahtarıyla aynı ada sahip bir alanın tanımlanmasını gerektirdiğini unutmayın. Örneğin, `Sensitivity` blob değeri `High`olan bir meta veri anahtarı varsa, arama `Sensitivity` dizininizde adlı bir alan tanımlamanız `High`gerekir ve değeri ile doldurulur.
* Standart blob meta veri özellikleri aşağıdaki alanlara ayıklanır:

  * **meta\_veri\_depolama adı** (Edm.String) - blob dosya adı. Örneğin, bir blob /my-container/my-folder/subfolder/resume.pdf varsa, bu alanın `resume.pdf`değeri .
  * **meta\_veri\_depolama yolu** (Edm.String) - depolama hesabı da dahil olmak üzere blob tam URI. Örneğin, `https://myaccount.blob.core.windows.net/my-container/my-folder/subfolder/resume.pdf`
  * **meta\_veri\_\_depolama içerik türü** (Edm.String) - blob yüklemek için kullandığınız kod tarafından belirtilen içerik türü. Örneğin, `application/octet-stream`.
  * **meta\_veri\_\_depolama son değiştirildi** (Edm.DateTimeOffset) - blob için son değiştirilmiş zaman damgası. Azure Bilişsel Arama, değiştirilen lekeleri tanımlamak ve ilk dizin oluşturmadan sonra her şeyi yeniden dizine almamak için bu zaman damgasını kullanır.
  * **meta\_veri\_depolama boyutu** (Edm.Int64) - bayt blob boyutu.
  * **meta\_veri\_\_depolama içeriği md5** (Edm.String) - varsa blob içeriğinin MD5 karma.
  * **meta\_veri\_depolama\_sas belirteci** (Edm.String) - Blob erişmek için [özel beceriler](cognitive-search-custom-skill-interface.md) tarafından kullanılabilecek geçici bir SAS belirteci. Bu belirteç, süresi dolabileceği için daha sonra kullanılmak üzere depolanmamalıdır.

* Her belge biçimine özgü meta veri özellikleri [burada](#ContentSpecificMetadata)listelenen alanlara ayıklanır.

Arama dizininizdeki yukarıdaki özelliklerin tümü için alanları tanımlamanız gerekmez - sadece uygulamanız için gereksinim duyduğunuz özellikleri yakalayın.

> [!NOTE]
> Genellikle, varolan dizininizdeki alan adları belge ayıklama sırasında oluşturulan alan adlarından farklıdır. Azure Bilişsel Arama tarafından sağlanan özellik adlarını arama dizininizdeki alan adlarıyla eşleştirmek için **alan eşlemelerini** kullanabilirsiniz. Aşağıda kullanılan alan eşlemeleri bir örnek göreceksiniz.
>
>

<a name="DocumentKeys"></a>
### <a name="defining-document-keys-and-field-mappings"></a>Belge anahtarlarını ve alan eşlemelerini tanımlama
Azure Bilişsel Arama'da belge anahtarı bir belgeyi benzersiz olarak tanımlar. Her arama dizininin Edm.String türünde tam olarak bir anahtar alanı olmalıdır. Anahtar alanı, dizine eklenen her belge için gereklidir (aslında gerekli tek alandır).  

Hangi çıkarılan alanın dizininizin anahtar alanına eşlemesi gerektiğini dikkatlice düşünmelisiniz. Adaylar şunlardır:

* **meta\_veri\_depolama adı** - bu uygun bir aday olabilir, ancak 1) farklı klasörlerde aynı ada sahip blobs olabilir, ve 2) ad tire gibi belge tuşları geçersiz karakterler içerebilir gibi, adlar benzersiz olmayabilir unutmayın. `base64Encode` [Alan eşleme işlevini](search-indexer-field-mappings.md#base64EncodeFunction) kullanarak geçersiz karakterlerle başa çıkabilirsiniz - bunu yaparsanız, Arama gibi API çağrılarına geçerken belge anahtarlarını kodlamayı unutmayın. (Örneğin, .NET'te bu amaçla [UrlTokenEncode yöntemini](https://msdn.microsoft.com/library/system.web.httpserverutility.urltokenencode.aspx) kullanabilirsiniz).
* **meta\_veri\_depolama yolu** - tam yolu kullanarak teklik `/` sağlar, ancak yol kesinlikle bir belge anahtarı [geçersiz](https://docs.microsoft.com/rest/api/searchservice/naming-rules)karakterler içerir.  Yukarıdaki gibi, `base64Encode` [işlevi](search-indexer-field-mappings.md#base64EncodeFunction)kullanarak tuşları kodlama seçeneğiniz vardır.
* Yukarıdaki seçeneklerden hiçbiri sizin için çalışmıyorsa, lekelere özel bir meta veri özelliği ekleyebilirsiniz. Ancak bu seçenek, bu meta veri özelliğini tüm blob'lara eklemek için blob yükleme işleminizin gerekli olduğunu. Anahtar gerekli bir özellik olduğundan, bu özelliğe sahip olmayan tüm lekeler dizine alınamaz.

> [!IMPORTANT]
> Dizindeki anahtar alanı için açık bir eşleme yoksa, Azure `metadata_storage_path` Bilişsel Arama anahtar ve base-64 anahtar değerlerini (yukarıdaki ikinci seçenek) kodlar olarak otomatik olarak kullanır.
>
>

Bu örnekiçin, `metadata_storage_name` alanı belge anahtarı olarak seçelim. Ayrıca dizininizin adında `key` önemli bir alana ve `fileSize` belge boyutunu depolamak için bir alana sahip olduğunu varsayalım. İşleri istenilen şekilde kablolamak için, dizin oluşturup güncellerken aşağıdaki alan eşlemelerini belirtin:

    "fieldMappings" : [
      { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key", "mappingFunction" : { "name" : "base64Encode" } },
      { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
    ]

Tüm bunları bir araya getirmek için, alan eşlemelerini şu şekilde ekleyebilir ve varolan bir dizinleyici için temel-64 anahtar kodlamasını etkinleştirebilirsiniz:

    PUT https://[service name].search.windows.net/indexers/blob-indexer?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      "dataSourceName" : " blob-datasource ",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "fieldMappings" : [
        { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key", "mappingFunction" : { "name" : "base64Encode" } },
        { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
      ]
    }

> [!NOTE]
> Alan eşlemeleri hakkında daha fazla bilgi edinmek için [bu makaleye](search-indexer-field-mappings.md)bakın.
>
>

<a name="WhichBlobsAreIndexed"></a>
## <a name="controlling-which-blobs-are-indexed"></a>Hangi blobların dizine eklenmeyi denetleme
Hangi lekelerin dizine ekolduğunu ve hangilerinin atlanabileceğini kontrol edebilirsiniz.

### <a name="index-only-the-blobs-with-specific-file-extensions"></a>Yalnızca belirli dosya uzantılarına sahip lekeleri dizine dizine
`indexedFileNameExtensions` Dizinleyici yapılandırma parametresini kullanarak yalnızca belirttiğiniz dosya adı uzantılarıyla blobları dizine dizine dizine dizine dizine ekleyebilirsiniz. Değer, dosya uzantılarının (satır aralığı noktalı) virgülle ayrılmış bir listesini içeren bir dizedir. Örneğin, yalnızca dizin. PDF ve . DOCX lekeleri, bunu yapın:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "indexedFileNameExtensions" : ".pdf,.docx" } }
    }

### <a name="exclude-blobs-with-specific-file-extensions"></a>Belirli dosya uzantılarına sahip lekeleri hariç tut
`excludedFileNameExtensions` Yapılandırma parametresini kullanarak belirli dosya adı uzantılarına sahip lekeleri dizin oluşturmadan dışlayabilirsiniz. Değer, dosya uzantılarının (satır aralığı noktalı) virgülle ayrılmış bir listesini içeren bir dizedir. Örneğin, tüm lekeleri dizine dizinlemek için . PNG ve . JPEG uzantıları, bunu:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "excludedFileNameExtensions" : ".png,.jpeg" } }
    }

Hem `indexedFileNameExtensions` de `excludedFileNameExtensions` parametreler varsa, Azure Bilişsel `indexedFileNameExtensions`Arama önce `excludedFileNameExtensions`, sonra . Bu, her iki listede de aynı dosya uzantısı varsa, dizin oluşturma nın dışında tutulacağı anlamına gelir.

<a name="PartsOfBlobToIndex"></a>
## <a name="controlling-which-parts-of-the-blob-are-indexed"></a>Blob'un hangi bölümlerinin dizine eklenmesini denetleme

Yapılandırma parametresini kullanarak blobs'un hangi `dataToExtract` bölümlerinin dizinlenmiş olduğunu denetleyebilirsiniz. Aşağıdaki değerleri alabilir:

* `storageMetadata`- yalnızca [standart blob özelliklerinin ve kullanıcı tarafından belirtilen meta verilerin](../storage/blobs/storage-properties-metadata.md) dizine eklenmiş olduğunu belirtir.
* `allMetadata`- depolama meta verilerinin ve blob içeriğinden çıkarılan [içerik türüne özgü meta verilerin](#ContentSpecificMetadata) dizine eklenmiş olduğunu belirtir.
* `contentAndMetadata`- blob çıkarılan tüm meta veri ve metin içeriği dizine olduğunu belirtir. Varsayılan değer budur.

Örneğin, yalnızca depolama meta verilerini dizine ekolarak şunları kullanın:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "dataToExtract" : "storageMetadata" } }
    }

### <a name="using-blob-metadata-to-control-how-blobs-are-indexed"></a>Blobs dizinlenir nasıl kontrol etmek için blob meta verileri kullanma

Yukarıda açıklanan yapılandırma parametreleri tüm lekeler için geçerlidir. Bazen, *tek tek blobs* dizinlenir nasıl kontrol etmek isteyebilirsiniz. Bunu, aşağıdaki blob meta veri özelliklerini ve değerlerini ekleyerek yapabilirsiniz:

| Özellik adı | Özellik değeri | Açıklama |
| --- | --- | --- |
| AzureSearch_Skip |"Doğru" |Blob dizinleyiciye blob'u tamamen atlamasını bildirir. Ne meta veri ne de içerik ayıklama denenmez. Bu, belirli bir blob tekrar tekrar başarısız olduğunda ve dizin oluşturma işlemini kesintiye uğrattığında yararlıdır. |
| AzureSearch_SkipContent |"Doğru" |Bu, `"dataToExtract" : "allMetadata"` [yukarıda](#PartsOfBlobToIndex) açıklanan ayarın belirli bir blob'a eşdeğerdir. |

<a name="DealingWithErrors"></a>
## <a name="dealing-with-errors"></a>Hatalarla başa çıkma

Varsayılan olarak, blob dizinleyici, desteklenmeyen içerik türü (örneğin, bir görüntü) ile bir blob karşılaştığı nda durur. Elbette belirli içerik `excludedFileNameExtensions` türlerini atlamak için parametrekkullanabilirsiniz. Ancak, tüm olası içerik türlerini önceden bilmeden blobs dizini yapmanız gerekebilir. Desteklenmeyen bir içerik türüyle karşılaşıldığında dizin `failOnUnsupportedContentType` oluşturmayı sürdürmek `false`için yapılandırma parametresini şu şekilde ayarlayın:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "failOnUnsupportedContentType" : false } }
    }

Azure Bilişsel Arama, bazı blob'lar için içerik türünü belirleyemiyor veya başka şekilde desteklenen içerik türündeki bir belgeyi işleyemiyor. Bu hata modunu yoksaymak için yapılandırma parametresini `failOnUnprocessableDocument` false olarak ayarlayın:

      "parameters" : { "configuration" : { "failOnUnprocessableDocument" : false } }

Azure Bilişsel Arama, dizine eklenmiş lekelerin boyutunu sınırlar. Bu sınırlar Azure [Bilişsel Arama'da Hizmet Sınırları olarak](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity)belgelenmiştir. Büyük boyutlu lekeler varsayılan olarak hata olarak kabul edilir. Ancak, yapılandırma parametresini doğru olarak ayarlarsanız, `indexStorageMetadataOnlyForOversizedDocuments` büyük boyutlu blobların depolama meta verilerini yine de dizine alabilirsiniz: 

    "parameters" : { "configuration" : { "indexStorageMetadataOnlyForOversizedDocuments" : true } }

Lekeleri ayrıştırken veya dizin lere belge eklerken, herhangi bir işlem noktasında hatalar olursa dizin oluşturmaya devam edebilirsiniz. Belirli sayıda hata yoksaymak `maxFailedItems` için, ve `maxFailedItemsPerBatch` yapılandırma parametrelerini istenilen değerlere ayarlayın. Örnek:

    {
      ... other parts of indexer definition
      "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 10 }
    }

## <a name="incremental-indexing-and-deletion-detection"></a>Artımlı dizin oluşturma ve silme algılama

Bir zamanlamada çalışacak bir blob dizinleyici ayarladığınızda, blob `LastModified` zaman damgası tarafından belirlenen yalnızca değiştirilen lekeleri yeniden dizine röşey.

> [!NOTE]
> Bir değişiklik algılama ilkesi belirtmeniz gerekmez – artımlı dizin oluşturma sizin için otomatik olarak etkinleştirilir.

Belgeleri silmeyi desteklemek için "yumuşak silme" yaklaşımını kullanın. Lekeleri düpedüz silerseniz, ilgili belgeler arama dizininden kaldırılmaz.

Yumuşak silme yaklaşımını uygulamanın iki yolu vardır. Her ikisi de aşağıda açıklanmıştır.

### <a name="native-blob-soft-delete-preview"></a>Yerel blob yumuşak silme (önizleme)

> [!IMPORTANT]
> Yerel blob yumuşak silme desteği önizlemede. Önizleme işlevi hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın. [REST API sürümü 2019-05-06-Önizleme](https://docs.microsoft.com/azure/search/search-api-preview) bu özelliği sağlar. Şu anda portal veya .NET SDK desteği yok.

> [!NOTE]
> Yerel blob yumuşak silme ilkesini kullanırken dizininizdeki belgelerin belge anahtarları bir blob özelliği veya blob meta veri olmalıdır.

Bu yöntemde Azure [Blob](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete) depolama tarafından sunulan yerel blob yumuşak silme özelliğini kullanırsınız. Depolama hesabınızda yerel blob yumuşak silme etkinleştirilmişse, veri kaynağınızda yerel bir yumuşak silme ilkesi kümesi varsa ve dizinleyici yumuşak silinmiş bir duruma geçişedilmiş bir blob bulur, dizinleyici bu belgeyi dizinden kaldırır. Azure Veri Gölü Depolama Gen2'den blob'ları dizine ekstediğinde yerel blob soft silme ilkesi desteklenmez.

Aşağıdaki adımları kullanın:
1. [Azure Blob depolama alanı için yerel yumuşak silmeyi](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete)etkinleştirin. Bekletme ilkesini dizin leyici aralığı zamanlamanızdan çok daha yüksek bir değere ayarlamanızı öneririz. Dizinleyiciyi çalıştıran bir sorun varsa veya dizin dizinlediğiniz çok sayıda belge varsa, dizinleyicinin sonunda silinen yumuşak lekeleri işlemesi için bolca zaman vardır. Azure Bilişsel Arama dizin leyicileri, bir belgeyi yalnızca blob'u yumuşak silinmiş durumdayken işlerse dizinden siler.
1. Veri kaynağında yerel bir blob yumuşak silme algılama ilkesini yapılandırın. Aşağıda bir örnek gösterilmiştir. Bu özellik önizlemede olduğundan, önizleme REST API'sini kullanmanız gerekir.
1. Dizinleyiciyi çalıştırın veya dizinleyiciyi zamanlamada çalışacak şekilde ayarlayın. Dizinleyici blob çalışır ve işler, belge dizin kaldırılır.

    ```
    PUT https://[service name].search.windows.net/datasources/blob-datasource?api-version=2019-05-06-Preview
    Content-Type: application/json
    api-key: [admin key]
    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : null },
        "dataDeletionDetectionPolicy" : {
            "@odata.type" :"#Microsoft.Azure.Search.NativeBlobSoftDeleteDeletionDetectionPolicy"
        }
    }
    ```

#### <a name="reindexing-undeleted-blobs"></a>Silinmemiş lekeleri yeniden dizine alma

Azure Blob depolama deposundan, depolama hesabınızda yerel yumuşak silme etkinleştirilmiş bir blob silerseniz, blob, bekletme süresi içinde bu blob'u silme seçeneği ni içeren yumuşak silinmiş bir duruma geçecektir. Bir Azure Bilişsel Arama veri kaynağında yerel bir blob yumuşak silme ilkesi varsa ve dizinleyici yumuşak silinmiş bir blob işlerse, bu belgeyi dizinden kaldırır. Bu blob daha sonra silinmemiş ise dizinleyici her zaman bu blob reindex olmaz. Bunun nedeni, dizinleyiciblo'nun `LastModified` zaman damgasını temel alan diziliş hangi lekeleri belirler. Yumuşak silinmiş bir blob `LastModified` silinmediğinde zaman damgası güncelleştirilmemişse, dizinleyici `LastModified` silinmemiş blob'dan daha yeni zaman damgalı blob'ları işlemişse silinmemiş blob'u yeniden dizine almaz. Silinmemiş bir blob'un yeniden dizine eklenmiş olduğundan emin olmak `LastModified` için, blob'un zaman damgasını güncelleştirmeniz gerekir. Bunu yapmanın bir yolu, bu lekenin meta verilerini yeniden kaydetmektir. Meta verileri değiştirmeniz gerekmez, ancak meta verileri yeniden kaydetmek, dizinin `LastModified` dizini bu blob'u yeniden dizine ekolarak yapması gerektiğini bilmesi için blob'un zaman damgasını günceller.

### <a name="soft-delete-using-custom-metadata"></a>Özel meta verileri kullanarak yumuşak silme

Bu yöntemde, bir belgenin arama dizininden ne zaman kaldırılması gerektiğini belirtmek için bir blob meta verisi kullanırsınız.

Aşağıdaki adımları kullanın:

1. Azure Bilişsel Arama'ya mantıksal olarak silindiğini belirtmek için blob'a özel bir meta veri anahtar değeri çifti ekleyin.
1. Veri kaynağında yumuşak bir silme sütunu algılama ilkesini yapılandırın. Aşağıda bir örnek gösterilmiştir.
1. Dizinleyici blob'u işleyip belgeyi dizinden sildikten sonra Azure Blob depolama alanı için blob'u silebilirsiniz.

Örneğin, değeri olan bir meta veri özelliği `IsDeleted` varsa, aşağıdaki ilke bir `true`blob silinecek dikkate alır:

    PUT https://[service name].search.windows.net/datasources/blob-datasource?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : null },
        "dataDeletionDetectionPolicy" : {
            "@odata.type" :"#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",     
            "softDeleteColumnName" : "IsDeleted",
            "softDeleteMarkerValue" : "true"
        }
    }

#### <a name="reindexing-undeleted-blobs"></a>Silinmemiş lekeleri yeniden dizine alma

Veri kaynağınızda yumuşak bir silme sütun algılama ilkesi ayarlarsanız, işaretdeğeri olan bir blob'a özel meta verileri ekleyin, ardından dizinleyiciyi çalıştırırsanız, dizinleyici bu belgeyi dizinden kaldırır. Bu belgeyi yeniden dizine almak istiyorsanız, bu blob için yumuşak silme meta veri değerini değiştirmeniz ve dizinleyiciyi yeniden çalıştırmanız yeterlidir.

## <a name="indexing-large-datasets"></a>Büyük veri kümelerini dizine alma

Lekeleri dizine alma zaman alan bir işlem olabilir. Dizin olarak milyonlarca blob'unuzun olduğu durumlarda, verilerinizi bölümlere alarak ve verileri paralel olarak işlemek için birden çok dizinleyici kullanarak dizin oluşturmayı hızlandırabilirsiniz. Bunu şu şekilde ayarlayabilirsiniz:

- Verilerinizi birden çok blob kapsayıcısına veya sanal klasörlere bölme
- Konteyner veya klasör başına birden fazla Azure Bilişsel Arama veri kaynağı ayarlayın. Blob klasörüne işaret etmek `query` için parametreyi kullanın:

    ```
    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : "my-folder" }
    }
    ```

- Her veri kaynağı için karşılık gelen bir dizin oluştur. Tüm dizinleyiciler aynı hedef arama dizinini gösterebilir.  

- Hizmetinizdeki bir arama birimi herhangi bir zamanda bir dizinleyici çalıştırabilir. Yukarıda açıklandığı gibi birden çok dizin oluştursanız, yalnızca gerçekten paralel olarak çalıştıklarında yararlıdır. Birden çok dizin oluşturup paralel olarak çalıştırmak için, uygun sayıda bölüm ve yineleme oluşturarak arama hizmetinizi ölçeklendirin. Örneğin, arama hizmetinizde 6 arama birimi (örneğin, 2 bölüm x 3 yineleme) varsa, 6 dizinleyici aynı anda çalıştırılabilir ve bu da dizin oluşturma iş biriminde altı kat artışa neden olabilir. Ölçeklendirme ve kapasite planlaması hakkında daha fazla bilgi edinmek için Azure [Bilişsel Arama'da sorgu ve iş yüklerini dizine ekleme için ölçek kaynak düzeylerine](search-capacity-planning.md)bakın.

## <a name="indexing-documents-along-with-related-data"></a>Belgeleri ilgili verilerle birlikte dizine alma

Dizininizdeki birden çok kaynaktan gelen belgeleri "birleştirmek" isteyebilirsiniz. Örneğin, blobs'daki metni Cosmos DB'de depolanan diğer meta verilerle birleştirmek isteyebilirsiniz. Hatta birden çok bölümden arama belgeleri oluşturmak için çeşitli dizinleyiciler ile birlikte push indexing API kullanabilirsiniz. 

Bunun işe yaraması için, tüm dizinleyicilerin ve diğer bileşenlerin belge anahtarı üzerinde anlaştığı gerekir. Bu konuyla ilgili ek ayrıntılar için, [Dizin birden çok Azure veri kaynağına](https://docs.microsoft.com/azure/search/tutorial-multiple-data-sources)bakın. Ayrıntılı bir gezinme için şu dış makaleye bakın: [Belgeleri Azure Bilişsel Arama'daki diğer verilerle birleştirin.](https://blog.lytzen.name/2017/01/combine-documents-with-other-data-in.html)

<a name="IndexingPlainText"></a>
## <a name="indexing-plain-text"></a>Düz metni dizine alma 

Tüm lekeleriniz aynı kodlamada düz metin içeriyorsa, **metin ayrıştma modunu**kullanarak dizin oluşturma performansını önemli ölçüde artırabilirsiniz. Metin ayrıştma modunu `parsingMode` kullanmak için `text`yapılandırma özelliğini şu şekilde ayarlayın:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "parsingMode" : "text" } }
    }

Varsayılan olarak, `UTF-8` kodlama kabul edilir. Farklı bir kodlama belirtmek için `encoding` yapılandırma özelliğini kullanın: 

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "parsingMode" : "text", "encoding" : "windows-1252" } }
    }


<a name="ContentSpecificMetadata"></a>
## <a name="content-type-specific-metadata-properties"></a>İçerime özgü meta veri özellikleri
Aşağıdaki tablo, her belge biçimi için yapılan işlemleri özetler ve Azure Bilişsel Arama tarafından ayıklanan meta veri özelliklerini açıklar.

| Belge biçimi / içerik türü | İçerik türüne özgü meta veri özellikleri | İşlem ayrıntıları |
| --- | --- | --- |
| HTML (metin/html) |`metadata_content_encoding`<br/>`metadata_content_type`<br/>`metadata_language`<br/>`metadata_description`<br/>`metadata_keywords`<br/>`metadata_title` |HTML biçimlendirmeyi şeritle ve metni ayıkla |
| PDF (uygulama/pdf) |`metadata_content_type`<br/>`metadata_language`<br/>`metadata_author`<br/>`metadata_title` |Katıştırılmış belgeler (resimler hariç) dahil olmak üzere metni ayıklama |
| DOCX (uygulama/vnd.openxmlformats-officedocument.wordprocessingml.document) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Katıştılmış belgeler de dahil olmak üzere metni ayıklama |
| DOC (uygulama/msword) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Katıştılmış belgeler de dahil olmak üzere metni ayıklama |
| DOCM (uygulama/vnd.ms-word.document.macroenabled.12) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Katıştılmış belgeler de dahil olmak üzere metni ayıklama |
| WORD XML (uygulama/vnd.ms-word2006ml) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |XML biçimlendirmeyi şeritle ve metni ayıkla |
| WORD 2003 XML (uygulama/vnd.ms-wordml) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date` |XML biçimlendirmeyi şeritle ve metni ayıkla |
| XLSX (uygulama/vnd.openxmlformats-officedocument.elektronik tabloml.sheet) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Katıştılmış belgeler de dahil olmak üzere metni ayıklama |
| XLS (uygulama/vnd.ms-excel) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Katıştılmış belgeler de dahil olmak üzere metni ayıklama |
| XLSM (uygulama/vnd.ms-excel.sheet.macroenabled.12) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Katıştılmış belgeler de dahil olmak üzere metni ayıklama |
| PPTX (uygulama/vnd.openxmlformats-officedocument.presentationml.presentation) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Katıştılmış belgeler de dahil olmak üzere metni ayıklama |
| PPT (uygulama/vnd.ms-powerpoint) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Katıştılmış belgeler de dahil olmak üzere metni ayıklama |
| PPTM (uygulama/vnd.ms-powerpoint.presentation.macroenabled.12) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Katıştılmış belgeler de dahil olmak üzere metni ayıklama |
| MSG (uygulama/vnd.ms-outlook) |`metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_from_email`<br/>`metadata_message_to`<br/>`metadata_message_to_email`<br/>`metadata_message_cc`<br/>`metadata_message_cc_email`<br/>`metadata_message_bcc`<br/>`metadata_message_bcc_email`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_subject` |Ekler de dahil olmak üzere metni ayıklayın. `metadata_message_to_email`, `metadata_message_cc_email` `metadata_message_bcc_email` ve dize koleksiyonları, alanların geri kalanı dizeleri vardır.|
| ODT (uygulama/vnd.oasis.opendocument.text) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Katıştılmış belgeler de dahil olmak üzere metni ayıklama |
| ODS (uygulama/vnd.oasis.opendocument.elektronik tablo) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Katıştılmış belgeler de dahil olmak üzere metni ayıklama |
| ODP (uygulama/vnd.oasis.opendocument.presentation) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`title` |Katıştılmış belgeler de dahil olmak üzere metni ayıklama |
| ZIP (uygulama/zip) |`metadata_content_type` |Arşivdeki tüm belgelerden metin ayıklama |
| GZ (uygulama/gzip) |`metadata_content_type` |Arşivdeki tüm belgelerden metin ayıklama |
| EPUB (uygulama/epub+zip) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_title`<br/>`metadata_description`<br/>`metadata_language`<br/>`metadata_keywords`<br/>`metadata_identifier`<br/>`metadata_publisher` |Arşivdeki tüm belgelerden metin ayıklama |
| XML (uygulama/xml) |`metadata_content_type`<br/>`metadata_content_encoding`<br/> |XML biçimlendirmeyi şeritle ve metni ayıkla |
| JSON (uygulama/json) |`metadata_content_type`<br/>`metadata_content_encoding` |Metin ayıklama<br/>NOT: Bir JSON blob'undan birden çok belge alanı ayıklamanız gerekiyorsa, ayrıntılar için [Dizin oluşturma JSON blobs'a](search-howto-index-json-blobs.md) bakın |
| EML (ileti/rfc822) |`metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_to`<br/>`metadata_message_cc`<br/>`metadata_creation_date`<br/>`metadata_subject` |Ekler de dahil olmak üzere metni ayıklama |
| RTF (uygulama/rtf) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_page_count`<br/>`metadata_word_count`<br/> | Metin ayıklama|
| Düz metin (metin/düz) |`metadata_content_type`<br/>`metadata_content_encoding`<br/> | Metin ayıklama|


## <a name="help-us-make-azure-cognitive-search-better"></a>Azure Bilişsel Arama'yı daha iyi hale getirmemize yardımcı olun
Geliştirmeler için özellik talepleriniz veya fikirleriniz varsa, [UserVoice sitemizdebize](https://feedback.azure.com/forums/263029-azure-search/)bildirin.
