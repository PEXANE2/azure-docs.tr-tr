---
title: Azure Cosmos DB'de (SQL API) verileri kopyalama ve dönüştürme
description: Veri Fabrikası'nı kullanarak Azure Cosmos DB'ye (SQL API) verileri nasıl kopyalayıp dönüştürecekve Azure Cosmos DB'deki (SQL API) verileri nasıl dönüştüreceklerini öğrenin.
services: data-factory, cosmosdb
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: multiple
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 12/11/2019
ms.openlocfilehash: 7096b429145a54b5a09fe38eb8099c4ff24ac452
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79243620"
---
# <a name="copy-and-transform-data-in-azure-cosmos-db-sql-api-by-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak Azure Cosmos DB'deki (SQL API) verileri kopyalama ve dönüştürme

> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-azure-documentdb-connector.md)
> * [Geçerli sürüm](connector-azure-cosmos-db.md)

Bu makalede Azure Cosmos DB’den (SQL API) ve Azure Cosmos DB'ye veri kopyalamak için Azure Data Factory’nin Kopya Etkinliğini kullanma ve Azure Cosmos DB’deki (SQL API) verileri dönüştürmek için Veri Akışı’nı kullanma işlemleri açıklanır. Azure Veri Fabrikası hakkında bilgi edinmek için [giriş makalesini](introduction.md)okuyun.

>[!NOTE]
>Bu bağlayıcı yalnızca Cosmos DB SQL API'yi destekler. MongoDB API için Azure [Cosmos DB'nin MongoDB için API'si için bağlayıcıya](connector-azure-cosmos-db-mongodb-api.md)bakın. Diğer API türleri artık desteklenmiyor.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Azure Cosmos DB (SQL API) bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Veri akışını eşleme](concepts-data-flow-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)

Kopyalama etkinliği için bu Azure Cosmos DB (SQL API) bağlayıcısı aşağıdakileri destekler:

- Verileri Azure Cosmos DB [SQL API'den](https://docs.microsoft.com/azure/cosmos-db/documentdb-introduction)kopyalayın.
- Azure Cosmos DB'ye **ekleme** veya **yükseltme**olarak yazın.
- JSON belgelerini olduğu gibi içe aktarın ve dışa aktarın veya verileri bir tabular veri kümesinden kopyalayın veya kopyalayın. Örnekler arasında bir SQL veritabanı ve csv dosyası yer almaktadır. Belgeleri JSON dosyalarına veya başka bir Azure Cosmos DB koleksiyonundan veya başka bir Azure Cosmos DB koleksiyonundan olduğu gibi kopyalamak için [JSON belgelerini içe aktarma ve dışa aktarma bilgisine](#import-and-export-json-documents)bakın.

Veri Fabrikası, Azure Cosmos DB'ye yazarken en iyi performansı sağlamak için [Azure Cosmos DB toplu yürütme kitaplığıyla](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started) tümleşir.

> [!TIP]
> [Veri Geçişi videosu,](https://youtu.be/5-SRNiC_qOU) Azure Blob depolamadan Azure Cosmos DB'ye veri kopyalama adımlarında size yol sunar. Videoda, genel olarak Azure Cosmos DB'ye veri sindirilmesi için performans ayarı hususları da açıklanmaktadır.

## <a name="get-started"></a>Kullanmaya başlayın

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, Azure Cosmos DB'ye (SQL API) özgü Veri Fabrikası varlıklarını tanımlamak için kullanabileceğiniz özellikler hakkında ayrıntılı bilgi verilir.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Azure Cosmos DB (SQL API) bağlantılı hizmet için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Tür** özelliği **CosmosDb**olarak ayarlanmalıdır. | Evet |
| Connectionstring |Azure Cosmos DB veritabanına bağlanmak için gereken bilgileri belirtin.<br />**Not**: Aşağıdaki örneklerde gösterildiği gibi bağlantı dizesinde veritabanı bilgilerini belirtmeniz gerekir. <br/> Hesap anahtarını Azure Key Vault'a `accountKey` koyabilir ve yapılandırmayı bağlantı dizesinin dışına çekebilirsiniz. Azure Key Vault [makalesinde](store-credentials-in-key-vault.md) daha fazla ayrıntı içeren aşağıdaki örneklere ve Mağaza kimlik bilgilerine bakın. |Evet |
| connectVia | Veri deposuna bağlanmak için kullanılacak [Tümleştirme Çalışma Süresi.](concepts-integration-runtime.md) Azure Tümleştirme Çalışma Zamanı'nı veya kendi kendine barındırılan tümleştirme çalışma zamanını (veri deponuz özel bir ağda bulunuyorsa) kullanabilirsiniz. Bu özellik belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresi kullanılır. |Hayır |

**Örnek**

```json
{
    "name": "CosmosDbSQLAPILinkedService",
    "properties": {
        "type": "CosmosDb",
        "typeProperties": {
            "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek: Azure Key Vault'ta mağaza hesabı anahtarı**

```json
{
    "name": "CosmosDbSQLAPILinkedService",
    "properties": {
        "type": "CosmosDb",
        "typeProperties": {
            "connectionString": "AccountEndpoint=<EndpointUrl>;Database=<Database>",
            "accountKey": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [Bkz. Veri Kümeleri ve bağlantılı hizmetler.](concepts-datasets-linked-services.md)

Azure Cosmos DB (SQL API) veri kümesi için aşağıdaki özellikler desteklenir: 

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin **tür** özelliği **CosmosDbSqlApiCollection**olarak ayarlanmalıdır. |Evet |
| Toplamaadı |Azure Cosmos DB belge koleksiyonunun adı. |Evet |

"DocumentDbCollection" türü veri kümesini kullanıyorsanız, Kopyala ve Arama etkinliği için geriye dönük uyumluluk için olduğu gibi desteklenir, Veri Akışı için desteklenmez. İleriye dönük yeni modeli kullanmanız önerilir.

**Örnek**

```json
{
    "name": "CosmosDbSQLAPIDataset",
    "properties": {
        "type": "CosmosDbSqlApiCollection",
        "linkedServiceName":{
            "referenceName": "<Azure Cosmos DB linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [],
        "typeProperties": {
            "collectionName": "<collection name>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Etkinlik özelliklerini kopyalama

Bu bölümde, Azure Cosmos DB (SQL API) kaynağı ve lavabo desteği özelliklerinin bir listesi yer almaktadır. Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [bkz.](concepts-pipelines-activities.md)

### <a name="azure-cosmos-db-sql-api-as-source"></a>Kaynak olarak Azure Cosmos DB (SQL API)

Azure Cosmos DB'deki (SQL API) verileri kopyalamak **için,** Kopyalama Etkinliği'ndeki kaynak türünü **DocumentDbCollectionSource**olarak ayarlayın. 

Aşağıdaki özellikler Kopyalama Etkinliği **kaynak** bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının **tür** özelliği **CosmosDbSqlApiSource**olarak ayarlanmalıdır. |Evet |
| sorgu |Verileri okumak için Azure Cosmos DB sorgusunu belirtin.<br/><br/>Örnek:<br /> `SELECT c.BusinessEntityID, c.Name.First AS FirstName, c.Name.Middle AS MiddleName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` |Hayır <br/><br/>Belirtilmemişse, bu SQL deyimi yürütülür:`select <columns defined in structure> from mycollection` |
| tercih Edilen Bölgeler | Cosmos DB'den veri alırken bağlanılabilmek için tercih edilen bölgeler listesi. | Hayır |
| Pagesize | Sorgu sonucunun sayfa başına belge sayısı. Varsayılan değer "-1" anlamına gelir, yani hizmet tarafı dinamik sayfa boyutunu 1000'e kadar kullanır. | Hayır |

"DocumentDbCollectionSource" türü kaynağı kullanıyorsanız, geriye dönük uyumluluk için olduğu gibi desteklenir. Cosmos DB'den verileri kopyalamak için daha zengin yetenekler sağlayan yeni modeli kullanmanız önerilir.

**Örnek**

```json
"activities":[
    {
        "name": "CopyFromCosmosDBSQLAPI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Cosmos DB SQL API input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "CosmosDbSqlApiSource",
                "query": "SELECT c.BusinessEntityID, c.Name.First AS FirstName, c.Name.Middle AS MiddleName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"",
                "preferredRegions": [
                    "East US"
                ]
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

Cosmos DB'den veri kopyaladığında, [JSON belgelerini olduğu gibi dışa](#import-and-export-json-documents)aktarmak istemiyorsanız, en iyi yöntem, kopyalama etkinliğinde eşlemeyi belirtmektir. Veri Fabrikası, etkinlikte belirttiğiniz eşlemeyi onurlandırAr - bir satır sütun için bir değer içermiyorsa, sütun değeri için null bir değer sağlanır. Bir eşleme belirtmezseniz, Veri Fabrikası verilerdeki ilk satırı kullanarak şemayı çıkartır. İlk satır tam şema içermiyorsa, etkinlik çalışması nın sonucu olarak bazı sütunlar eksik olur.

### <a name="azure-cosmos-db-sql-api-as-sink"></a>Azure Cosmos DB (SQL API) lavabo olarak

Verileri Azure Cosmos DB'ye (SQL API) kopyalamak için, Kopyalama Etkinliği'ndeki **lavabo** türünü **DocumentDbCollectionSink**olarak ayarlayın. 

Aşağıdaki özellikler Kopyalama Etkinliği **kaynak** bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama Etkinliği lavabonun **türü** özelliği **CosmosDbSqlApiSink**olarak ayarlanmalıdır. |Evet |
| yazmaDavranışı |Azure Cosmos DB'ye nasıl veri yazılabildiğini açıklar. İzin verilen değerler: ekle ve **yukarı sa.** **insert**<br/><br/>Aynı kİmLiğe sahip bir belge zaten **varsa, yukarı seeklemek'in** davranışı belgenin değiştirilmesidir; aksi takdirde, belgeyi ekleyin.<br /><br />**Not**: Kimlik özgün belgede veya sütun eşlemesinde belirtilmemişse, Veri Fabrikası belge için otomatik olarak bir kimlik oluşturur. Bu, **yükseltmenin** beklendiği gibi çalışması için belgenizin bir kimliği olduğundan emin olmak gerektiği anlamına gelir. |Hayır<br />(varsayılan **insert)** |
| yazmaBatchSize | Veri Fabrikası, Azure Cosmos DB'ye veri yazmak için [Azure Cosmos DB toplu yürütme kitaplığını](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started) kullanır. **writeBatchSize** özelliği, ADF'nin kitaplıkiçin sağladığı belgelerin boyutunu denetler. Performansı artırmak için **writeBatchSize** değerini artırmayı ve belge boyutunuz büyükse değeri azaltmayı deneyebilirsiniz - aşağıdaki ipuçlarına bakın. |Hayır<br />(varsayılan **değer 10.000'dir)** |
| devre dışı KalımMetricsCollection | Veri Fabrikası, kopya performans optimizasyonu ve öneriler için Cosmos DB RUs gibi ölçümler toplar. Bu davranışla ilgileniyorsanız, `true` kapatmak için belirtin. | Hayır (varsayılan) `false` |

>[!TIP]
>JSON belgelerini olduğu gibi almak için, [JSON belgelerini içe aktarma veya dışa aktarma](#import-and-export-json-documents) bölümüne bakın; tabular şekilli verilerden kopyalamak [için, İlişkisel veritabanından Cosmos DB'ye geçir'e](#migrate-from-relational-database-to-cosmos-db)bakın.

>[!TIP]
>Cosmos DB, tek isteğin boyutunu 2MB ile sınırlar. Formül İstek Boyutu = Tek Belge Boyutu * Toplu Boyut Yaz. **"İstek boyutu çok büyüktür."** diyerek hataya uğrarsa, kopya lavabo yapılandırmasındaki ** `writeBatchSize` değeri azaltın.**

"DocumentDbCollectionSink" türü kaynağı kullanıyorsanız, geriye dönük uyumluluk için olduğu gibi yine de desteklenir. Cosmos DB'den verileri kopyalamak için daha zengin yetenekler sağlayan yeni modeli kullanmanız önerilir.

**Örnek**

```json
"activities":[
    {
        "name": "CopyToCosmosDBSQLAPI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Document DB output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "CosmosDbSqlApiSink",
                "writeBehavior": "upsert"
            }
        }
    }
]
```

### <a name="schema-mapping"></a>Şema haritalama

Azure Cosmos DB'deki verileri tabular lavaboya veya ters temaya kopyalamak için [şema eşleciliği'ne](copy-activity-schema-and-type-mapping.md#schema-mapping)bakın.

## <a name="mapping-data-flow-properties"></a>Veri akışı özelliklerini eşleme

Veri akışını eşlemede verileri dönüştürürken, Cosmos DB'deki koleksiyonlara okuma ve yazma yapabilirsiniz. Daha fazla bilgi için, veri akışlarını eşlemedeki [kaynak dönüşümü](data-flow-source.md) ve [lavabo dönüşümüne](data-flow-sink.md) bakın.

### <a name="source-transformation"></a>Kaynak dönüşümü

Azure Cosmos DB'ye özgü ayarlar kaynak dönüştürmenin **Kaynak Seçenekleri** sekmesinde kullanılabilir. 

**Sistem sütunları ekleyin:** Doğruysa, ```id```, ```_ts```ve diğer sistem sütunları CosmosDB'den veri akışı meta verilerinize dahil edilir. Koleksiyonları güncellerken, varolan satır kimliğini yakalayabilmeniz için bunu eklemek önemlidir.

**Sayfa boyutu:** Sorgu sonucunun sayfa başına belge sayısı. Varsayılan, 1000'e kadar hizmet dinamik sayfasını kullanan "-1"dir.

**Elde iş sayısı:** Okuma işlemi sırasında bu veri akışının her yürütülmesi için CosmosDB koleksiyonunuza uygulamak istediğiniz RUs sayısı için isteğe bağlı bir değer ayarlayın. En az 400.

**Tercih edilen bölgeler:** Bu işlem için tercih edilen okuma bölgelerini seçin.

#### <a name="json-settings"></a>JSON Ayarları

**Tek belge:** ADF tüm dosyayı tek bir JSON dokümanı olarak ele almak için bu seçeneği belirleyin.

**Tırnak içinde sütun adları:** JSON'daki sütun adları alıntılanmazsa bu seçeneği belirleyin.

**Yorumları var:** JSON belgelerinizin verilerde yorumları varsa bu seçimi kullanın.

**Tek alıntı:** Belgenizdeki sütunlar ve değerler tek tırnak la birlikte alıntılanırsa bu seçilmelidir.

**Backslash kaçtı:** JSON'unuzdaki karakterlerden kaçmak için ters eğik çizgiler kullanıyorsanız, bu seçeneği belirleyin.

### <a name="sink-transformation"></a>Lavabo dönüşümü

Azure Cosmos DB'ye özgü ayarlar, lavabo dönüşümünün **Ayarlar** sekmesinde kullanılabilir.

**Güncelleme yöntemi:** Veritabanı hedefinizde hangi işlemlere izin verileni belirler. Varsayılan yalnızca eklere izin vermektir. Satırları güncelleştirmek, yükseltmek veya silmek için, bu eylemler için satırları etiketlemek için bir alter-row dönüşümü gerekir. Güncelleştirmeler, ekler ve silmeler için, hangi satırı değiştireceğini belirlemek için bir anahtar sütunu veya sütun ayarlanmalıdır.

**Toplama eylemi:** Yazmadan önce hedef koleksiyonun yeniden oluşturulup oluşturulmayacağını belirler.
* Yok: Koleksiyona hiçbir işlem yapılmaz.
* Yeniden oluşturma: Koleksiyon bırakılacak ve yeniden oluşturulacak

**Toplu iş boyutu**: Her kovaya kaç satır yazıldığını denetler. Daha büyük toplu iş boyutları sıkıştırma ve bellek optimizasyonunu artırır, ancak verileri önbelleğe alırken bellek özel durumları riski.

**Bölüm Anahtarı:** Koleksiyonunuz için bölüm anahtarını temsil eden bir dize girin. Örnek: ```/movies/title```

**Elde iş sayısı:** Bu veri akışının her yürütülmesi için CosmosDB koleksiyonunuza uygulamak istediğiniz RUs sayısı için isteğe bağlı bir değer ayarlayın. En az 400.

**Elde iş bütçesi yazın:** Toplu yutma Spark işine ayırmak istediğiniz RUs sayısını temsil eden bir karşılayıcı. Bu sayı, koleksiyona ayrılan toplam iş kaynağının dışındadır.

## <a name="lookup-activity-properties"></a>Arama etkinlik özellikleri

Özellikler hakkında daha fazla bilgi edinmek için [Arama etkinliğini](control-flow-lookup-activity.md)kontrol edin.

## <a name="import-and-export-json-documents"></a>JSON belgelerini alma ve dışa aktarma

Bu Azure Cosmos DB (SQL API) bağlayıcısını kolayca kullanabilirsiniz:

* Belgeleri olduğu gibi iki Azure Cosmos DB koleksiyonu arasında kopyalayın.
* JSON belgelerini Azure Blob depolama, Azure Veri Gölü Deposu ve Azure Veri Fabrikası'nın desteklediği diğer dosya tabanlı mağazalar da dahil olmak üzere çeşitli kaynaklardan Azure Cosmos DB'ye aktarın.
* JSON belgelerini Azure Cosmos DB koleksiyonundan çeşitli dosya tabanlı mağazalara dışa aktarın.

Şema-agnostik kopya elde etmek için:

* Verileri Kopyala aracını kullandığınızda, **JSON dosyalarına veya Cosmos DB koleksiyonu seçeneğine olduğu gibi dışa** aktar'ı seçin.
* Etkinlik yazma yı kullandığınızda, kaynak veya lavabo için ilgili dosya deposuyla JSON biçimini seçin.

## <a name="migrate-from-relational-database-to-cosmos-db"></a>İlişkisel veritabanından Cosmos DB'ye geçirin

SQL Server gibi ilişkisel bir veritabanından Azure Cosmos DB'ye geçiş yaparken, kopyalama etkinliği, cosmos DB'deki JSON belgelerini düzleştirmek için kaynaktan gelen tabular verileri kolayca eşleyebilir. Bazı durumlarda, veri modelini Azure [Cosmos DB'deki Veri modellemesine](../cosmos-db/modeling-data.md)göre NoSQL kullanım örnekleri için optimize etmek için yeniden tasarlamak isteyebilirsiniz , örneğin, ilgili alt öğelerin tümlerini tek bir JSON belgesine katıştırarak verilerin normalinden arındırMak için. Bu gibi durumda, Azure Veri Fabrikası kopyalama etkinliğini kullanarak bu makalenin nasıl elde edilene ilişkin bir iz bilgili olarak bu [makaleye](../cosmos-db/migrate-relational-to-cosmos-db-sql-api.md) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Azure Veri Fabrikası'nda Copy Activity'in kaynak ve lavabo olarak desteklediği veri depolarının listesi için [desteklenen veri depolarına](copy-activity-overview.md#supported-data-stores-and-formats)bakın.
