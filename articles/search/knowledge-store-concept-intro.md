---
title: Bilgi deposuna giriş (Önizleme)
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama ve diğer uygulamalarda zenginleştirilmiş belgeleri görüntüleyebileceğiniz, yeniden şekillendirbileceğiniz ve kullanabileceğiniz Azure depolama 'ya zenginleştirilmiş belgeler gönderin.
author: HeidiSteen
manager: nitinme
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 82f8606f4b4201833667347d3ed16fdd73f70a36
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72790364"
---
# <a name="introduction-to-knowledge-stores-in-azure-cognitive-search"></a>Azure Bilişsel Arama bilgi depolarına giriş

> [!Note]
> Bilgi deposu önizleme aşamasındadır ve üretim kullanımı için tasarlanmamıştır. [REST API sürüm 2019-05-06-önizleme](search-api-preview.md) bu özelliği sağlar. Şu anda .NET SDK desteği yok.
>

Bilgi deposu, daha sonraki analizler veya diğer aşağı akış işlemleri için bir [AI zenginleştirme ardışık düzeninde](cognitive-search-concept-intro.md) çıktıyı sürekli olarak sürdüren Azure bilişsel arama özelliğidir. *Zenginleştirilmiş bir belge* , yapay, yapılandırılmış ve AI süreçlerini kullanılarak çözümlenen içerikten oluşturulan bir ardışık düzen çıktıdır. Standart bir AI ardışık düzeninde, zenginleştirilmiş belgeler yalnızca dizin oluşturma sırasında kullanılır ve sonra atılır. Bilgi deposu ile zenginleştirilmiş belgeler korunur. 

Geçmişte Azure Bilişsel Arama bilişsel yetenekler kullandıysanız, *becerileri* bir belgeyi bir dizi şekilde taşımış olduğunu zaten anlarsınız. Sonuç bir bilgi deposundaki bir arama dizini veya (Bu önizlemede yeni) projeksiyonu olabilir. İki çıkış, arama dizini ve bilgi deposu, aynı içeriği paylaşır, ancak aynı şekilde depolanır ve çok farklı şekillerde kullanılır.

Azure [depolama](https://docs.microsoft.com/azure/storage/common/storage-account-overview), Azure Tablo depolama, Azure Blob depolama ya da her ikisi de fiziksel olarak bir bilgi deposu. Azure depolama 'ya bağlanabilecek herhangi bir araç veya işlem bilgi deposunun içeriğini kullanabilir.

![Ardışık düzen diyagramında bilgi deposu](./media/knowledge-store-concept-intro/knowledge-store-concept-intro.svg "Ardışık düzen diyagramında bilgi deposu")

Projeksiyonlar bir bilgi deposundaki verileri yapılandırmak için mekanizmadır. Örneğin, projeksiyonlar aracılığıyla çıktının tek bir blob olarak kaydedilip kaydedilmediğini veya ilgili tabloların bir koleksiyonunu seçebilirsiniz. 

Bilgi deposu kullanmak için, bir dizin oluşturma işlem hattındaki adım temelinde işlemleri tanımlayan bir beceri öğesine `knowledgeStore` öğesi ekleyin. Yürütme sırasında Azure Bilişsel Arama, Azure depolama hesabınızda bir alan oluşturur ve yapılandırmanıza bağlı olarak, belgeleri Bloblar veya tablolar halinde projeler halinde kaydeder.

## <a name="benefits-of-knowledge-store"></a>Bilgi deposunun avantajları

Bilgi deposu, yerleşik olarak yapılandırılmamış ve yarı yapılandırılmış veri dosyalarından, uygulanan Analize sahip görüntü dosyalarında veya hatta yeni formlara yeniden şekillenen yapılandırılmış verilere karşı bir yapı, bağlam ve gerçek içerik sağlar. Bu önizleme için yazılan [adım adım](knowledge-store-howto.md) kılavuzda, önce yoğun bir JSON belgesinin nasıl bölümlendiğini, yeni yapılara reconstituted, aksi takdirde makine gibi aşağı akış işlemlerinde nasıl kullanılabilir yapıldığını görebilirsiniz. öğrenme ve veri bilimi iş yükleri.

Bir AI zenginleştirme ardışık düzeninin ne işe yarayabileceği görmek faydalı olsa da, bilgi deposunun gerçek gücü verileri yeniden şekillendirmeye olanak tanır. Temel bir beceri ile başlayabilir ve daha sonra yeni yapılar halinde birleştirebileceğiniz, daha sonra Azure Bilişsel Arama yanı sıra diğer uygulamalarda kullanabileceğiniz daha fazla yapı düzeyleri eklemek için bunu yineleyebilirsiniz.

Numaralandırılmış, bilgi deposunun avantajları şunları içerir:

+ Arama dışındaki [analiz ve raporlama araçlarında](#tools-and-apps) zenginleştirilmiş belgeler kullanın. Power Query ile Power BI etkileyici bir seçimdir, ancak Azure depolama 'ya bağlanabilecek herhangi bir araç veya uygulama, oluşturduğunuz bir bilgi deposundan çekebilirsiniz.

+ Adımlarda ve beceri tanımlarında hata ayıklama sırasında bir AI dizin oluşturma işlem hattını daraltın. Bilgi deposu, bir AI dizin oluşturma işlem hattında bir beceri tanımının çarpımını gösterir. Bu sonuçları daha iyi bir beceri tasarlamak için kullanabilirsiniz, çünkü tam olarak neyin nasıl göründüğünü görebilirsiniz. Bir bilgi deposunun içeriğini görüntülemek için Azure Storage 'da [Depolama Gezgini](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows) kullanabilirsiniz.

+ Verileri yeni formlara şekillendirin. Yeniden şekillendirme becerileri içinde ortaklaşa bulunur, ancak nokta bir beceri artık bu özelliği sağlayabiliriz. Azure Bilişsel Arama 'de [beceri başına mil](cognitive-search-skill-shaper.md) bu görevi kapsayacak şekilde genişletildi. Yeniden şekillendirme, ilişkileri korurken verilerin amaçlanan kullanımı ile hizalanan bir projeksiyon tanımlamanızı sağlar.

> [!Note]
> Bilişsel hizmetler 'i kullanarak AI zenginleştirme hakkında bilgi sahibi değil misiniz? Azure Bilişsel Arama, görüntü dosyaları üzerinden optik karakter tanıma (OCR), metin dosyalarından varlık tanıma ve anahtar tümceciği ayıklama ve daha fazlasını kullanarak kaynak verileri ayıklamak ve zenginleştirmek için bilişsel hizmetler düzenleme ve dil özellikleriyle tümleşir. Daha fazla bilgi için bkz. [Azure bilişsel arama 'de AI zenginleştirme](cognitive-search-concept-intro.md).

## <a name="creating-a-knowledge-store"></a>Bilgi deposu oluşturma

Bilgi deposu, bir [dizin oluşturucunun](search-indexer-overview.md)parçası olan [beceri](cognitive-search-working-with-skillsets.md)bir parçasıdır. 

Bu önizlemede, REST API ve `api-version=2019-05-06-Preview` kullanarak ya da portalda **veri Içeri aktarma** Sihirbazı aracılığıyla bir bilgi deposu oluşturabilirsiniz.

### <a name="json-representation-of-a-knowledge-store"></a>Bilgi deposunun JSON temsili

Aşağıdaki JSON bir Dizin Oluşturucu tarafından çağrılan bir beceri parçası olan bir `knowledgeStore` belirtir (gösterilmez). AI zenginleştirme hakkında zaten bilgi sahibiyseniz, bir beceri, zenginleştirilmiş her belgenin oluşturulmasını, organizasyonunu ve akklığını belirler. Bir beceri, veri yapılarını modüle ediyorsanız en az bir yetenek, büyük olasılıkla yetenek başına bir yetenek içermelidir.

Bir `knowledgeStore` bağlantı ve projeksiyonlar oluşur. 

+ Bağlantı, Azure Search aynı bölgedeki bir depolama hesabıdır. 

+ Projeksiyonlar tablolar-nesneler çiftleridir. `Tables` Azure Tablo depolamada zenginleştirilmiş belgelerin fiziksel ifadesini tanımlayın. Azure Blob depolamada fiziksel nesneleri tanımlama `Objects`.

```json
{
  "name": "my-new-skillset",
  "description": "Example showing knowledgeStore placement in a skillset.",
  "skills":
  [
    {
    "@odata.type": "#Microsoft.Skills.Util.ShaperSkill",
    "context": "/document/content/phrases/*",
    "inputs": [
        {
        "name": "text",
        "source": "/document/content/phrases/*"
        },
        {
        "name": "sentiment",
        "source": "/document/content/phrases/*/sentiment"
        }
    ],
    "outputs": [
        {
        "name": "output",
        "targetName": "analyzedText"
        }
    ]
    },
  ],
  "cognitiveServices": 
    {
    "@odata.type": "#Microsoft.Azure.Search.CognitiveServicesByKey",
    "description": "mycogsvcs resource in West US 2",
    "key": "<your key goes here>"
    },
  "knowledgeStore": { 
    "storageConnectionString": "<your connection string goes here>", 
    "projections": [ 
        { 
            "tables": [  
            { "tableName": "Reviews", "generatedKeyName": "ReviewId", "source": "/document/Review" , "sourceContext": null, "inputs": []}, 
            { "tableName": "Sentences", "generatedKeyName": "SentenceId", "source": "/document/Review/Sentences/*", "sourceContext": null, "inputs": []}, 
            { "tableName": "KeyPhrases", "generatedKeyName": "KeyPhraseId", "source": "/document/Review/Sentences/*/KeyPhrases", "sourceContext": null, "inputs": []}, 
            { "tableName": "Entities", "generatedKeyName": "EntityId", "source": "/document/Review/Sentences/*/Entities/*" ,"sourceContext": null, "inputs": []} 

            ], 
            "objects": [ 
               
            ]      
        },
        { 
            "tables": [ 
            ], 
            "objects": [ 
                { 
                "storageContainer": "Reviews", 
                "format": "json", 
                "source": "/document/Review", 
                "key": "/document/Review/Id" 
                } 
            ]      
        }        
    ]     
    } 
}
```

### <a name="sources-of-data-for-a-knowledge-store"></a>Bilgi deposu için veri kaynakları

Bilgi deposu bir AI zenginleştirme işlem hattının çıktımıdır, girişler nelerdir? Bir bilgi deposuna ayıklamak, zenginleştirmek ve son olarak kaydetmek istediğiniz özgün veriler, arama Dizin oluşturucular tarafından desteklenen herhangi bir Azure veri kaynağından kaynaklanabilmelidir: 

* [Azure Cosmos DB](search-howto-index-cosmosdb.md)

* [Azure Blob Depolama](search-howto-indexing-azure-blob-storage.md)

* [Azure Tablo Depolama](search-howto-indexing-azure-tables.md)

* [Azure SQL](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)

Oluşturduğunuz Dizin oluşturucular ve becerileri, bu içeriği bir dizin oluşturma iş yükünün parçası olarak ayıkladıktan ve zenginleştirerek, sonra sonuçları bir bilgi deposuna kaydeder.

### <a name="rest-apis-used-in-creation-of-a-knowledge-store"></a>Bilgi deposu oluştururken kullanılan REST API 'Leri

Yalnızca iki API, bilgi deposu oluşturmak için gereken uzantılara sahiptir (beceri oluşturun ve Dizin Oluşturucu oluşturun). Diğer API 'Ler olduğu gibi kullanılır.

| Nesne | REST API | Açıklama |
|--------|----------|-------------|
| Veri kaynağı | [Veri Kaynağı Oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-data-source)  | Zenginleştirilmiş belgeler oluşturmak için kullanılan kaynak verileri sağlayan bir dış Azure veri kaynağını tanımlayan kaynak.  |
| Beceri | [Beceri oluşturma (api-Version = 2019-05 -06-Preview)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)  | Dizin oluşturma sırasında bir zenginleştirme ardışık düzeninde kullanılan [yerleşik yeteneklerin](cognitive-search-predefined-skills.md) ve özel bilişsel [yeteneklerin](cognitive-search-custom-skill-interface.md) kullanımını koordine eden bir kaynak. Bir beceri, alt öğe olarak `knowledgeStore` tanımına sahiptir. |
| indeks | [Dizin Oluştur](https://docs.microsoft.com/rest/api/searchservice/create-index)  | Arama dizinini ifade eden bir şema. Dizindeki alanlar, kaynak verilerdeki alanlara veya zenginleştirme aşamasında üretilen alanlara (örneğin, varlık tanıma tarafından oluşturulan kuruluş adları için bir alan) eşlenir. |
| dizinleyic | [Dizin Oluşturucu oluştur (api-Version = 2019-05-06)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)  | Dizin oluşturma sırasında kullanılan bileşenleri tanımlayan kaynak: veri kaynağı, Beceri, kaynak ve ara veri yapılarından hedef dizine ve dizinin kendisi de dahil olmak üzere. Dizin oluşturucuyu çalıştırmak, veri alımı ve zenginleştirme için tetikleyiciydi. Çıktı, kaynak verilerle doldurulan, becerileri aracılığıyla zenginleştirilmiş, Dizin şemasını temel alan bir arama dizinidir.  |

### <a name="physical-composition-of-a-knowledge-store"></a>Bilgi deposunu fiziksel olarak oluşturma

 Bir `knowledgeStore` tanımının bir öğesi olan *projeksiyon*, hedeflenen kullanım ile eşleşecek şekilde çıktının şemasını ve yapısını ifade eder. Farklı biçimlerdeki ve şekillerdeki verileri kullanan uygulamalarınız varsa, birden çok projeksiyonu tanımlayabilirsiniz. 

Projeksiyonlar nesne veya tablo olarak ifade edilebilir:

+ Bir nesne olarak, projeksiyon BLOB depolama alanına eşlenir; burada projeksiyon, bir veri bilimi işlem hattı gibi senaryolar için JSON 'daki nesneler veya hiyerarşik gösterimler olan bir kapsayıcıya kaydedilir.

+ Tablo olarak, projeksiyon tablo depolamasına eşlenir. Tablo temsili, veri analizi veya makine öğrenimi için veri çerçeveleri olarak dışa aktarma gibi senaryolar için ilişkileri korur. Zenginleştirilmiş projeksiyonlar daha sonra diğer veri depolarına kolayca aktarılabilir. 

Bir bilgi deposunda, kuruluşunuzda çeşitli yapısal olarak uyum sağlamak için birden çok projeksiyonlar oluşturabilirsiniz. Geliştirici, zenginleştirilmiş bir belgenin tam JSON gösterimine erişim gerektirebilir, ancak veri bilimcileri veya analistleri, Beceri göre şekillendirilmiş veya modüler veri yapıları isteyebilir.

Örneğin, zenginleştirme işleminin amaçlarından biri de bir modeli eğitmek için kullanılan bir veri kümesi oluşturacaksa, verileri nesne deposuna yansıtma, veri bilimi işlem hatlarınızın verilerini kullanmanın bir yoludur. Alternatif olarak, tablo projeksiyonunu zenginleştirilmiş belgeler temelinde hızlı bir Power BI panosu oluşturmak istiyorsanız tablo projeksiyonu iyi çalışacaktır.

<a name="tools-and-apps"></a>

## <a name="connecting-with-tools-and-apps"></a>Araçlar ve uygulamalarla bağlanma

Depolarda enzenginler varsa, Azure Blob veya tablo depolama 'ya bağlanan herhangi bir araç veya teknoloji, içeriği incelemek, çözümlemek veya kullanmak için kullanılabilir. Aşağıdaki liste bir başlangıç:

+ Zenginleştirilmiş belge yapısını ve içeriğini görüntülemek için [Depolama Gezgini](knowledge-store-view-storage-explorer.md) . Bilgi deposu içeriğini görüntülemek için bunu taban çizgisi aracınız olarak düşünün.

+ Sayısal veriniz varsa raporlama ve analiz araçları için [Power BI](knowledge-store-connect-power-bi.md) .

+ Daha fazla düzenleme için [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/) .


<!---
## Data lifecycle and billing

Each time you run the indexer, the cache in Azure storage is updated if the skillset definition or underlying source data has changed. As input documents are edited or deleted, changes are propagated through the annotation cache to the projections, ensuring that your projected data is a current representation of your inputs at the end of the indexer run. 

Generally speaking, pipeline processing can be an all-or-nothing operation, but Azure Search can process incremental changes, which saves you time and money.

If a document is new or updated, all skills are run. If only the skillset changes, reprocessing is scoped to just those skills and documents affected by your edit.

### Changes to a skillset
Suppose that you have a pipeline composed of multiple skills, operating over a large body of static data (for example, scanned documents), that takes 8 hours and costs $200 to create the knowledge store. Now suppose you need to tweak one of the skills in the skillset. Rather than starting over, Azure Search can determine which skill is affected, and reprocess only that skill. Cached data and projections that are unaffected by the change remain intact in the knowledge store.

### Changes in the data
Scenarios can vary considerably, but let's suppose instead of static data, you have volatile data that changes between indexer invocations. Given no changes to the skillset, you are charged for processing the delta of new and modified document. The timestamp information varies by data source, but for illustration, in a Blob container, Azure Search looks at the `lastmodified` date to determine which blobs need to be ingested.

> [!Note]
> While you can edit the data in the projections, any edits will be overwritten on the next pipeline invocation, assuming the document in source data is updated. 

### Deletions

Although Azure Search creates and updates structures and content in Azure storage, it does not delete them. Projections and cached documents continue to exist even when the skillset is deleted. As the owner of the storage account, you should delete a projection if it is no longer needed. 

### Tips for development

+ Start small with a representative sample of your data as you make significant changes to skillset composition. As your design finalizes, you can slowly add more data during later-stage development, and then roll in the entire data set when you are comfortable with the pipeline composition.

+ Retain control over indexer invocation. Indexers can run on a schedule, which is helpful for solutions that are rolled into production, but less helpful if you are actively developing your pipeline. During development, avoid schedules so that you don’t lose track of cache or projection state. Once your solution is in production and skillset composition is static, you can put the indexer on a schedule to pick up routine changes in the external source data. 

-->

<!-- ## Where do I start?

We recommend the Free service for learning purposes, but be aware that the number of free transactions is limited to 20 documents per day, per subscription.

When using multiple services, create all of your services in the same region for best performance and to minimize costs. You are not charged for bandwidth for inbound data or outbound data that goes to another service in the same region.

**Step 1: [Create an Azure Cognitive Search resource](search-create-service-portal.md)** 

**Step 2: [Create an Azure storage account](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal)** 

**Step 3: [Create a Cognitive Services resource](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)** 

**Step 4: [Get started with the portal](cognitive-search-quickstart-blob.md) - or - [Get started with sample data using REST and Postman](knowledge-store-howto.md)** 

You can use REST `api-version=2019-05-06-Preview` to construct an AI-based pipeline that includes knowledge store. In the newest preview API, the Skillset object provides the `knowledgeStore` definition. -->

## <a name="next-steps"></a>Sonraki adımlar

Bilgi deposu, bir Azure depolama hesabına erişim özellikli herhangi bir istemci uygulaması tarafından tüketimine yönelik bir beceri tasarımı veya yeni yapıların oluşturulması için yararlı olan belgelerin kalıcılığını, yararlı bir şekilde veya yeni yapıların oluşturulmasını sağlar.

Zenginleştirilmiş belgeler oluşturmaya yönelik en basit yaklaşım, **veri alma** Sihirbazı ' nı kullanmaktır, ancak nesnelerin nasıl oluşturulduğuna ve başvurulduğunu öğrenmek isterseniz daha kullanışlı olan Postman ve REST API de kullanabilirsiniz.

> [!div class="nextstepaction"]
> [Portalı kullanarak bir bilgi deposu oluşturun](knowledge-store-create-portal.md) 
> [POSTMAN ve REST API kullanarak bilgi deposu oluşturma](knowledge-store-create-rest.md)
