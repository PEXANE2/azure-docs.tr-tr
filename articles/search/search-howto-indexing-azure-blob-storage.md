---
title: Azure Blob depolama içeriğini arama
titleSuffix: Azure Cognitive Search
description: Azure Blob depolama alanını dizinlemeyi ve Azure Bilişsel Arama ile belgelerden metin ayıklamayı öğrenin.
manager: nitinme
author: mgottein
ms.author: magottei
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.custom: fasttrack-edit
ms.openlocfilehash: 3ed3ff94b764c0fcb5521ef8106b32923b203a01
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86260644"
---
# <a name="how-to-index-documents-in-azure-blob-storage-with-azure-cognitive-search"></a>Azure Bilişsel Arama Azure Blob depolamada belge dizin oluşturma

Bu makalede, Azure Blob depolamada depolanan belgelerin (PDF 'Ler, Microsoft Office belgeler ve diğer birçok yaygın biçim gibi) dizin oluşturma için Azure Bilişsel Arama nasıl kullanılacağı gösterilmektedir. İlk olarak, blob Dizin oluşturucuyu ayarlama ve yapılandırma hakkında temel bilgileri açıklar. Daha sonra, büyük olasılıkla karşılaştığınız davranış ve senaryolar hakkında daha ayrıntılı bir araştırma sunar.

<a name="SupportedFormats"></a>

## <a name="supported-document-formats"></a>Desteklenen belge biçimleri
Blob Indexer aşağıdaki belge biçimlerinden metin ayıklayabilir:

[!INCLUDE [search-blob-data-sources](../../includes/search-blob-data-sources.md)]

## <a name="setting-up-blob-indexing"></a>Blob dizinlemeyi ayarlama
Kullanarak bir Azure Blob depolama Dizin Oluşturucu ayarlayabilirsiniz:

* [Azure portalındaki](https://ms.portal.azure.com)
* Azure Bilişsel Arama [REST API](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations)
* Azure Bilişsel Arama [.NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/search)

> [!NOTE]
> Bazı Özellikler (örneğin, alan eşlemeleri) portalda henüz kullanılamaz ve programlı olarak kullanılması gerekir.
>

Burada, akışı REST API kullanarak gösteririz.

### <a name="step-1-create-a-data-source"></a>1. Adım: Veri kaynağı oluşturma
Veri kaynağı, hangi verilerin dizine erişmesi gerektiğini, verilere erişmek için gereken kimlik bilgilerini ve verilerdeki değişiklikleri verimli bir şekilde belirlemek için ilkeleri (yeni, değiştirilmiş veya silinen satırlar) belirler. Aynı arama hizmetinde birden çok Dizin Oluşturucu tarafından bir veri kaynağı kullanılabilir.

Blob dizin oluşturma için veri kaynağı aşağıdaki gerekli özelliklere sahip olmalıdır:

* **ad** , arama hizmetinizin içindeki veri kaynağının benzersiz adıdır.
* **tür** olmalıdır `azureblob` .
* **kimlik bilgileri** , depolama hesabı bağlantı dizesini parametre olarak sağlar `credentials.connectionString` . Ayrıntılar için aşağıdaki [kimlik bilgilerini belirtme](#Credentials) konusuna bakın.
* **kapsayıcı** , depolama hesabınızda bir kapsayıcı belirtir. Varsayılan olarak, kapsayıcıdaki tüm Bloblar alınabilir. Yalnızca belirli bir sanal dizinde Blobları indekslemek istiyorsanız, bu dizini isteğe bağlı **sorgu** parametresini kullanarak belirtebilirsiniz.

Bir veri kaynağı oluşturmak için:

```http
    POST https://[service name].search.windows.net/datasources?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "<optional-virtual-directory-name>" }
    }   
```

Veri kaynağı API 'SI oluşturma hakkında daha fazla bilgi için bkz. [veri kaynağı oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-data-source).

<a name="Credentials"></a>
#### <a name="how-to-specify-credentials"></a>Kimlik bilgilerini belirtme ####

Blob kapsayıcısının kimlik bilgilerini şu yollarla sağlayabilirsiniz:

- **Tam erişimli depolama hesabı bağlantı dizesi**: `DefaultEndpointsProtocol=https;AccountName=<your storage account>;AccountKey=<your account key>` depolama hesabı dikey > > penceresine (klasik depolama hesapları Için) veya ayarları > erişim anahtarlarına (Azure Resource Manager depolama hesapları için) giderek Azure Portal bağlantı dizesini alabilirsiniz.
- **Depolama hesabı paylaşılan erişim imzası** (SAS) bağlantı DIZESI: `BlobEndpoint=https://<your account>.blob.core.windows.net/;SharedAccessSignature=?sv=2016-05-31&sig=<the signature>&spr=https&se=<the validity end time>&srt=co&ss=b&sp=rl` SAS, kapsayıcılar ve nesneler (Bu durumda blob 'lar) üzerinde liste ve okuma izinlerine sahip olmalıdır.
-  **Kapsayıcı paylaşılan erişim imzası**: `ContainerSharedAccessUri=https://<your storage account>.blob.core.windows.net/<container name>?sv=2016-05-31&sr=c&sig=<the signature>&se=<the validity end time>&sp=rl` SAS, kapsayıcıda liste ve okuma izinlerine sahip olmalıdır.

Depolama paylaşılan erişim imzaları hakkında daha fazla bilgi için bkz. [paylaşılan erişim Imzalarını kullanma](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

> [!NOTE]
> SAS kimlik bilgilerini kullanıyorsanız, zaman aşımı süresini engellemek için veri kaynağı kimlik bilgilerini yenilenen imzalara göre düzenli aralıklarla güncelleştirmeniz gerekecektir. SAS kimlik bilgilerinin kullanım süreleri dolarsa, Dizin Oluşturucu şuna benzer bir hata iletisiyle başarısız olur `Credentials provided in the connection string are invalid or have expired.` .  

### <a name="step-2-create-an-index"></a>2. Adım: Dizin oluşturma
Dizin, bir belge, öznitelik ve arama deneyimini şekillendirip diğer yapıların alanlarını belirtir.

`content`Bloblardan ayıklanan metni depolamak için aranabilir bir alanla dizin oluşturma:   

```http
    POST https://[service name].search.windows.net/indexes?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
          "name" : "my-target-index",
          "fields": [
            { "name": "id", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "content", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false }
          ]
    }
```

Dizinler oluşturma hakkında daha fazla bilgi için bkz. [Dizin oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-index)

### <a name="step-3-create-an-indexer"></a>3. Adım: Dizin Oluşturucu oluşturma
Bir Dizin Oluşturucu bir veri kaynağını hedef arama diziniyle bağlar ve veri yenilemeyi otomatikleştirmek için bir zamanlama sağlar.

Dizin ve veri kaynağı oluşturulduktan sonra, Dizin oluşturucuyu oluşturmaya hazırsınız:

```http
    POST https://[service name].search.windows.net/indexers?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "blob-indexer",
      "dataSourceName" : "blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }
```

Bu Dizin Oluşturucu her iki saatte bir çalışır (zamanlama aralığı "PT2H" olarak ayarlanır). Her 30 dakikada bir dizin oluşturucu çalıştırmak için, aralığı "PT30M" olarak ayarlayın. Desteklenen en kısa Aralık 5 dakikadır. Zamanlama isteğe bağlıdır-atlanırsa, Dizin Oluşturucu yalnızca bir kez oluşturulduğunda çalışır. Ancak, bir dizin oluşturucuyu dilediğiniz zaman isteğe bağlı olarak çalıştırabilirsiniz.   

Dizin Oluşturucu oluşturma API 'SI hakkında daha fazla bilgi için bkz. [Dizin Oluşturucu oluştur](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

Dizin Oluşturucu zamanlamalarını tanımlama hakkında daha fazla bilgi için bkz. [Azure bilişsel arama için Dizin Oluşturucu zamanlama](search-howto-schedule-indexers.md).

<a name="how-azure-search-indexes-blobs"></a>

## <a name="how-azure-cognitive-search-indexes-blobs"></a>Azure Bilişsel Arama blob 'ları dizin oluşturma

[Dizin Oluşturucu yapılandırmasına](#PartsOfBlobToIndex)bağlı olarak, blob Indexer yalnızca depolama meta verilerini dizine alabilir (yalnızca meta verileri ilgilenmeniz ve Blobların içeriğini dizinlemek gerekmez), depolama ve içerik meta verileri ya da hem meta veri hem de metin içeriği. Varsayılan olarak, Dizin Oluşturucu hem meta verileri hem de içeriği ayıklar.

> [!NOTE]
> Varsayılan olarak, JSON veya CSV gibi yapılandırılmış içeriğe sahip Bloblar tek bir metin öbeği olarak dizinlenir. JSON ve CSV bloblarını yapılandırılmış bir şekilde indekslemek istiyorsanız, daha fazla bilgi için bkz. [JSON bloblarını](search-howto-index-json-blobs.md) dizine ekleme ve [CSV bloblarını dizin oluşturma](search-howto-index-csv-blobs.md) .
>
> Birleşik veya katıştırılmış bir belge (örneğin, posta arşivi, ekleri içeren katıştırılmış Outlook e-postasına sahip bir Word belgesi) veya. Eki olan MSG dosyası) aynı zamanda tek bir belge olarak dizinlenir. Örneğin, bir. eknden ayıklanan tüm görüntüler. MSG dosyası normalized_images alanına döndürülür.

* Belgenin metinsel içeriği adlı bir dize alanında ayıklanır `content` .

> [!NOTE]
> Azure Bilişsel Arama, fiyatlandırma katmanına bağlı olarak ne kadar metin ayıklar: ücretsiz katman için 32.000 karakter, temel için 64.000, standart için 4.000.000, Standart S2 için 8.000.000 ve Standart S3 için 16.000.000. Kesilen belgeler için Dizin Oluşturucu durum yanıtında bir uyarı bulunur.  

* Blob üzerinde bulunan Kullanıcı tarafından belirtilen meta veri özellikleri, varsa, tam olarak ayıklanır. Bunun, bir alanın, Blobun meta veri anahtarıyla aynı ada sahip dizinde tanımlanmasını gerektirdiğini unutmayın. Örneğin, Blobun değeri ile bir meta veri anahtarı varsa `Sensitivity` `High` , arama dizininizdeki adlı bir alan tanımlamalısınız `Sensitivity` ve değer ile doldurulur `High` .
* Standart blob meta veri özellikleri aşağıdaki alanlara ayıklanır:

  * **meta veri \_ depolama \_ adı** (EDM. String)-Blobun dosya adı. Örneğin, bir blob/My-Container/My-Folder/subfolder/resume.pdf varsa, bu alanın değeri `resume.pdf` .
  * **meta veri \_ depolama \_ yolu** (EDM. String)-depolama hesabı da dahil olmak üzere Blobun tam URI 'si. Örneğin, `https://myaccount.blob.core.windows.net/my-container/my-folder/subfolder/resume.pdf`
  * **meta veri \_ depolama \_ içerik \_ türü** (EDM. String)-blobu yüklemek için kullandığınız kodla belirtilen içerik türü. Örneğin, `application/octet-stream`.
  * **meta veri \_ depolama \_ son \_ değiştirme** (EDM. DateTimeOffset)-blob için son değiştirme zaman damgası. Azure Bilişsel Arama, ilk dizinledikten sonra her şeyin yeniden dizinlenmesini önlemek için değişen Blobları tanımlamak üzere bu zaman damgasını kullanır.
  * **meta veri \_ depolama \_ boyutu** (EDM. Int64)-BLOB boyutu bayt cinsinden.
  * **meta veri \_ depolama \_ içerik \_ MD5** (EDM. String)-varsa, Blob içeriğinin MD5 karması.
  * **meta veri \_ depolama \_ SAS \_ belirteci** (EDM. String)-bloba erişim sağlamak için [özel yetenekler](cognitive-search-custom-skill-interface.md) tarafından kullanılabilen geçici bir SAS belirteci. Bu belirtecin kullanım süreleri dolduğunda daha sonra kullanılmak üzere depolanmamalıdır.

* Her belge biçimine özgü meta veri özellikleri [burada](#ContentSpecificMetadata)listelenen alanlara ayıklanır.

Arama dizininizdeki yukarıdaki özelliklerin tümü için alanları tanımlamanız gerekmez; uygulamanız için gereken özellikleri yakalayın.

> [!NOTE]
> Genellikle, var olan dizininizdeki alan adları belge ayıklama sırasında oluşturulan alan adlarından farklı olacaktır. Azure Bilişsel Arama tarafından sunulan özellik adlarını arama dizininizdeki alan adlarına eşlemek için **alan eşlemelerini** kullanabilirsiniz. Aşağıda, alan eşlemelerinin kullanımıyla bir örnek görürsünüz.
>
>

<a name="DocumentKeys"></a>
### <a name="defining-document-keys-and-field-mappings"></a>Belge anahtarlarını ve alan eşlemelerini tanımlama
Azure Bilişsel Arama 'de belge anahtarı bir belgeyi benzersiz şekilde tanımlar. Her arama dizininde Edm. String türünde tam olarak bir anahtar alanı olmalıdır. Dizine eklenmekte olan her belge için anahtar alanı gereklidir (aslında yalnızca gerekli olan alandır).  

Hangi ayıklanan alanın, dizininiz için anahtar alanla eşleşmesi gerektiğini dikkatle düşünmeniz gerekir. Adaylar şunlardır:

* **meta veri \_ depolama \_ adı** -bu kullanışlı bir aday olabilir, ancak 1) adların benzersiz olamayacağını, farklı klasörlerde aynı ada sahip bloblarınız olabilir ve 2) ad, tireler gibi belge anahtarlarında geçersiz karakterler içeriyor olabilir. `base64Encode` [Alan eşleme işlevini](search-indexer-field-mappings.md#base64EncodeFunction) kullanarak geçersiz karakterlerle işlem yapabilirsiniz-bunu yaparsanız, arama gibi API çağrılarına geçirirken belge anahtarlarını kodlamayı unutmayın. (Örneğin, .NET 'te bu amaçla [UrlTokenEncode yöntemini](https://msdn.microsoft.com/library/system.web.httpserverutility.urltokenencode.aspx) kullanabilirsiniz).
* **meta veri \_ depolama \_ yolu** -tam yolun kullanılması benzersizlik sağlar, ancak yol kesinlikle `/` [bir belge anahtarında geçersiz](https://docs.microsoft.com/rest/api/searchservice/naming-rules)olan karakterler içeriyor.  Yukarıdaki gibi, işlevini kullanarak anahtarları kodlama seçeneğiniz vardır `base64Encode` [function](search-indexer-field-mappings.md#base64EncodeFunction).
* Yukarıdaki seçeneklerden hiçbiri sizin için işe çalışmadıysanız, bloblara özel meta veri özelliği ekleyebilirsiniz. Ancak, bu seçenek, bu meta veri özelliğini tüm bloblara eklemek için blob karşıya yükleme işleminizi gerektirir. Anahtar gerekli bir özellik olduğundan, bu özelliğe sahip olmayan tüm Blobların dizini oluşturulamaz.

> [!IMPORTANT]
> Dizinde anahtar alanı için açık eşleme yoksa, Azure Bilişsel Arama otomatik olarak `metadata_storage_path` anahtar olarak kullanılır ve temel 64 anahtar değerlerini kodluyor (yukarıdaki ikinci seçenek).
>
>

Bu örnekte, `metadata_storage_name` alanı belge anahtarı olarak seçelim. Ayrıca, dizininizin adlı bir anahtar alanı `key` ve `fileSize` belge boyutunu depolamak için bir alan olduğunu varsayalım. İstediğiniz gibi durumlarda, Dizin oluşturucuyu oluştururken veya güncelleştirirken aşağıdaki alan eşlemelerini belirtin:

```http
    "fieldMappings" : [
      { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key", "mappingFunction" : { "name" : "base64Encode" } },
      { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
    ]
```

Bunu bir araya getirmek için, alan eşlemelerini nasıl ekleyebileceğiniz ve mevcut bir dizin oluşturucunun anahtarlarının Base-64 kodlamasını nasıl etkinleştirebileceğiniz aşağıda verilmiştir:

```http
    PUT https://[service name].search.windows.net/indexers/blob-indexer?api-version=2020-06-30
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
```

> [!NOTE]
> Alan eşlemeleri hakkında daha fazla bilgi edinmek için [Bu makaleye](search-indexer-field-mappings.md)bakın.
>
>

#### <a name="what-if-you-need-to-encode-a-field-to-use-it-as-a-key-but-you-also-want-to-search-it"></a>Bir alanı anahtar olarak kullanmak için kodlamanız gerekirse, ancak bunu da aramak istiyorsanız ne olur?

Anahtar olarak metadata_storage_path gibi bir alanın kodlanmış bir sürümünü kullanmanız gerektiğinde, ancak aynı zamanda bu alanın aranabilir olması gerekir (kodlama olmadan). Bu sorunu çözmek için iki alana eşleyebilirsiniz; anahtar için kullanılacak bir tane ve arama amacıyla kullanılacak başka bir tane. Aşağıdaki örnekte, *anahtar* alanı kodlanmış yolu içerir, ancak *yol* alanı kodlanmaz ve dizinde aranabilir alan olarak kullanılacaktır.

```http
    PUT https://[service name].search.windows.net/indexers/blob-indexer?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
      "dataSourceName" : " blob-datasource ",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "fieldMappings" : [
        { "sourceFieldName" : "metadata_storage_path", "targetFieldName" : "key", "mappingFunction" : { "name" : "base64Encode" } },
        { "sourceFieldName" : "metadata_storage_path", "targetFieldName" : "path" }
      ]
    }
```
<a name="WhichBlobsAreIndexed"></a>
## <a name="controlling-which-blobs-are-indexed"></a>Hangi Blobların dizine alınacağını denetleme
Hangi Blobların dizine alınacağını ve hangilerinin atlandığını denetleyebilirsiniz.

### <a name="index-only-the-blobs-with-specific-file-extensions"></a>Yalnızca belirli dosya uzantılarına sahip Blobları dizine oluştur
Dizin Oluşturucu yapılandırma parametresini kullanarak yalnızca belirttiğiniz dosya adı uzantılarına sahip Blobları dizinleyerek dizin oluşturabilirsiniz `indexedFileNameExtensions` . Değer, dosya uzantılarının virgülle ayrılmış bir listesini içeren bir dizedir (önde gelen noktayla). Örneğin, yalnızca ' ı dizine eklemek için. PDF ve. DOCX blob 'ları, bunu yapın:

```http
    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "indexedFileNameExtensions" : ".pdf,.docx" } }
    }
```

### <a name="exclude-blobs-with-specific-file-extensions"></a>Belirli dosya uzantılarına sahip Blobları Dışla
Yapılandırma parametresini kullanarak, dizin oluşturma işleminden belirli dosya adı uzantılarına sahip Blobları dışlayabilirsiniz `excludedFileNameExtensions` . Değer, dosya uzantılarının virgülle ayrılmış bir listesini içeren bir dizedir (önde gelen noktayla). Örneğin, tüm Blobları hariç tüm Blobları dizine eklemek için. PNG ve. JPEG uzantıları, bunu yapın:

```http
    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "excludedFileNameExtensions" : ".png,.jpeg" } }
    }
```

Hem hem `indexedFileNameExtensions` de `excludedFileNameExtensions` parametreleri varsa, Azure bilişsel arama önce `indexedFileNameExtensions` ' e, sonra da ' a bakar `excludedFileNameExtensions` . Bu, aynı dosya uzantısının her iki listede de mevcutsa, dizin oluşturma işleminin dışlanacağını belirtir.

<a name="PartsOfBlobToIndex"></a>
## <a name="controlling-which-parts-of-the-blob-are-indexed"></a>Blob 'un hangi bölümlerinin dizine alınacağını denetleme

Yapılandırma parametresi kullanılarak Blobların hangi bölümlerinin dizine alınacağını kontrol edebilirsiniz `dataToExtract` . Bu, aşağıdaki değerleri alabilir:

* `storageMetadata`-yalnızca [Standart blob özelliklerinin ve Kullanıcı tarafından belirtilen meta verilerin](../storage/blobs/storage-properties-metadata.md) dizine alınacağını belirtir.
* `allMetadata`-blob içeriğinden ayıklanan depolama meta verilerinin ve [içerik türü belirli meta verilerin](#ContentSpecificMetadata) dizine alınacağını belirtir.
* `contentAndMetadata`-bloba ayıklanan tüm meta veri ve metin içeriğinin dizine alınacağını belirtir. Varsayılan değer budur.

Örneğin, yalnızca depolama meta verilerini indekslemek için şunu kullanın:

```http
    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "dataToExtract" : "storageMetadata" } }
    }
```

### <a name="using-blob-metadata-to-control-how-blobs-are-indexed"></a>Blobların nasıl dizine alınacağını denetlemek için blob meta verilerini kullanma

Yukarıda açıklanan yapılandırma parametreleri tüm Bloblar için geçerlidir. Bazen, *ayrı Blobların* nasıl dizine alınacağını denetlemek isteyebilirsiniz. Bunu, aşağıdaki blob meta veri özelliklerini ve değerlerini ekleyerek yapabilirsiniz:

| Özellik adı | Özellik değeri | Açıklama |
| --- | --- | --- |
| AzureSearch_Skip |değeri |Blob Dizin oluşturucuyu blobu tamamen atlayacak şekilde yönlendirir. Meta veri veya içerik ayıklama denenmez. Bu, belirli bir blob sürekli olarak başarısız olduğunda ve dizin oluşturma işlemini kesintiye uğradığında yararlı olur. |
| AzureSearch_SkipContent |değeri |Bu, `"dataToExtract" : "allMetadata"` [yukarıda](#PartsOfBlobToIndex) belirtilen bir Blobun kapsamına alınan ayarların eşdeğeridir. |

<a name="DealingWithErrors"></a>
## <a name="dealing-with-errors"></a>Hatalarla ilgilenme

Varsayılan olarak, blob Indexer, desteklenmeyen içerik türü (örneğin, bir görüntü) ile bir blob ile karşılaştığında yanıt vermez. Tabii ki, `excludedFileNameExtensions` belirli içerik türlerini atlamak için parametresini kullanabilirsiniz. Ancak, tüm olası içerik türlerini önceden bilmeden blob 'ları dizinleyebilirsiniz. Desteklenmeyen bir içerik türüyle karşılaşıldığında dizine almaya devam etmek için `failOnUnsupportedContentType` yapılandırma parametresini şu şekilde ayarlayın `false` :

```http
    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "failOnUnsupportedContentType" : false } }
    }
```

Bazı Bloblar için Azure Bilişsel Arama, içerik türünü belirleyemiyor veya başka türlü desteklenen içerik türündeki bir belgeyi işleyemez. Bu hata modunu yoksaymak için `failOnUnprocessableDocument` yapılandırma parametresini false olarak ayarlayın:

```http
      "parameters" : { "configuration" : { "failOnUnprocessableDocument" : false } }
```

Azure Bilişsel Arama, dizini oluşturulmuş Blobların boyutunu sınırlandırır. Bu sınırlar [Azure bilişsel arama hizmet sınırları](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity)bölümünde belgelenmiştir. Büyük bir blob, varsayılan olarak hata olarak değerlendirilir. Ancak yapılandırma parametresini doğru olarak ayarlarsanız, hala büyük Blobların depolama meta verilerinin dizinini oluşturabilirsiniz `indexStorageMetadataOnlyForOversizedDocuments` : 

```http
    "parameters" : { "configuration" : { "indexStorageMetadataOnlyForOversizedDocuments" : true } }
```

Blob 'ları ayrıştırırken veya bir dizine belge eklerken, herhangi bir işlem noktasında hatalar meydana geliyorsa dizin oluşturmaya da devam edebilirsiniz. Belirli sayıda hatayı yoksaymak için, `maxFailedItems` ve `maxFailedItemsPerBatch` yapılandırma parametrelerini istenen değerlere ayarlayın. Örnek:

```http
    {
      ... other parts of indexer definition
      "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 10 }
    }
```

## <a name="incremental-indexing-and-deletion-detection"></a>Artımlı dizin oluşturma ve silme algılaması

Bir blob Dizin Oluşturucuyu bir zamanlamaya göre çalışacak şekilde ayarlarken, blob 'un zaman damgasıyla belirlendiği şekilde yalnızca değiştirilen Blobları yeniden dizinleyebilirsiniz `LastModified` .

> [!NOTE]
> Değişiklik algılama ilkesi belirtmeniz gerekmez – artımlı dizin oluşturma sizin için otomatik olarak etkinleştirilir.

Belge silmeyi desteklemek için "geçici silme" yaklaşımını kullanın. Blob 'ları sağ silme, ilgili belgeler arama dizininden kaldırılmaz.

Geçici silme yaklaşımını uygulamak için iki yol vardır. Her ikisi de aşağıda açıklanmıştır.

### <a name="native-blob-soft-delete-preview"></a>Yerel blob geçici silme (Önizleme)

> [!IMPORTANT]
> Yerel blob geçici silme desteği önizlemededir. Önizleme işlevselliği, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). [REST API sürüm 2020-06-30-önizleme](https://docs.microsoft.com/azure/search/search-api-preview) bu özelliği sağlar. Şu anda portal veya .NET SDK desteği yok.

> [!NOTE]
> Yerel blob geçici silme ilkesini kullanırken, dizininizdeki belgelerin belge anahtarlarının bir blob özelliği veya blob meta verileri olması gerekir.

Bu yöntemde, Azure Blob depolama tarafından sunulan [Yerel blob geçici silme](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete) özelliğini kullanacaksınız. Depolama hesabınızda yerel blob geçici silme etkinse, veri kaynağınız yerel bir geçici silme ilkesi kümesine sahiptir ve Dizin Oluşturucu, geçici olarak silinen bir duruma geçiş yapılmış bir blob buluyor, Dizin Oluşturucu bu belgeyi dizinden kaldırır. Blob 'ları Azure Data Lake Storage 2. dizinlerken yerel blob geçici silme ilkesi desteklenmez.

Aşağıdaki adımları kullanın:
1. [Azure Blob depolaması için yerel geçici silme](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete)özelliğini etkinleştirin. Bekletme ilkesini, Dizin Oluşturucu aralığı zamanlamadan çok daha yüksek bir değere ayarlamanız önerilir. Bu şekilde, Dizin oluşturucuyu çalıştıran bir sorun varsa veya dizinlemek için çok sayıda belgeniz varsa, dizin oluşturucunun, geçici olarak silinen Blobları işlemesi çok fazla zaman vardır. Azure Bilişsel Arama Dizin oluşturucular, bir belgeyi, geçici olarak silinen bir durumdayken blobu işliyorsa siler.
1. Veri kaynağında yerel bir blob geçici silme algılama ilkesi yapılandırın. Aşağıda bir örnek gösterilmiştir. Bu özellik önizlemede olduğundan, önizleme REST API kullanmanız gerekir.
1. Dizin oluşturucuyu çalıştırın veya dizin Oluşturucuyu bir zamanlamaya göre çalışacak şekilde ayarlayın. Dizin Oluşturucu çalıştırıldığında ve blobu işlediğinde belge dizinden kaldırılır.

    ```
    PUT https://[service name].search.windows.net/datasources/blob-datasource?api-version=2020-06-30-Preview
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

#### <a name="reindexing-undeleted-blobs"></a>Silinmeyen Blobların yeniden oluşturulması

Depolama hesabınızda yerel geçici silme özelliği etkin olan bir blob 'u Azure Blob depolamadan silerseniz, blob, bu Blobun süre içinde geri alma seçeneği sunarak geçici olarak silinen bir duruma geçer. Bir Azure Bilişsel Arama veri kaynağında yerel bir blob geçici silme ilkesi olduğunda ve Dizin Oluşturucu geçici olarak silinen bir blobu işlediğinde, bu belgeyi dizinden kaldırır. Blob daha sonra silindiğinde, Dizin Oluşturucu her zaman o blobu yeniden kullanmaz. Bunun nedeni, dizin oluşturucunun blob zaman damgasına göre hangi Blobların dizine eklenebileceğini belirler `LastModified` . Geçici olarak silinen bir blob silinmeden `LastModified` önce zaman damgası güncellenmez. bu nedenle, Dizin Oluşturucu, `LastModified` silinmeyen Blobun daha yeni bir zaman damgalarına sahip Blobları zaten işledi ve silinmemiş blobu yeniden oluşturmaz. Silinmeyen bir Blobun yeniden dizinlendiğinden emin olmak için Blobun `LastModified` zaman damgasını güncelleştirmeniz gerekir. Bunu yapmanın bir yolu, söz konusu Blobun meta verilerinin yeniden kaydedilerek yapılır. Meta verileri değiştirmeniz gerekmez, ancak meta verileri yeniden kaydetmeniz, `LastModified` dizin oluşturucunun bu blobu yeniden eklemesi gerektiğini bilmesi için Blobun zaman damgasını güncelleştirir.

### <a name="soft-delete-using-custom-metadata"></a>Özel meta verileri kullanarak geçici silme

Bu yöntemde, bir belgenin arama dizininden ne zaman kaldırılması gerektiğini göstermek için bir Blobun meta verilerini kullanacaksınız.

Aşağıdaki adımları kullanın:

1. Bilişsel Arama Azure 'da mantıksal olarak silindiğini göstermek için blob 'a özel bir meta veri anahtar-değer çifti ekleyin.
1. Veri kaynağında geçici silme sütunu algılama ilkesi yapılandırın. Aşağıda bir örnek gösterilmiştir.
1. Dizin Oluşturucu blobu işledikten ve belgeyi dizinden sildikten sonra, blob 'u Azure Blob depolama için silebilirsiniz.

Örneğin, aşağıdaki ilke, bir blob 'un şu değere sahip bir meta veri özelliği varsa silineceğini kabul eder `IsDeleted` `true` :

```http
    PUT https://[service name].search.windows.net/datasources/blob-datasource?api-version=2020-06-30
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
```

#### <a name="reindexing-undeleted-blobs"></a>Silinmeyen Blobların yeniden oluşturulması

Veri kaynağınızda geçici bir silme sütunu algılama ilkesi ayarlarsanız, bir bloba işaret değeri ile özel meta verileri ekleyin ve ardından dizin oluşturucuyu çalıştırın, Dizin Oluşturucu bu belgeyi dizinden kaldırır. Bu belgeyi yeniden eklemek isterseniz, söz konusu Blobun için geçici silme meta verisi değerini değiştirmeniz ve Dizin oluşturucuyu yeniden çalıştırmanız yeterlidir.

## <a name="indexing-large-datasets"></a>Büyük veri kümelerini dizinleme

Dizin oluşturma Blobları zaman alan bir işlem olabilir. Dizin oluşturmak için milyonlarca blob 'un bulunduğu durumlarda, verileri bölümleyerek ve paralel olarak verileri işlemek için birden çok Dizin Oluşturucu kullanarak dizin oluşturmayı hızlandırabilirsiniz. Bunu nasıl ayarlayabilmeniz gerekir:

- Verilerinizi birden çok blob kapsayıcılarına veya sanal klasöre bölme
- Her kapsayıcı veya klasör için bir tane olmak üzere çeşitli Azure Bilişsel Arama veri kaynakları ayarlayın. Blob klasörünü işaret etmek için şu `query` parametreyi kullanın:

    ```
    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : "my-folder" }
    }
    ```

- Her veri kaynağı için karşılık gelen bir Dizin Oluşturucu oluşturun. Tüm Dizin oluşturucular aynı hedef arama dizinine işaret edebilir.  

- Hizmetinizdeki bir arama birimi, belirli bir zamanda bir Dizin Oluşturucu çalıştırabilir. Yukarıda açıklandığı gibi birden çok Dizin Oluşturucu oluşturmak yalnızca aslında paralel olarak çalıştırıldıklarında yararlıdır. Birden çok dizin oluşturucuyu paralel olarak çalıştırmak için, uygun sayıda bölüm ve çoğaltma oluşturarak arama hizmetinizi ölçeklendirin. Örneğin, arama hizmetinizin 6 arama birimi varsa (örneğin, 2 bölüm x 3 çoğaltmalar), 6 Dizin oluşturucular eşzamanlı olarak çalışabilir ve dizin oluşturma sırasında altı kat artışına neden olur. Ölçeklendirme ve kapasite planlaması hakkında daha fazla bilgi edinmek için bkz. [Azure bilişsel arama 'de sorgu ve dizin oluşturma iş yükleri için kaynak düzeylerini ölçeklendirme](search-capacity-planning.md).

## <a name="indexing-documents-along-with-related-data"></a>İlgili verilerle birlikte belgelerin dizinini oluşturma

Dizininizdeki birden fazla kaynaktaki belgeleri "birleştirmek" isteyebilirsiniz. Örneğin, Cosmos DB içinde depolanan diğer meta verilerle bloblardan metin birleştirmek isteyebilirsiniz. Birden çok bölümden arama belgeleri oluşturmak için çeşitli dizin oluşturucularla birlikte gönderme dizin oluşturma API 'sini de kullanabilirsiniz. 

Bunun çalışması için, tüm dizin oluşturucularının ve diğer bileşenlerin belge anahtarını kabul etmesi gerekir. Bu konuyla ilgili daha fazla bilgi için bkz. [birden çok Azure veri kaynağını dizine](https://docs.microsoft.com/azure/search/tutorial-multiple-data-sources)gönderme. Ayrıntılı bir adım adım için bkz. bu dış Makale: [belgeleri Azure bilişsel arama diğer verilerle birleştirme](https://blog.lytzen.name/2017/01/combine-documents-with-other-data-in.html).

<a name="IndexingPlainText"></a>
## <a name="indexing-plain-text"></a>Düz metin dizini oluşturma 

Tüm bloblarınız aynı kodlamada düz metin içeriyorsa, **metin ayrıştırma modunu**kullanarak dizin oluşturma performansını önemli ölçüde artırabilirsiniz. Metin ayrıştırma modunu kullanmak için `parsingMode` yapılandırma özelliğini şu şekilde ayarlayın `text` :

```http
    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "parsingMode" : "text" } }
    }
```

Varsayılan olarak, `UTF-8` kodlama varsayılır. Farklı bir kodlama belirtmek için `encoding` yapılandırma özelliğini kullanın: 

```http
    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "parsingMode" : "text", "encoding" : "windows-1252" } }
    }
```

<a name="ContentSpecificMetadata"></a>
## <a name="content-type-specific-metadata-properties"></a>İçerik türüne özgü meta veri özellikleri
Aşağıdaki tabloda her belge biçimi için yapılan işlem özetlenmektedir ve Azure Bilişsel Arama tarafından ayıklanan meta veri özellikleri açıklanmaktadır.

| Belge biçimi/içerik türü | İçerik türü belirli meta veri özellikleri | İşleme ayrıntıları |
| --- | --- | --- |
| HTML (metin/html) |`metadata_content_encoding`<br/>`metadata_content_type`<br/>`metadata_language`<br/>`metadata_description`<br/>`metadata_keywords`<br/>`metadata_title` |HTML biçimlendirmesini şerit ve metin Ayıkla |
| PDF (uygulama/PDF) |`metadata_content_type`<br/>`metadata_language`<br/>`metadata_author`<br/>`metadata_title` |Gömülü belgeler dahil olmak üzere metni Ayıkla (görüntüler hariç) |
| DOCX (uygulama/vnd.openxmlformats-officedocument.wordprocessingml.document) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Gömülü belgeler dahil olmak üzere metni Ayıkla |
| DOC (Application/msword) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Gömülü belgeler dahil olmak üzere metni Ayıkla |
| DOCM (Application/vnd.ms-word.document. macroenabled. 12) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Gömülü belgeler dahil olmak üzere metni Ayıkla |
| WORD XML (application/vnd. MS-word2006ml) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |XML işaretlemesini şerit ve metin Ayıkla |
| WORD 2003 XML (application/vnd. MS-WordML) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date` |XML işaretlemesini şerit ve metin Ayıkla |
| XLSX (application/vnd. openxmlformats-officedocument. elektronik sayfaml. Sheet) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Gömülü belgeler dahil olmak üzere metni Ayıkla |
| XLS (application/vnd. MS-Excel) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Gömülü belgeler dahil olmak üzere metni Ayıkla |
| XLSM (application/vnd. MS-Excel. Sheet. makroenabled. 12) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Gömülü belgeler dahil olmak üzere metni Ayıkla |
| PPTX (application/vnd. openxmlformats-officedocument. presentationml. Presentation) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Gömülü belgeler dahil olmak üzere metni Ayıkla |
| PPT (application/vnd. MS-PowerPoint) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Gömülü belgeler dahil olmak üzere metni Ayıkla |
| PPTM (application/vnd. MS-PowerPoint. Presentation. macroenabled. 12) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` |Gömülü belgeler dahil olmak üzere metni Ayıkla |
| MSG (application/vnd. MS-Outlook) |`metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_from_email`<br/>`metadata_message_to`<br/>`metadata_message_to_email`<br/>`metadata_message_cc`<br/>`metadata_message_cc_email`<br/>`metadata_message_bcc`<br/>`metadata_message_bcc_email`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_subject` |Eklerden ayıklanan metin de dahil olmak üzere metni ayıkla. `metadata_message_to_email``metadata_message_cc_email`ve `metadata_message_bcc_email` dize koleksiyonlarıdır, alanların geri kalanı dizelerdir.|
| ODT (application/vnd. oassıs. OpenDocument. Text) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` |Gömülü belgeler dahil olmak üzere metni Ayıkla |
| ODS (application/vnd. oassıs. OpenDocument. Spreadsheet) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` |Gömülü belgeler dahil olmak üzere metni Ayıkla |
| ODP (application/vnd. oassıs. OpenDocument. Presentation) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`title` |Gömülü belgeler dahil olmak üzere metni Ayıkla |
| ZIP (uygulama/zip) |`metadata_content_type` |Arşivdeki tüm belgelerden metin Ayıkla |
| GZ (uygulama/gzip) |`metadata_content_type` |Arşivdeki tüm belgelerden metin Ayıkla |
| EPUB (Application/EPUB + zip) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_title`<br/>`metadata_description`<br/>`metadata_language`<br/>`metadata_keywords`<br/>`metadata_identifier`<br/>`metadata_publisher` |Arşivdeki tüm belgelerden metin Ayıkla |
| XML (Application/XML) |`metadata_content_type`<br/>`metadata_content_encoding`<br/> |XML işaretlemesini şerit ve metin Ayıkla |
| JSON (uygulama/JSON) |`metadata_content_type`<br/>`metadata_content_encoding` |Metin ayıklama<br/>NOTE: bir JSON blobundan birden çok belge alanı ayıklamanız gerekiyorsa, Ayrıntılar için bkz. [JSON Bloblarını dizine ekleme](search-howto-index-json-blobs.md) |
| EML (ileti/rfc822) |`metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_to`<br/>`metadata_message_cc`<br/>`metadata_creation_date`<br/>`metadata_subject` |Ekleri dahil metin Ayıkla |
| RTF (uygulama/RTF) |`metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_page_count`<br/>`metadata_word_count`<br/> | Metin ayıklama|
| Düz metin (metin/düz) |`metadata_content_type`<br/>`metadata_content_encoding`<br/> | Metin ayıklama|


## <a name="help-us-make-azure-cognitive-search-better"></a>Azure Bilişsel Arama daha iyi hale getirmemize yardımcı olun
Geliştirmeler için özellik istekleriniz veya fikirler varsa [UserVoice sitemizi](https://feedback.azure.com/forums/263029-azure-search/)bize tanıyın.
