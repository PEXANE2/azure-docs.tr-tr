---
title: Blob Dizin Oluşturucu yapılandırma
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama 'de tam metin arama işlemleri için blob içeriğini dizinlemeyi otomatik hale getirmek üzere bir Azure Blob Dizin Oluşturucu ayarlayın.
manager: nitinme
author: MarkHeff
ms.author: maheff
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 09/23/2020
ms.openlocfilehash: e3419711c9a7358914f85574f6dbd5af29def1cf
ms.sourcegitcommit: dc68a2c11bae2e9d57310d39fbed76628233fd7f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/28/2020
ms.locfileid: "91403628"
---
# <a name="how-to-configure-a-blob-indexer-in-azure-cognitive-search"></a>Azure Bilişsel Arama blob Dizin Oluşturucu yapılandırma

Bu makalede, Azure Blob depolamada depolanan metin tabanlı belgeleri (Örneğin PDF 'Ler, Microsoft Office belgeler ve diğer birçok yaygın biçim) dizin oluşturma amacıyla Azure Bilişsel Arama nasıl kullanabileceğiniz gösterilmektedir. İlk olarak, blob Dizin oluşturucuyu ayarlama ve yapılandırma hakkında temel bilgileri açıklar. Daha sonra, büyük olasılıkla karşılaştığınız davranış ve senaryolar hakkında daha ayrıntılı bir araştırma sunar.

<a name="SupportedFormats"></a>

## <a name="supported-formats"></a>Desteklenen biçimler

Blob Indexer aşağıdaki belge biçimlerinden metin ayıklayabilir:

[!INCLUDE [search-blob-data-sources](../../includes/search-blob-data-sources.md)]

## <a name="set-up-blob-indexing"></a>Blob dizinlemeyi ayarlama

Kullanarak bir Azure Blob depolama Dizin Oluşturucu ayarlayabilirsiniz:

* [Azure Portal](https://ms.portal.azure.com)
* Azure Bilişsel Arama [REST API](/rest/api/searchservice/Indexer-operations)
* Azure Bilişsel Arama [.NET SDK](/dotnet/api/overview/azure/search)

> [!NOTE]
> Bazı Özellikler (örneğin, alan eşlemeleri) portalda henüz kullanılamaz ve programlı olarak kullanılması gerekir.
>

Burada, akışı REST API kullanarak gösteririz.

### <a name="step-1-create-a-data-source"></a>1. Adım: Veri kaynağı oluşturma

Veri kaynağı, hangi verilerin dizine erişmesi gerektiğini, verilere erişmek için gereken kimlik bilgilerini ve verilerdeki değişiklikleri verimli bir şekilde belirlemek için ilkeleri (yeni, değiştirilmiş veya silinen satırlar) belirler. Aynı arama hizmetinde birden çok Dizin Oluşturucu tarafından bir veri kaynağı kullanılabilir.

Blob dizin oluşturma için veri kaynağı aşağıdaki gerekli özelliklere sahip olmalıdır:

* **ad** , arama hizmetinizin içindeki veri kaynağının benzersiz adıdır.
* **tür** olmalıdır `azureblob` .
* * * kimlik bilgileri, depolama hesabı bağlantı dizesini parametre olarak sağlar `credentials.connectionString` . Ayrıntılar için aşağıdaki [kimlik bilgilerini belirtme](#Credentials) konusuna bakın.
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

Veri kaynağı API 'SI oluşturma hakkında daha fazla bilgi için bkz. [veri kaynağı oluşturma](/rest/api/searchservice/create-data-source).

<a name="Credentials"></a>

#### <a name="how-to-specify-credentials"></a>Kimlik bilgilerini belirtme

Blob kapsayıcısının kimlik bilgilerini şu yollarla sağlayabilirsiniz:

* **Yönetilen kimlik bağlantı dizesi**: `ResourceId=/subscriptions/<your subscription ID>/resourceGroups/<your resource group name>/providers/Microsoft.Storage/storageAccounts/<your storage account name>/;` 

  Bu bağlantı dizesinde hesap anahtarı gerekmez, ancak [yönetilen bir kimlik kullanarak bir Azure depolama hesabına bağlantı ayarlama](search-howto-managed-identities-storage.md)yönergelerini izlemeniz gerekir.

* **Tam erişimli depolama hesabı bağlantı dizesi**: `DefaultEndpointsProtocol=https;AccountName=<your storage account>;AccountKey=<your account key>`

  Azure portal > anahtarlar (klasik depolama hesapları için) veya ayarlar > erişim anahtarları (Azure Resource Manager depolama hesapları için) > depolama hesabı dikey penceresine giderek bağlantı dizesini alabilirsiniz.

* **Depolama hesabı paylaşılan erişim imzası** (SAS) bağlantı dizesi: `BlobEndpoint=https://<your account>.blob.core.windows.net/;SharedAccessSignature=?sv=2016-05-31&sig=<the signature>&spr=https&se=<the validity end time>&srt=co&ss=b&sp=rl`

  SAS, kapsayıcılar ve nesneler (Bu durumda Bloblar) üzerinde liste ve okuma izinlerine sahip olmalıdır.

* **Kapsayıcı paylaşılan erişim imzası**: `ContainerSharedAccessUri=https://<your storage account>.blob.core.windows.net/<container name>?sv=2016-05-31&sr=c&sig=<the signature>&se=<the validity end time>&sp=rl`

  SAS, kapsayıcıda liste ve okuma izinlerine sahip olmalıdır.

Depolama paylaşılan erişim imzaları hakkında daha fazla bilgi için bkz. [paylaşılan erişim Imzalarını kullanma](../storage/common/storage-sas-overview.md).

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

Daha fazla bilgi için bkz. [Dizin oluşturma (REST API)](/rest/api/searchservice/create-index).

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

Daha fazla bilgi için bkz. [Dizin Oluşturucu oluşturma (REST API)](/rest/api/searchservice/create-indexer). Dizin Oluşturucu zamanlamalarını tanımlama hakkında daha fazla bilgi için bkz. [Azure bilişsel arama için Dizin Oluşturucu zamanlama](search-howto-schedule-indexers.md).

<a name="how-azure-search-indexes-blobs"></a>

## <a name="how-blobs-are-indexed"></a>Blobların dizini oluşturma

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

<a name="DocumentKeys"></a>

### <a name="defining-document-keys-and-field-mappings"></a>Belge anahtarlarını ve alan eşlemelerini tanımlama

Azure Bilişsel Arama 'de belge anahtarı bir belgeyi benzersiz şekilde tanımlar. Her arama dizininde Edm. String türünde tam olarak bir anahtar alanı olmalıdır. Dizine eklenmekte olan her belge için anahtar alanı gereklidir (aslında yalnızca gerekli olan alandır).  

Hangi ayıklanan alanın, dizininiz için anahtar alanla eşleşmesi gerektiğini dikkatle düşünmeniz gerekir. Adaylar şunlardır:

* **meta veri \_ depolama \_ adı** -bu kullanışlı bir aday olabilir, ancak 1) adların benzersiz olamayacağını, farklı klasörlerde aynı ada sahip bloblarınız olabilir ve 2) ad, tireler gibi belge anahtarlarında geçersiz karakterler içeriyor olabilir. `base64Encode` [Alan eşleme işlevini](search-indexer-field-mappings.md#base64EncodeFunction) kullanarak geçersiz karakterlerle işlem yapabilirsiniz-bunu yaparsanız, arama gibi API çağrılarına geçirirken belge anahtarlarını kodlamayı unutmayın. (Örneğin, .NET 'te bu amaçla [UrlTokenEncode yöntemini](/dotnet/api/system.web.httpserverutility.urltokenencode) kullanabilirsiniz).

* **meta veri \_ depolama \_ yolu** -tam yolun kullanılması benzersizlik sağlar, ancak yol kesinlikle `/` [bir belge anahtarında geçersiz](/rest/api/searchservice/naming-rules)olan karakterler içeriyor.  Yukarıdaki gibi, işlevini kullanarak anahtarları kodlama seçeneğiniz vardır `base64Encode` [function](search-indexer-field-mappings.md#base64EncodeFunction).

* Üçüncü seçenek, bloblara özel meta veri özelliği eklemektir. Ancak, bu seçenek, blob karşıya yükleme işleminin bu meta veri özelliğini tüm bloblara eklemesine gerek duyar. Anahtar gerekli bir özellik olduğundan, bu özelliğe sahip olmayan tüm Blobların dizini oluşturulamaz.

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

Daha fazla bilgi için bkz. [alan eşlemeleri ve dönüştürmeleri](search-indexer-field-mappings.md).

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

## <a name="index-by-file-type"></a>Dosya türüne göre dizin

Hangi Blobların dizine alınacağını ve hangilerinin atlandığını denetleyebilirsiniz.

### <a name="include-blobs-having-specific-file-extensions"></a>Belirli dosya uzantılarına sahip Blobları dahil et

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

### <a name="exclude-blobs-having-specific-file-extensions"></a>Belirli dosya uzantılarına sahip Blobları Dışla

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

## <a name="index-parts-of-a-blob"></a>Bir Blobun dizin parçaları

Yapılandırma parametresi kullanılarak Blobların hangi bölümlerinin dizine alınacağını kontrol edebilirsiniz `dataToExtract` . Bu, aşağıdaki değerleri alabilir:

* `storageMetadata` -yalnızca [Standart blob özelliklerinin ve Kullanıcı tarafından belirtilen meta verilerin](../storage/blobs/storage-blob-container-properties-metadata.md) dizine alınacağını belirtir.
* `allMetadata` -blob içeriğinden ayıklanan depolama meta verilerinin ve [içerik türü belirli meta verilerin](#ContentSpecificMetadata) dizine alınacağını belirtir.
* `contentAndMetadata` -bloba ayıklanan tüm meta veri ve metin içeriğinin dizine alınacağını belirtir. Varsayılan değer budur.

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

## <a name="index-from-multiple-sources"></a>Birden çok kaynaktan Dizin

Dizininizdeki birden fazla kaynaktaki belgeleri "birleştirmek" isteyebilirsiniz. Örneğin, Cosmos DB içinde depolanan diğer meta verilerle bloblardan metin birleştirmek isteyebilirsiniz. Birden çok bölümden arama belgeleri oluşturmak için çeşitli dizin oluşturucularla birlikte gönderme dizin oluşturma API 'sini de kullanabilirsiniz.

Bunun çalışması için, tüm dizin oluşturucularının ve diğer bileşenlerin belge anahtarını kabul etmesi gerekir. Bu konu hakkında daha fazla bilgi için bkz. [birden çok Azure veri kaynağını](./tutorial-multiple-data-sources.md) veya bu blog gönderisini dizine gönderme, [belgeleri Azure bilişsel arama diğer verilerle birleştirme](https://blog.lytzen.name/2017/01/combine-documents-with-other-data-in.html).

## <a name="index-large-datasets"></a>Büyük veri kümelerini diz

Dizin oluşturma Blobları zaman alan bir işlem olabilir. Dizin oluşturmak için milyonlarca blob 'un bulunduğu durumlarda, verileri bölümleyerek ve paralel olarak verileri işlemek için birden çok Dizin Oluşturucu kullanarak dizin oluşturmayı hızlandırabilirsiniz. Bunu nasıl ayarlayabilmeniz gerekir:

* Verilerinizi birden çok blob kapsayıcılarına veya sanal klasöre bölme

* Her kapsayıcı veya klasör için bir tane olmak üzere çeşitli Azure Bilişsel Arama veri kaynakları ayarlayın. Blob klasörünü işaret etmek için şu `query` parametreyi kullanın:

    ```json
    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : "my-folder" }
    }
    ```

* Her veri kaynağı için karşılık gelen bir Dizin Oluşturucu oluşturun. Tüm Dizin oluşturucular aynı hedef arama dizinine işaret edebilir.  

* Hizmetinizdeki bir arama birimi, belirli bir zamanda bir Dizin Oluşturucu çalıştırabilir. Yukarıda açıklandığı gibi birden çok Dizin Oluşturucu oluşturmak yalnızca aslında paralel olarak çalıştırıldıklarında yararlıdır. Birden çok dizin oluşturucuyu paralel olarak çalıştırmak için, uygun sayıda bölüm ve çoğaltma oluşturarak arama hizmetinizi ölçeklendirin. Örneğin, arama hizmetinizin 6 arama birimi varsa (örneğin, 2 bölüm x 3 çoğaltmalar), 6 Dizin oluşturucular eşzamanlı olarak çalışabilir ve dizin oluşturma sırasında altı kat artışına neden olur. Ölçeklendirme ve kapasite planlaması hakkında daha fazla bilgi edinmek için bkz. [Azure bilişsel arama hizmetinin kapasitesini ayarlama](search-capacity-planning.md).

<a name="DealingWithErrors"></a>

## <a name="handle-errors"></a>Hataları işleme

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

Azure Bilişsel Arama, dizini oluşturulmuş Blobların boyutunu sınırlandırır. Bu sınırlar [Azure bilişsel arama hizmet sınırları](./search-limits-quotas-capacity.md)bölümünde belgelenmiştir. Büyük bir blob, varsayılan olarak hata olarak değerlendirilir. Ancak yapılandırma parametresini doğru olarak ayarlarsanız, hala büyük Blobların depolama meta verilerinin dizinini oluşturabilirsiniz `indexStorageMetadataOnlyForOversizedDocuments` :

```http
    "parameters" : { "configuration" : { "indexStorageMetadataOnlyForOversizedDocuments" : true } }
```

Blob 'ları ayrıştırırken veya bir dizine belge eklerken, herhangi bir işlem noktasında hatalar meydana geliyorsa dizin oluşturmaya da devam edebilirsiniz. Belirli sayıda hatayı yoksaymak için, `maxFailedItems` ve `maxFailedItemsPerBatch` yapılandırma parametrelerini istenen değerlere ayarlayın. Örneğin:

```http
    {
      ... other parts of indexer definition
      "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 10 }
    }
```

<a name="ContentSpecificMetadata"></a>

## <a name="content-type-specific-metadata-properties"></a>İçerik türüne özgü meta veri özellikleri

Aşağıdaki tabloda her belge biçimi için yapılan işlem özetlenmektedir ve Azure Bilişsel Arama tarafından ayıklanan meta veri özellikleri açıklanmaktadır.

| Belge biçimi/içerik türü | Ayıklanan meta veriler | İşleme ayrıntıları |
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

## <a name="see-also"></a>Ayrıca bkz.

* [Azure Bilişsel Arama'daki Dizin Oluşturucular](search-indexer-overview.md)
* [AI kullanarak blob 'ları anlama](search-blob-ai-integration.md)
* [Blob dizinlemeye genel bakış](search-blob-storage-integration.md)