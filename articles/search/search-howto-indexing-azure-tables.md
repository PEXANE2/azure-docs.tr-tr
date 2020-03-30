---
title: Azure Tablo depolama içeriği üzerinden arama yapın
titleSuffix: Azure Cognitive Search
description: Azure Tablo depolama alanında depolanan verileri azure bilişsel arama dizinleyicisiyle nasıl dizine ekinlerlenizi öğrenin.
manager: nitinme
author: mgottein
ms.author: magottei
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: e8f6c0454497b1cb1d62417e566e9662469c56d0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74113007"
---
# <a name="how-to-index-tables-from-azure-table-storage-with-azure-cognitive-search"></a>Azure Bilişsel Arama ile Azure Tablo depolamasından tabloları dizin oluşturma

Bu makalede, Azure Tablo depolama alanında depolanan verileri dizinlemek için Azure Bilişsel Arama'nın nasıl kullanılacağı gösterilmektedir.

## <a name="set-up-azure-table-storage-indexing"></a>Azure Tablo depolama dizini oluşturma

Bu kaynakları kullanarak bir Azure Tablo depolama dizinleyicisi ayarlayabilirsiniz:

* [Azure portalında](https://ms.portal.azure.com)
* Azure Bilişsel Arama [REST API](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations)
* Azure Bilişsel Arama [.NET SDK](https://aka.ms/search-sdk)

Burada REST API kullanarak akışı göstermek. 

### <a name="step-1-create-a-datasource"></a>Adım 1: Veri kaynağı oluşturma

Bir veri kaynağı, hangi verileri dizine ekleyip, verilere erişmek için gereken kimlik bilgilerini ve Azure Bilişsel Arama'nın verilerdeki değişiklikleri verimli bir şekilde tanımlamasını sağlayan ilkeleri belirtir.

Tablo dizini için veri kaynağının aşağıdaki özelliklere sahip olması gerekir:

- **adı,** arama hizmetinizdeki veri kaynağının benzersiz adıdır.
- **türü** olmalıdır. `azuretable`
- **kimlik bilgileri** parametresi depolama hesabı bağlantı dizesini içerir. Ayrıntılar için [kimlik bilgilerini belirt](#Credentials) bölümüne bakın.
- **kapsayıcı** tablo adını ve isteğe bağlı bir sorgu ayarlar.
    - Parametreyi kullanarak tablo `name` adını belirtin.
    - İsteğe bağlı olarak, parametreyi kullanarak bir sorgu belirtin. `query` 

> [!IMPORTANT] 
> Mümkün olduğunda, daha iyi performans için PartitionKey'de bir filtre kullanın. Başka bir sorgu, büyük tablolar için düşük performansla sonuçlanan tam bir tablo tarar. Performans [hususları](#Performance) bölümüne bakın.


Bir veri kaynağı oluşturmak için:

    POST https://[service name].search.windows.net/datasources?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "table-datasource",
        "type" : "azuretable",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-table", "query" : "PartitionKey eq '123'" }
    }   

Veri Kaynağı OLUŞTUR API'si hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/rest/api/searchservice/create-data-source)

<a name="Credentials"></a>
#### <a name="ways-to-specify-credentials"></a>Kimlik bilgilerini belirtme yolları ####

Tablonun kimlik bilgilerini aşağıdaki yollardan biriyle sağlayabilirsiniz: 

- **Tam erişimli depolama hesabı bağlantı dizesi** `DefaultEndpointsProtocol=https;AccountName=<your storage account>;AccountKey=<your account key>` : Depolama hesabı **bıçak** > **Ayarları** > **Anahtarları'na** (klasik depolama hesapları için) veya **Ayarlar** > Erişim**anahtarlarına** (Azure Kaynak Yöneticisi depolama hesapları için) giderek bağlantı dizesini Azure portalından alabilirsiniz.
- **Depolama hesabı paylaşılan erişim imza bağlantı dizesi**: `TableEndpoint=https://<your account>.table.core.windows.net/;SharedAccessSignature=?sv=2016-05-31&sig=<the signature>&spr=https&se=<the validity end time>&srt=co&ss=t&sp=rl` Paylaşılan erişim imzası nın listesi olmalı ve kapsayıcılar (bu durumda tablolar) ve nesneler (tablo satırları) üzerindeki izinleri okumalıdır.
-  **Tablo paylaşılan**erişim `ContainerSharedAccessUri=https://<your storage account>.table.core.windows.net/<table name>?tn=<table name>&sv=2016-05-31&sig=<the signature>&se=<the validity end time>&sp=r` imzası : Paylaşılan erişim imzasının tabloda sorgu (okuma) izinleri olmalıdır.

Depolama paylaşılan erişim imzaları hakkında daha fazla bilgi için [bkz.](../storage/common/storage-dotnet-shared-access-signature-part-1.md)

> [!NOTE]
> Paylaşılan erişim imzası kimlik bilgilerini kullanıyorsanız, son kullanma sürelerini önlemek için veri kaynağı kimlik bilgilerini düzenli aralıklarla yenilenen imzalarla güncelleştirmeniz gerekir. Paylaşılan erişim imzası kimlik bilgilerinin süresi dolduğunda, dizinleyici "Bağlantı dizesinde sağlanan kimlik bilgileri geçersiz dir veya süresi dolmuş" gibi bir hata iletisi ile başarısız olur.  

### <a name="step-2-create-an-index"></a>2. Adım: Dizin oluşturma
Dizin, bir belgedeki alanları, öznitelikleri ve arama deneyimini şekillendiren diğer yapıları belirtir.

Dizin oluşturmak için:

    POST https://[service name].search.windows.net/indexes?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
          "name" : "my-target-index",
          "fields": [
            { "name": "key", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "SomeColumnInMyTable", "type": "Edm.String", "searchable": true }
          ]
    }

Dizin oluşturma hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/rest/api/searchservice/create-index)

### <a name="step-3-create-an-indexer"></a>Adım 3: Bir dizin oluştur
Dizin leyici, bir veri kaynağını hedef arama dizinine bağlar ve veri yenilemesini otomatikleştirmek için bir zamanlama sağlar. 

Dizin ve veri kaynağı oluşturulduktan sonra dizin oluşturabilirsiniz:

    POST https://[service name].search.windows.net/indexers?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "table-indexer",
      "dataSourceName" : "table-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }

Bu dizinleyici her iki saatte bir çalışır. (Zamanlama aralığı "PT2H" olarak ayarlanır.) Her 30 dakikada bir dizinleyici çalıştırmak için aralığı "PT30M" olarak ayarlayın. Desteklenen en kısa aralık beş dakikadır. Zamanlama isteğe bağlıdır; atlanırsa, bir dizin oluşturucu oluşturulduğunda yalnızca bir kez çalışır. Ancak, istediğiniz zaman isteğe bağlı bir dizinleyici çalıştırabilirsiniz.   

Create Indexer API hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/rest/api/searchservice/create-indexer)

Dizin oluşturma zamanlamaları hakkında daha fazla bilgi için Azure Bilişsel Arama için dizin oluşturma yı [nasıl zamanlayınız'](search-howto-schedule-indexers.md)a bakın.

## <a name="deal-with-different-field-names"></a>Farklı alan adlarıyla anlaşma
Bazen, varolan dizininizdeki alan adları tablonuzdaki özellik adlarından farklıdır. Alan eşlemelerini, tablodaki özellik adlarını arama dizininizdeki alan adlarına eşlemek için kullanabilirsiniz. Alan eşlemeleri hakkında daha fazla bilgi edinmek için bkz: [Azure Bilişsel Arama dizinleyici alan eşlemeleri veri kaynakları ve arama dizinleri arasındaki farkları köprüler.](search-indexer-field-mappings.md)

## <a name="handle-document-keys"></a>Belge anahtarlarını işleme
Azure Bilişsel Arama'da belge anahtarı bir belgeyi benzersiz olarak tanımlar. Her arama dizininin tam `Edm.String`olarak bir anahtar türü olmalıdır. Dizin ekilen her belge için anahtar alanı gereklidir. (Aslında, gerekli tek alandır.)

Tablo satırlarının bileşik anahtarı olduğundan, Azure Bilişsel Arama, `Key` bölüm anahtarı ve satır anahtar değerlerinin bir araya geldiği sentetik bir alan oluşturur. Örneğin, bir satırın PartitionKey `PK1` ve RowKey `RK1`ise, `Key` o zaman `PK1RK1`alanın değeri.

> [!NOTE]
> Değer, `Key` tire gibi belge anahtarlarında geçersiz olan karakterler içerebilir. `base64Encode` [Alan eşleme işlevini](search-indexer-field-mappings.md#base64EncodeFunction)kullanarak geçersiz karakterlerle başa çıkabilirsiniz. Bunu yaparsanız, Arama gibi API çağrılarında belge anahtarlarını geçirirken de güvenli URL Base64 kodlamasını kullanmayı unutmayın.
>
>

## <a name="incremental-indexing-and-deletion-detection"></a>Artımlı dizin oluşturma ve silme algılama
Bir programda çalışacak bir tablo dizinleyicisi ayarladığınızda, bir `Timestamp` satırın değerine göre belirlenen yalnızca yeni veya güncelleştirilmiş satırları yeniden dizine eder. Değişiklik algılama ilkesi belirtmeniz gerekmez. Artımlı dizin oluşturma sizin için otomatik olarak etkinleştirilir.

Belirli belgelerin dizinden kaldırılması gerektiğini belirtmek için yumuşak silme stratejisi kullanabilirsiniz. Bir satırı silerken yerine, silindiğini belirtmek için bir özellik ekleyin ve veri kaynağında yumuşak bir silme algılama ilkesi ayarlayın. Örneğin, aşağıdaki ilke, satır değeri `IsDeleted` `"true"`olan bir özelliği varsa bir satır silinir dikkate alır:

    PUT https://[service name].search.windows.net/datasources?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-table-datasource",
        "type" : "azuretable",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "table name", "query" : "<query>" },
        "dataDeletionDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy", "softDeleteColumnName" : "IsDeleted", "softDeleteMarkerValue" : "true" }
    }   

<a name="Performance"></a>
## <a name="performance-considerations"></a>Performansla ilgili önemli noktalar

Varsayılan olarak, Azure Bilişsel Arama aşağıdaki `Timestamp >= HighWaterMarkValue`sorgu filtresini kullanır: . Azure tablolarının `Timestamp` alanında ikincil bir dizin olmadığından, bu tür bir sorgu tam tablo tonu gerektirir ve bu nedenle büyük tablolar için yavaştır.


Tablo dizinoluşturma performansını artırmak için iki olası yaklaşım aşağıda veda edinilebilir. Bu yaklaşımların her ikisi de tablo bölümlerini kullanır: 

- Verileriniz doğal olarak birkaç bölüm aralığına bölünebiliyorsa, her bölüm aralığı için bir veri kaynağı ve karşılık gelen bir dizin oluşturun. Her dizinleyici nin artık yalnızca belirli bir bölüm aralığını işlemesi ve bu da daha iyi sorgu performansı elde etmesi gerekiyor. Dizinlenmesi gereken verilerin az sayıda sabit bölümü varsa, daha da iyi: her dizinleyici yalnızca bir bölüm tarar. Örneğin, bir bölüm aralığını işlemek için bir `000` veri `100`kaynağı oluşturmak için , şu şekilde bir sorgu kullanın: 
    ```
    "container" : { "name" : "my-table", "query" : "PartitionKey ge '000' and PartitionKey lt '100' " }
    ```

- Verileriniz zamana göre bölümlenmişse (örneğin, her gün veya haftada yeni bir bölüm oluşturursanız), aşağıdaki yaklaşımı göz önünde bulundurun: 
    - Formun sorgusunu kullanın: `(PartitionKey ge <TimeStamp>) and (other filters)`. 
    - Dizini bul [Durum API'sini](https://docs.microsoft.com/rest/api/searchservice/get-indexer-status)kullanarak dizin leyici `<TimeStamp>` ilerlemesini izleyin ve sorgunun koşulunu en son başarılı yüksek su işareti değerine göre düzenli aralıklarla güncelleştirin. 
    - Bu yaklaşımla, tam bir yeniden dizini tetikleme niz gerekiyorsa, dizinleyicisıfırlamaek olarak veri kaynağı sorgusunu sıfırlamanız gerekir. 


## <a name="help-us-make-azure-cognitive-search-better"></a>Azure Bilişsel Arama'yı daha iyi hale getirmemize yardımcı olun
Geliştirmeler için özellik istekleriniz veya fikirleriniz varsa, bunları [UserVoice sitemizde](https://feedback.azure.com/forums/263029-azure-search/)gönderin.
