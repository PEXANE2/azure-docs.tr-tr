---
title: Tam metin arama için Azure Tablo depolamadan içerik dizini oluştur-Azure Search
description: Azure Tablo depolamada depolanan verileri Azure Search Dizin Oluşturucu ile nasıl dizinleyeceğinizi öğrenin.
ms.date: 05/02/2019
author: mgottein
manager: nitinme
ms.author: magottei
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: dffb0a41dbf33cd86014115b089036d69a8e4718
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69648178"
---
# <a name="index-azure-table-storage-with-azure-search"></a>Azure Tablo depolama alanını Azure Search ile dizinle
Bu makalede, Azure Tablo Depolaması 'nda depolanan verilerin dizinini oluşturma Azure Search nasıl kullanılacağı gösterilmektedir.

## <a name="set-up-azure-table-storage-indexing"></a>Azure Tablo depolama Dizin oluşturmayı ayarlama

Şu kaynakları kullanarak bir Azure Tablo depolama Dizin Oluşturucu oluşturabilirsiniz:

* [Azure portal](https://ms.portal.azure.com)
* Azure Search [REST API](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations)
* Azure Search [.NET SDK](https://aka.ms/search-sdk)

Burada, REST API kullanarak akışı gösteririz. 

### <a name="step-1-create-a-datasource"></a>1\. adım: Veritabanı oluşturma

Bir veri kaynağı, hangi verilerin dizine alınacağı, verilere erişmek için gereken kimlik bilgilerinin ve verilerin verilerdeki değişikliklerin etkili bir şekilde tanımlanmasını sağlayan ilkelerin Azure Search belirler.

Tablo dizini oluşturma için, veri kaynağı aşağıdaki özelliklere sahip olmalıdır:

- **ad** , arama hizmetinizin içindeki veri kaynağının benzersiz adıdır.
- **tür** olmalıdır `azuretable`.
- **kimlik bilgileri** parametresi, depolama hesabı bağlantı dizesini içerir. Ayrıntılar için [kimlik bilgilerini belirtin](#Credentials) bölümüne bakın.
- **kapsayıcı** , tablo adını ve isteğe bağlı bir sorguyu ayarlar.
    - `name` Parametresini kullanarak tablo adını belirtin.
    - İsteğe bağlı olarak, `query` parametresini kullanarak bir sorgu belirtin. 

> [!IMPORTANT] 
> Mümkün olduğunda, daha iyi performans için PartitionKey üzerinde bir filtre kullanın. Diğer herhangi bir sorgu tam tablo taraması yapar ve bu da büyük tablolar için düşük performansa neden olur. [Performans konuları](#Performance) bölümüne bakın.


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

DataSource API 'SI oluşturma hakkında daha fazla bilgi için bkz. [veri kaynağı oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-data-source).

<a name="Credentials"></a>
#### <a name="ways-to-specify-credentials"></a>Kimlik bilgilerini belirtme yolları ####

Aşağıdaki yollarla tablo için kimlik bilgilerini sağlayabilirsiniz: 

- **Tam erişimli depolama hesabı bağlantı dizesi**: `DefaultEndpointsProtocol=https;AccountName=<your storage account>;AccountKey=<your account key>`Azure Portal, **depolama hesabı dikey** > penceresi**ayarları** > **anahtarlarına** (klasik depolama hesapları için) veya **Ayarlar** > erişim anahtarlarına giderek bağlantı dizesini alabilirsiniz(Azure Resource Manager depolama hesapları için).
- **Depolama hesabı paylaşılan erişim imzası bağlantı dizesi**: `TableEndpoint=https://<your account>.table.core.windows.net/;SharedAccessSignature=?sv=2016-05-31&sig=<the signature>&spr=https&se=<the validity end time>&srt=co&ss=t&sp=rl`Paylaşılan erişim imzası, kapsayıcılar (Bu durumda tablolar) ve nesneler (tablo satırları) üzerinde liste ve okuma izinlerine sahip olmalıdır.
-  **Tablo paylaşılan erişim imzası**: `ContainerSharedAccessUri=https://<your storage account>.table.core.windows.net/<table name>?tn=<table name>&sv=2016-05-31&sig=<the signature>&se=<the validity end time>&sp=r`Paylaşılan erişim imzası tabloda sorgu (okuma) izinlerine sahip olmalıdır.

Depolama paylaşılan erişim imzaları hakkında daha fazla bilgi için bkz. [paylaşılan erişim Imzalarını kullanma](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

> [!NOTE]
> Paylaşılan erişim imzası kimlik bilgilerini kullanıyorsanız, bu veri kaynağı kimlik bilgilerini yenilenen imzalara göre düzenli aralıklarla güncelleştirmeniz gerekir. Paylaşılan erişim imzası kimlik bilgilerinin kullanım süreleri dolarsa, Dizin Oluşturucu, "bağlantı dizesinde girilen kimlik bilgileri geçersiz veya süreleri sona erdiğinde" aşağıdakine benzer bir hata iletisiyle başarısız olur.  

### <a name="step-2-create-an-index"></a>2\. adım: Dizin oluşturma
Dizin, bir belgedeki alanları, öznitelikleri ve arama deneyimini şekillendirieden diğer yapıları belirler.

Bir dizin oluşturmak için:

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

Dizinler oluşturma hakkında daha fazla bilgi için bkz. [Dizin oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-index).

### <a name="step-3-create-an-indexer"></a>3\. adım: Dizin oluşturucu oluşturun
Dizin Oluşturucu bir veri kaynağını hedef arama diziniyle bağlar ve veri yenilemeyi otomatikleştirmek için bir zamanlama sağlar. 

Dizin ve veri kaynağı oluşturulduktan sonra, Dizin oluşturucuyu oluşturmaya hazırsınız:

    POST https://[service name].search.windows.net/indexers?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "table-indexer",
      "dataSourceName" : "table-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }

Bu Dizin Oluşturucu her iki saatte bir çalışır. (Zamanlama aralığı "PT2H" olarak ayarlanır.) Her 30 dakikada bir dizin oluşturucu çalıştırmak için, aralığı "PT30M" olarak ayarlayın. Desteklenen en kısa Aralık beş dakikadır. Zamanlama isteğe bağlıdır; Atlanırsa, bir Dizin Oluşturucu oluşturulduğunda yalnızca bir kez çalışır. Ancak, bir dizin oluşturucuyu dilediğiniz zaman isteğe bağlı olarak çalıştırabilirsiniz.   

Dizin Oluşturucu oluşturma API 'SI hakkında daha fazla bilgi için bkz. [Dizin Oluşturucu oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

Dizin Oluşturucu zamanlamalarını tanımlama hakkında daha fazla bilgi için bkz. [Azure Search Dizin oluşturucuyu zamanlama](search-howto-schedule-indexers.md).

## <a name="deal-with-different-field-names"></a>Farklı alan adlarıyla dağıt
Bazen, var olan dizininizdeki alan adları, tablonuzdaki Özellik adlarından farklı olabilir. Alan eşlemelerini, tablodaki özellik adlarını, arama dizininizdeki alan adlarıyla eşlemek için kullanabilirsiniz. Alan eşlemeleri hakkında daha fazla bilgi için bkz. [Azure Search Dizin Oluşturucu alan eşlemeleri Köprüsü, veri kaynakları ve arama dizinleri arasındaki farkları köprüleyin](search-indexer-field-mappings.md).

## <a name="handle-document-keys"></a>Belge anahtarlarını işle
Azure Search, belge anahtarı bir belgeyi benzersiz şekilde tanımlar. Her arama dizininin türünde `Edm.String`tam olarak bir anahtar alanı olmalıdır. Dizine eklenmekte olan her belge için anahtar alanı gereklidir. (Aslında, tek gerekli alandır.)

Tablo satırları bileşik bir anahtara sahip olduğundan Azure Search, bölüm anahtarı ve satır anahtarı `Key` değerlerinin birleşimi olan adlı yapay bir alan oluşturur. Örneğin, `PK1` bir satırın partitionkey ve `RK1`rowkey `Key` ise alanın değeri olur `PK1RK1`.

> [!NOTE]
> `Key` Değer, çizgiler gibi belge anahtarlarında geçersiz olan karakterler içeriyor olabilir. `base64Encode` [Alan eşleme işlevini](search-indexer-field-mappings.md#base64EncodeFunction)kullanarak geçersiz karakterlerle işlem yapabilirsiniz. Bunu yaparsanız, arama gibi API çağrılarında belge anahtarlarını geçirirken de URL güvenli Base64 kodlaması kullanmayı unutmayın.
>
>

## <a name="incremental-indexing-and-deletion-detection"></a>Artımlı dizin oluşturma ve silme algılaması
Bir zamanlamaya göre çalıştırılacak bir tablo dizin oluşturucuyu ayarlarken, bir satırın `Timestamp` değeri tarafından belirlendiği şekilde yalnızca yeni veya güncelleştirilmiş satırları yeniden dizinleyebilirsiniz. Değişiklik algılama ilkesi belirtmeniz gerekmez. Artımlı dizin oluşturma sizin için otomatik olarak etkinleştirilir.

Belirli belgelerin dizinden kaldırılması gerektiğini belirtmek için, bir geçici silme stratejisi kullanabilirsiniz. Bir satırı silmek yerine, silindiğini göstermek için bir özellik ekleyin ve DataSource üzerinde bir geçici silme algılama ilkesi ayarlayın. Örneğin, aşağıdaki ilke, satırın değeri `IsDeleted` `"true"`olan bir özelliği varsa, bir satırın silindiğini kabul eder:

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

Varsayılan olarak, Azure Search aşağıdaki sorgu filtresini kullanır: `Timestamp >= HighWaterMarkValue`. Azure tablolarının `Timestamp` alan üzerinde ikincil bir dizini olmadığından, bu tür bir sorgu tam tablo taraması gerektirir ve bu nedenle büyük tablolar için yavaş olur.


Tablo dizinleme performansını iyileştirmeye yönelik iki olası yaklaşım aşağıda verilmiştir. Bu yaklaşımların her ikisi de tablo bölümlerinin kullanılmasına bağımlıdır: 

- Verileriniz doğal olarak çeşitli bölüm aralıklarında bölümlenebilir, her bölüm aralığı için bir veri kaynağı ve karşılık gelen bir Dizin Oluşturucu oluşturun. Her bir dizin oluşturucunun şimdi yalnızca belirli bir bölüm aralığını işlemesi gerekir ve bu, daha iyi sorgu performansına neden olur. Dizine alınması gereken verilerin az sayıda sabit bölümü varsa ve daha iyi: her bir Dizin Oluşturucu yalnızca bir bölüm taraması yapar. Örneğin, ile arasında `000` `100`anahtarlarla bir bölüm aralığını işlemeye yönelik bir veri kaynağı oluşturmak için, şöyle bir sorgu kullanın: 
    ```
    "container" : { "name" : "my-table", "query" : "PartitionKey ge '000' and PartitionKey lt '100' " }
    ```

- Verileriniz zamana göre bölümlense (örneğin, her gün veya hafta için yeni bir bölüm oluşturursanız) aşağıdaki yaklaşımı göz önünde bulundurun: 
    - Şu biçimde bir sorgu kullanın: `(PartitionKey ge <TimeStamp>) and (other filters)`. 
    - Dizin Oluşturucu [durum API 'sini](https://docs.microsoft.com/rest/api/searchservice/get-indexer-status)kullanarak dizin oluşturucunun ilerlemesini izleyin ve en son başarılı `<TimeStamp>` yüksek su işaretine göre sorgunun koşulunu düzenli olarak güncelleştirin. 
    - Bu yaklaşım sayesinde, bir yeniden dizin oluşturmayı tetiklemeniz gerekiyorsa, Dizin oluşturucuyu sıfırlamaya ek olarak DataSource sorgusunu sıfırlamanız gerekir. 


## <a name="help-us-make-azure-search-better"></a>Azure Search daha iyi hale getirmemize yardımcı olun
Geliştirmeler için özellik istekleriniz veya fikirler varsa, bunları [UserVoice sitemiz](https://feedback.azure.com/forums/263029-azure-search/)üzerinde gönderebilirsiniz.
