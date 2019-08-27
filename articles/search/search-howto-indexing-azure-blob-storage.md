---
title: Tam metin araması için Azure Blob depolama içeriğini Dizin-Azure Search
description: Azure Blob depolamayı dizinlemeyi ve Azure Search olan belgelerden metin ayıklamayı öğrenin.
ms.date: 05/02/2019
author: mgottein
manager: nitinme
ms.author: magottei
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: ce268f03f53378544b8c329ee69a2bcb8dcc232d
ms.sourcegitcommit: 94ee81a728f1d55d71827ea356ed9847943f7397
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2019
ms.locfileid: "70032127"
---
# <a name="indexing-documents-in-azure-blob-storage-with-azure-search"></a>Azure Blob depolamada Azure Search ile belgelerin dizinini oluşturma
Bu makalede, Azure Blob depolamada depolanan belgelerin (PDF 'Ler, Microsoft Office belgeler ve diğer birçok yaygın biçim gibi) dizin oluşturma için Azure Search nasıl kullanılacağı gösterilmektedir. İlk olarak, blob Dizin oluşturucuyu ayarlama ve yapılandırma hakkında temel bilgileri açıklar. Daha sonra, büyük olasılıkla karşılaştığınız davranış ve senaryolar hakkında daha ayrıntılı bir araştırma sunar.

## <a name="supported-document-formats"></a>Desteklenen belge biçimleri
Blob Indexer aşağıdaki belge biçimlerinden metin ayıklayabilir:

[!INCLUDE [search-blob-data-sources](../../includes/search-blob-data-sources.md)]

## <a name="setting-up-blob-indexing"></a>Blob dizinlemeyi ayarlama
Kullanarak bir Azure Blob depolama Dizin Oluşturucu ayarlayabilirsiniz:

* [Azure portal](https://ms.portal.azure.com)
* Azure Search [REST API](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations)
* Azure Search [.NET SDK](https://aka.ms/search-sdk)

> [!NOTE]
> Bazı Özellikler (örneğin, alan eşlemeleri) portalda henüz kullanılamaz ve programlı olarak kullanılması gerekir.
>

Burada, akışı REST API kullanarak gösteririz.

### <a name="step-1-create-a-data-source"></a>1\. adım: Bir veri kaynağı oluşturun
Veri kaynağı, hangi verilerin dizine erişmesi gerektiğini, verilere erişmek için gereken kimlik bilgilerini ve verilerdeki değişiklikleri verimli bir şekilde belirlemek için ilkeleri (yeni, değiştirilmiş veya silinen satırlar) belirler. Aynı arama hizmetinde birden çok Dizin Oluşturucu tarafından bir veri kaynağı kullanılabilir.

Blob dizin oluşturma için veri kaynağı aşağıdaki gerekli özelliklere sahip olmalıdır:

* **ad** , arama hizmetinizin içindeki veri kaynağının benzersiz adıdır.
* **tür** olmalıdır `azureblob`.
* **kimlik bilgileri** , depolama hesabı bağlantı dizesini `credentials.connectionString` parametre olarak sağlar. Ayrıntılar için aşağıdaki [kimlik bilgilerini belirtme](#Credentials) konusuna bakın.
* **kapsayıcı** , depolama hesabınızda bir kapsayıcı belirtir. Varsayılan olarak, kapsayıcıdaki tüm Bloblar alınabilir. Yalnızca belirli bir sanal dizinde Blobları indekslemek istiyorsanız, bu dizini isteğe bağlı **sorgu** parametresini kullanarak belirtebilirsiniz.

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

Veri kaynağı API 'SI oluşturma hakkında daha fazla bilgi için bkz. [veri kaynağı oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-data-source).

<a name="Credentials"></a>
#### <a name="how-to-specify-credentials"></a>Kimlik bilgilerini belirtme ####

Blob kapsayıcısının kimlik bilgilerini şu yollarla sağlayabilirsiniz:

- **Tam erişimli depolama hesabı bağlantı dizesi**: `DefaultEndpointsProtocol=https;AccountName=<your storage account>;AccountKey=<your account key>`Azure portal > anahtarlar (klasik depolama hesapları için) veya ayarlar > erişim anahtarları (Azure Resource Manager depolama hesapları için) > depolama hesabı dikey penceresine giderek bağlantı dizesini alabilirsiniz.
- **Depolama hesabı paylaşılan erişim imzası** (SAS) bağlantı dizesi: `BlobEndpoint=https://<your account>.blob.core.windows.net/;SharedAccessSignature=?sv=2016-05-31&sig=<the signature>&spr=https&se=<the validity end time>&srt=co&ss=b&sp=rl`SAS, kapsayıcılar ve nesneler (Bu durumda Bloblar) üzerinde liste ve okuma izinlerine sahip olmalıdır.
-  **Kapsayıcı paylaşılan erişim imzası**: `ContainerSharedAccessUri=https://<your storage account>.blob.core.windows.net/<container name>?sv=2016-05-31&sr=c&sig=<the signature>&se=<the validity end time>&sp=rl`SAS, kapsayıcıda liste ve okuma izinlerine sahip olmalıdır.

Depolama paylaşılan erişim imzaları hakkında daha fazla bilgi için bkz. [paylaşılan erişim Imzalarını kullanma](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

> [!NOTE]
> SAS kimlik bilgilerini kullanıyorsanız, zaman aşımı süresini engellemek için veri kaynağı kimlik bilgilerini yenilenen imzalara göre düzenli aralıklarla güncelleştirmeniz gerekecektir. SAS kimlik bilgilerinin kullanım süreleri dolarsa, Dizin Oluşturucu şuna benzer `Credentials provided in the connection string are invalid or have expired.`bir hata iletisiyle başarısız olur.  

### <a name="step-2-create-an-index"></a>2\. adım: Dizin oluşturma
Dizin, bir belge, öznitelik ve arama deneyimini şekillendirip diğer yapıların alanlarını belirtir.

Bloblardan ayıklanan metni depolamak için aranabilir `content` bir alanla dizin oluşturma:   

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

Dizinler oluşturma hakkında daha fazla bilgi için bkz. [Dizin oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-index)

### <a name="step-3-create-an-indexer"></a>3\. adım: Dizin oluşturucu oluşturun
Bir Dizin Oluşturucu bir veri kaynağını hedef arama diziniyle bağlar ve veri yenilemeyi otomatikleştirmek için bir zamanlama sağlar.

Dizin ve veri kaynağı oluşturulduktan sonra, Dizin oluşturucuyu oluşturmaya hazırsınız:

    POST https://[service name].search.windows.net/indexers?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "blob-indexer",
      "dataSourceName" : "blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }

Bu Dizin Oluşturucu her iki saatte bir çalışır (zamanlama aralığı "PT2H" olarak ayarlanır). Her 30 dakikada bir dizin oluşturucu çalıştırmak için, aralığı "PT30M" olarak ayarlayın. Desteklenen en kısa Aralık 5 dakikadır. Zamanlama isteğe bağlıdır-atlanırsa, Dizin Oluşturucu yalnızca bir kez oluşturulduğunda çalışır. Ancak, bir dizin oluşturucuyu dilediğiniz zaman isteğe bağlı olarak çalıştırabilirsiniz.   

Dizin Oluşturucu oluşturma API 'SI hakkında daha fazla bilgi için bkz. [Dizin Oluşturucu oluştur](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

Dizin Oluşturucu zamanlamalarını tanımlama hakkında daha fazla bilgi için bkz. [Azure Search Dizin oluşturucuyu zamanlama](search-howto-schedule-indexers.md).

## <a name="how-azure-search-indexes-blobs"></a>Azure Search Dizin blob 'ları

[Dizin Oluşturucu yapılandırmasına](#PartsOfBlobToIndex)bağlı olarak, blob Indexer yalnızca depolama meta verilerini dizine alabilir (yalnızca meta verileri ilgilenmeniz ve Blobların içeriğini dizinlemek gerekmez), depolama ve içerik meta verileri ya da hem meta veri hem de metin içeriği. Varsayılan olarak, Dizin Oluşturucu hem meta verileri hem de içeriği ayıklar.

> [!NOTE]
> Varsayılan olarak, JSON veya CSV gibi yapılandırılmış içeriğe sahip Bloblar tek bir metin öbeği olarak dizinlenir. JSON ve CSV bloblarını yapılandırılmış bir şekilde indekslemek istiyorsanız, daha fazla bilgi için bkz. [JSON bloblarını](search-howto-index-json-blobs.md) dizine ekleme ve [CSV bloblarını dizin oluşturma](search-howto-index-csv-blobs.md) .
>
> Birleşik veya katıştırılmış bir belge (örneğin, bir posta arşivi veya ek içeren katıştırılmış Outlook e-postasına sahip bir Word belgesi) tek bir belge olarak dizinlenir.

* Belgenin metinsel içeriği adlı `content`bir dize alanında ayıklanır.

> [!NOTE]
> Azure Search, fiyatlandırma katmanına bağlı olarak ne kadar metin çýkarmasını kısıtlar: Ücretsiz katman için 32.000 karakter, temel için 64.000 ve standart, Standart S2 ve Standart S3 katmanları için 4.000.000. Kesilen belgeler için Dizin Oluşturucu durum yanıtında bir uyarı bulunur.  

* Blob üzerinde bulunan Kullanıcı tarafından belirtilen meta veri özellikleri, varsa, tam olarak ayıklanır.
* Standart blob meta veri özellikleri aşağıdaki alanlara ayıklanır:

  * **meta\_veridepolama\_adı** (EDM. String)-Blobun dosya adı. Örneğin, bir blob/My-Container/My-Folder/subfolder/Resume.exe PDF varsa, bu alanın `resume.pdf`değeri.
  * **meta\_veridepolama\_yolu** (EDM. String)-depolama hesabı da dahil olmak üzere Blobun tam URI 'si. Örneğin, `https://myaccount.blob.core.windows.net/my-container/my-folder/subfolder/resume.pdf`
  * **meta\_veridepolama\_içerik\_türü** (EDM. String)-blobu yüklemek için kullandığınız kodla belirtilen içerik türü. Örneğin: `application/octet-stream`.
  * **meta\_veridepolama\_son\_değiştirme** (EDM. DateTimeOffset)-blob için son değiştirme zaman damgası. Azure Search, ilk dizinledikten sonra her şeyin yeniden dizinlenmesini önlemek için bu zaman damgasını değişen Blobları belirlemek için kullanır.
  * **meta\_veridepolama\_boyutu** (EDM. Int64)-BLOB boyutu bayt cinsinden.
  * **meta\_veridepolama\_içerik\_MD5** (EDM. String)-varsa, Blob içeriğinin MD5 karması.
  * **meta\_veridepolama\_SAS\_belirteci** (EDM. String)-bloba erişim sağlamak için [özel yetenekler](cognitive-search-custom-skill-interface.md) tarafından kullanılabilen geçici bir SAS belirteci. Bu belirtecin kullanım süreleri dolduğunda daha sonra kullanılmak üzere depolanmamalıdır.

* Her belge biçimine özgü meta veri özellikleri [burada](#ContentSpecificMetadata)listelenen alanlara ayıklanır.

Arama dizininizdeki yukarıdaki özelliklerin tümü için alanları tanımlamanız gerekmez; uygulamanız için gereken özellikleri yakalayın.

> [!NOTE]
> Genellikle, var olan dizininizdeki alan adları belge ayıklama sırasında oluşturulan alan adlarından farklı olacaktır. Azure Search tarafından belirtilen özellik adlarını arama dizininizdeki alan adlarına eşlemek için **alan eşlemelerini** kullanabilirsiniz. Aşağıda, alan eşlemelerinin kullanımıyla bir örnek görürsünüz.
>
>

<a name="DocumentKeys"></a>
### <a name="defining-document-keys-and-field-mappings"></a>Belge anahtarlarını ve alan eşlemelerini tanımlama
Azure Search, belge anahtarı bir belgeyi benzersiz şekilde tanımlar. Her arama dizininde Edm. String türünde tam olarak bir anahtar alanı olmalıdır. Dizine eklenmekte olan her belge için anahtar alanı gereklidir (aslında yalnızca gerekli olan alandır).  

Hangi ayıklanan alanın, dizininiz için anahtar alanla eşleşmesi gerektiğini dikkatle düşünmeniz gerekir. Adaylar şunlardır:

* **meta\_veri\_depolama alanı adı** -bu kullanışlı bir aday olabilir, ancak 1) adların benzersiz olamayacağını unutmayın. farklı klasörlerde aynı ada sahip bloblarınız olabilir ve 2) ad geçersiz karakterler içeriyor olabilir çizgiler gibi belge anahtarlarında. `base64Encode` [Alan eşleme işlevini](search-indexer-field-mappings.md#base64EncodeFunction) kullanarak geçersiz karakterlerle işlem yapabilirsiniz-bunu yaparsanız, arama gibi API çağrılarına geçirirken belge anahtarlarını kodlamayı unutmayın. (Örneğin, .NET 'te bu amaçla [UrlTokenEncode yöntemini](https://msdn.microsoft.com/library/system.web.httpserverutility.urltokenencode.aspx) kullanabilirsiniz).
* **meta\_veridepolama\_yolu** -tam yolun kullanılması benzersizlik sağlar, ancak yol kesinlikle [bir belge anahtarında geçersiz](https://docs.microsoft.com/rest/api/searchservice/naming-rules)olan karakterler içeriyor. `/`  Yukarıdaki gibi, `base64Encode` [işlevini](search-indexer-field-mappings.md#base64EncodeFunction)kullanarak anahtarları kodlama seçeneğiniz vardır.
* Yukarıdaki seçeneklerden hiçbiri sizin için işe çalışmadıysanız, bloblara özel meta veri özelliği ekleyebilirsiniz. Ancak, bu seçenek, bu meta veri özelliğini tüm bloblara eklemek için blob karşıya yükleme işleminizi gerektirir. Anahtar gerekli bir özellik olduğundan, bu özelliğe sahip olmayan tüm Blobların dizini oluşturulamaz.

> [!IMPORTANT]
> Dizinde anahtar alanı için açık eşleme yoksa, Azure Search otomatik olarak anahtar olarak kullanır `metadata_storage_path` ve temel 64 anahtar değerlerini kodluyor (yukarıdaki ikinci seçenek).
>
>

Bu örnekte, `metadata_storage_name` alanı belge anahtarı olarak seçelim. Ayrıca, dizininizin adlı `key` bir anahtar alanı ve belge boyutunu depolamak için bir alan `fileSize` olduğunu varsayalım. İstediğiniz gibi durumlarda, Dizin oluşturucuyu oluştururken veya güncelleştirirken aşağıdaki alan eşlemelerini belirtin:

    "fieldMappings" : [
      { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key", "mappingFunction" : { "name" : "base64Encode" } },
      { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
    ]

Bunu bir araya getirmek için, alan eşlemelerini nasıl ekleyebileceğiniz ve mevcut bir dizin oluşturucunun anahtarlarının Base-64 kodlamasını nasıl etkinleştirebileceğiniz aşağıda verilmiştir:

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
> Alan eşlemeleri hakkında daha fazla bilgi edinmek için [Bu makaleye](search-indexer-field-mappings.md)bakın.
>
>

<a name="WhichBlobsAreIndexed"></a>
## <a name="controlling-which-blobs-are-indexed"></a>Hangi Blobların dizine alınacağını denetleme
Hangi Blobların dizine alınacağını ve hangilerinin atlandığını denetleyebilirsiniz.

### <a name="index-only-the-blobs-with-specific-file-extensions"></a>Yalnızca belirli dosya uzantılarına sahip Blobları dizine oluştur
`indexedFileNameExtensions` Dizin Oluşturucu yapılandırma parametresini kullanarak yalnızca belirttiğiniz dosya adı uzantılarına sahip Blobları dizinleyerek dizin oluşturabilirsiniz. Değer, dosya uzantılarının virgülle ayrılmış bir listesini içeren bir dizedir (önde gelen noktayla). Örneğin, yalnızca ' ı dizine eklemek için. PDF ve. DOCX blob 'ları, bunu yapın:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "indexedFileNameExtensions" : ".pdf,.docx" } }
    }

### <a name="exclude-blobs-with-specific-file-extensions"></a>Belirli dosya uzantılarına sahip Blobları Dışla
`excludedFileNameExtensions` Yapılandırma parametresini kullanarak, dizin oluşturma işleminden belirli dosya adı uzantılarına sahip Blobları dışlayabilirsiniz. Değer, dosya uzantılarının virgülle ayrılmış bir listesini içeren bir dizedir (önde gelen noktayla). Örneğin, tüm Blobları hariç tüm Blobları dizine eklemek için. PNG ve. JPEG uzantıları, bunu yapın:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "excludedFileNameExtensions" : ".png,.jpeg" } }
    }

Hem hem `indexedFileNameExtensions` de `excludedFileNameExtensions` parametreleri varsa Azure Search önce `indexedFileNameExtensions`' e, sonra da ' a `excludedFileNameExtensions`bakar. Bu, aynı dosya uzantısının her iki listede de mevcutsa, dizin oluşturma işleminin dışlanacağını belirtir.

<a name="PartsOfBlobToIndex"></a>
## <a name="controlling-which-parts-of-the-blob-are-indexed"></a>Blob 'un hangi bölümlerinin dizine alınacağını denetleme

`dataToExtract` Yapılandırma parametresi kullanılarak Blobların hangi bölümlerinin dizine alınacağını kontrol edebilirsiniz. Bu, aşağıdaki değerleri alabilir:

* `storageMetadata`-yalnızca [Standart blob özelliklerinin ve Kullanıcı tarafından belirtilen meta verilerin](../storage/blobs/storage-properties-metadata.md) dizine alınacağını belirtir.
* `allMetadata`-blob içeriğinden ayıklanan depolama meta verilerinin ve [içerik türü belirli meta verilerin](#ContentSpecificMetadata) dizine alınacağını belirtir.
* `contentAndMetadata`-bloba ayıklanan tüm meta veri ve metin içeriğinin dizine alınacağını belirtir. Varsayılan değer budur.

Örneğin, yalnızca depolama meta verilerini indekslemek için şunu kullanın:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "dataToExtract" : "storageMetadata" } }
    }

### <a name="using-blob-metadata-to-control-how-blobs-are-indexed"></a>Blobların nasıl dizine alınacağını denetlemek için blob meta verilerini kullanma

Yukarıda açıklanan yapılandırma parametreleri tüm Bloblar için geçerlidir. Bazen, *ayrı Blobların* nasıl dizine alınacağını denetlemek isteyebilirsiniz. Bunu, aşağıdaki blob meta veri özelliklerini ve değerlerini ekleyerek yapabilirsiniz:

| Özellik adı | Özellik değeri | Açıklama |
| --- | --- | --- |
| AzureSearch_Skip |"true" |Blob Dizin oluşturucuyu blobu tamamen atlayacak şekilde yönlendirir. Meta veri veya içerik ayıklama denenmez. Bu, belirli bir blob sürekli olarak başarısız olduğunda ve dizin oluşturma işlemini kesintiye uğradığında yararlı olur. |
| AzureSearch_SkipContent |"true" |Bu, [yukarıda](#PartsOfBlobToIndex) belirtilen `"dataToExtract" : "allMetadata"` bir Blobun kapsamına alınan ayarların eşdeğeridir. |

<a name="DealingWithErrors"></a>
## <a name="dealing-with-errors"></a>Hatalarla ilgilenme

Varsayılan olarak, blob Indexer, desteklenmeyen içerik türü (örneğin, bir görüntü) ile bir blob ile karşılaştığında yanıt vermez. Tabii ki, `excludedFileNameExtensions` belirli içerik türlerini atlamak için parametresini kullanabilirsiniz. Ancak, tüm olası içerik türlerini önceden bilmeden blob 'ları dizinleyebilirsiniz. Desteklenmeyen bir içerik türüyle karşılaşıldığında dizine almaya devam etmek için `failOnUnsupportedContentType` yapılandırma parametresini şu şekilde `false`ayarlayın:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "failOnUnsupportedContentType" : false } }
    }

Bazı Bloblar için Azure Search, içerik türünü belirleyemiyor veya başka türlü desteklenen içerik türündeki bir belgeyi işleyemedi. Bu hata modunu yoksaymak için `failOnUnprocessableDocument` yapılandırma parametresini false olarak ayarlayın:

      "parameters" : { "configuration" : { "failOnUnprocessableDocument" : false } }

Azure Search dizine eklenen Blobların boyutunu sınırlar. Bu sınırlar [Azure Search Içindeki hizmet sınırları](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity)bölümünde belgelenmiştir. Büyük bir blob, varsayılan olarak hata olarak değerlendirilir. Ancak yapılandırma parametresini doğru olarak ayarlarsanız `indexStorageMetadataOnlyForOversizedDocuments` , hala büyük Blobların depolama meta verilerinin dizinini oluşturabilirsiniz: 

    "parameters" : { "configuration" : { "indexStorageMetadataOnlyForOversizedDocuments" : true } }

Blob 'ları ayrıştırırken veya bir dizine belge eklerken, herhangi bir işlem noktasında hatalar meydana geliyorsa dizin oluşturmaya da devam edebilirsiniz. Belirli sayıda hatayı yoksaymak için, `maxFailedItems` ve `maxFailedItemsPerBatch` yapılandırma parametrelerini istenen değerlere ayarlayın. Örneğin:

    {
      ... other parts of indexer definition
      "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 10 }
    }

## <a name="incremental-indexing-and-deletion-detection"></a>Artımlı dizin oluşturma ve silme algılaması
Bir blob Dizin Oluşturucuyu bir zamanlamaya göre çalışacak şekilde ayarlarken, blob 'un zaman damgasıyla belirlendiği şekilde yalnızca değiştirilen Blobları `LastModified` yeniden dizinleyebilirsiniz.

> [!NOTE]
> Değişiklik algılama ilkesi belirtmeniz gerekmez – artımlı dizin oluşturma sizin için otomatik olarak etkinleştirilir.

Belge silmeyi desteklemek için "geçici silme" yaklaşımını kullanın. Blob 'ları sağ silme, ilgili belgeler arama dizininden kaldırılmaz. Bunun yerine, aşağıdaki adımları kullanın:  

1. Mantıksal olarak silindiğini Azure Search belirtmek için bloba özel meta veri özelliği ekleyin
2. Veri kaynağında geçici silme algılama ilkesi yapılandırma
3. Dizin Oluşturucu blob 'u işledikten sonra (Dizin Oluşturucu durum API 'SI tarafından gösterildiği gibi), blobu fiziksel olarak silebilirsiniz

Örneğin, aşağıdaki ilke, bir blob 'un şu değere `IsDeleted` `true`sahip bir meta veri özelliği varsa silineceğini kabul eder:

    PUT https://[service name].search.windows.net/datasources/blob-datasource?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : "my-folder" },
        "dataDeletionDetectionPolicy" : {
            "@odata.type" :"#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",     
            "softDeleteColumnName" : "IsDeleted",
            "softDeleteMarkerValue" : "true"
        }
    }   

## <a name="indexing-large-datasets"></a>Büyük veri kümelerini dizinleme

Dizin oluşturma Blobları zaman alan bir işlem olabilir. Dizin oluşturmak için milyonlarca blob 'un bulunduğu durumlarda, verileri bölümleyerek ve paralel olarak verileri işlemek için birden çok Dizin Oluşturucu kullanarak dizin oluşturmayı hızlandırabilirsiniz. Bunu nasıl ayarlayabilmeniz gerekir:

- Verilerinizi birden çok blob kapsayıcılarına veya sanal klasöre bölme
- Her kapsayıcı veya klasör için bir tane olmak üzere çeşitli Azure Search veri kaynakları ayarlayın. Blob klasörünü işaret etmek için şu `query` parametreyi kullanın:

    ```
    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : "my-folder" }
    }
    ```

- Her veri kaynağı için karşılık gelen bir Dizin Oluşturucu oluşturun. Tüm Dizin oluşturucular aynı hedef arama dizinine işaret edebilir.  

- Hizmetinizdeki bir arama birimi, belirli bir zamanda bir Dizin Oluşturucu çalıştırabilir. Yukarıda açıklandığı gibi birden çok Dizin Oluşturucu oluşturmak yalnızca aslında paralel olarak çalıştırıldıklarında yararlıdır. Birden çok dizin oluşturucuyu paralel olarak çalıştırmak için, uygun sayıda bölüm ve çoğaltma oluşturarak arama hizmetinizi ölçeklendirin. Örneğin, arama hizmetinizin 6 arama birimi varsa (örneğin, 2 bölüm x 3 çoğaltmalar), 6 Dizin oluşturucular eşzamanlı olarak çalışabilir ve dizin oluşturma sırasında altı kat artışına neden olur. Ölçekleme ve kapasite planlaması hakkında daha fazla bilgi için bkz. [Azure Search 'de sorgu ve dizin oluşturma iş yükleri Için ölçek kaynak düzeyleri](search-capacity-planning.md).

## <a name="indexing-documents-along-with-related-data"></a>İlgili verilerle birlikte belgelerin dizinini oluşturma

Dizininizdeki birden fazla kaynaktaki belgeleri "birleştirmek" isteyebilirsiniz. Örneğin, Cosmos DB içinde depolanan diğer meta verilerle bloblardan metin birleştirmek isteyebilirsiniz. Birden çok bölümden arama belgeleri oluşturmak için çeşitli dizin oluşturucularla birlikte gönderme dizin oluşturma API 'sini de kullanabilirsiniz. 

Bunun çalışması için, tüm dizin oluşturucularının ve diğer bileşenlerin belge anahtarını kabul etmesi gerekir. Ayrıntılı bir izlenecek yol için bu dış makaleye bakın: [Belgeleri Azure Search diğer verilerle birleştirin](https://blog.lytzen.name/2017/01/combine-documents-with-other-data-in.html).

<a name="IndexingPlainText"></a>
## <a name="indexing-plain-text"></a>Düz metin dizini oluşturma 

Tüm bloblarınız aynı kodlamada düz metin içeriyorsa, **metin ayrıştırma modunu**kullanarak dizin oluşturma performansını önemli ölçüde artırabilirsiniz. Metin ayrıştırma modunu kullanmak için `parsingMode` yapılandırma özelliğini şu şekilde `text`ayarlayın:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "parsingMode" : "text" } }
    }

Varsayılan olarak, `UTF-8` kodlama varsayılır. Farklı bir kodlama belirtmek için `encoding` yapılandırma özelliğini kullanın: 

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "parsingMode" : "text", "encoding" : "windows-1252" } }
    }


<a name="ContentSpecificMetadata"></a>
## <a name="content-type-specific-metadata-properties"></a>İçerik türüne özgü meta veri özellikleri
Aşağıdaki tabloda her belge biçimi için yapılan işlem özetlenmektedir ve Azure Search tarafından ayıklanan meta veri özellikleri açıklanmaktadır.

| Belge biçimi/içerik türü | İçerik türü belirli meta veri özellikleri | İşleme ayrıntıları |
| --- | --- | --- |
| HTML (`text/html`) |`metadata_content_encoding`<br/>`metadata_content_type`<br/>`metadata_language`<br/>`metadata_description`<br/>`metadata_keywords`<br/>`metadata_title` |HTML biçimlendirmesini şerit ve metin Ayıkla |
| PDF (`application/pdf`) |`metadata_content_type`<br/>`metadata_language`<br/>`metadata_author`<br/>`metadata_title` |Gömülü belgeler dahil olmak üzere metni Ayıkla (görüntüler hariç) |
| DOCX (application/vnd. openxmlformats-officedocument. WordprocessingML. Document) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Gömülü belgeler dahil olmak üzere metni Ayıkla |
| DOC (Application/msword) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Gömülü belgeler dahil olmak üzere metni Ayıkla |
| XLSX (application/vnd. openxmlformats-officedocument. elektronik sayfaml. Sheet) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Gömülü belgeler dahil olmak üzere metni Ayıkla |
| XLS (application/vnd. MS-Excel) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Gömülü belgeler dahil olmak üzere metni Ayıkla |
| PPTX (application/vnd. openxmlformats-officedocument. presentationml. Presentation) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Gömülü belgeler dahil olmak üzere metni Ayıkla |
| PPT (application/vnd. MS-PowerPoint) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Gömülü belgeler dahil olmak üzere metni Ayıkla |
| MSG (application/vnd. MS-Outlook) |`metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_from_email`<br/>`metadata_message_to`<br/>`metadata_message_to_email`<br/>`metadata_message_cc`<br/>`metadata_message_cc_email`<br/>`metadata_message_bcc`<br/>`metadata_message_bcc_email`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_subject` |Ekleri dahil metin Ayıkla |
| ZIP (uygulama/zip) |`metadata_content_type` |Arşivdeki tüm belgelerden metin Ayıkla |
| XML (Application/XML) |`metadata_content_type`<br/>`metadata_content_encoding`<br/> |XML işaretlemesini şerit ve metin Ayıkla |
| JSON (uygulama/JSON) |`metadata_content_type`<br/>`metadata_content_encoding` |Metin ayıklama<br/>NOT: JSON blobundan birden çok belge alanı ayıklamanız gerekiyorsa, Ayrıntılar için bkz. [JSON Bloblarını dizine ekleme](search-howto-index-json-blobs.md) |
| EML (ileti/rfc822) |`metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_to`<br/>`metadata_message_cc`<br/>`metadata_creation_date`<br/>`metadata_subject` |Ekleri dahil metin Ayıkla |
| RTF (uygulama/RTF) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_page_count`<br/>`metadata_word_count`<br/> | Metin ayıklama|
| Düz metin (metin/düz) |`metadata_content_type`<br/>`metadata_content_encoding`<br/> | Metin ayıklama|


## <a name="help-us-make-azure-search-better"></a>Azure Search daha iyi hale getirmemize yardımcı olun
Geliştirmeler için özellik istekleriniz veya fikirler varsa [UserVoice sitemizi](https://feedback.azure.com/forums/263029-azure-search/)bize tanıyın.
