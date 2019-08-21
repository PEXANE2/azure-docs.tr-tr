---
title: Bilgi deposuna giriş (Önizleme)-Azure Search
description: Azure Search ve diğer uygulamalarda zenginleştirilmiş belgeleri görüntüleyebileceğiniz, yeniden şekillendirbileceğiniz ve kullanabileceğiniz Azure depolama 'ya zenginleştirilmiş belgeler gönderin.
manager: nitinme
author: HeidiSteen
services: search
ms.service: search
ms.subservice: cognitive-search
ms.topic: overview
ms.date: 08/02/2019
ms.author: heidist
ms.openlocfilehash: f4308cf0309725fc0ba3b5feb047d04af2ebbe66
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69638193"
---
# <a name="what-is-knowledge-store-in-azure-search"></a>Azure Search bilgi deposu nedir?

> [!Note]
> Bilgi deposu önizleme aşamasındadır ve üretim kullanımı için tasarlanmamıştır. [REST API sürüm 2019-05-06-önizleme](search-api-preview.md) bu özelliği sağlar. Şu anda .NET SDK desteği yok.
>

Bilgi deposu, bir AI tabanlı dizin oluşturma işlem hattı (bilişsel [arama)](cognitive-search-concept-intro.md)tarafından oluşturulan zenginleştirilmiş belgeleri ve meta verileri kaydeden bir Azure Search özelliğidir. Zenginleştirilmiş bir belge, bilişsel hizmetler 'deki kaynaklar kullanılarak ayıklanan, yapılandırılmış ve çözümlenen içerikten oluşturulan bir ardışık düzen çıktıdır. Standart bir AI tabanlı işlem hattında, zenginleştirilmiş belgeler yalnızca dizin oluşturma sırasında kullanılır ve sonra atılır. Bilgi deposu ile, belgeler sonraki değerlendirme, araştırma için kaydedilir ve muhtemelen bir aşağı akış veri bilimi iş yüküne giriş haline gelebilir. 

Daha önce bilişsel arama 'yı kullandıysanız, bir belgeyi bir veya daha çok zenginleştirme ile taşımak için becerileri kullanıldığını zaten anlarsınız. Sonuç bir bilgi deposundaki Azure Search bir dizin veya (Bu önizlemede yeni) projeksiyonu olabilir. İki çıkış, arama dizini ve bilgi deposu birbirinden fiziksel olarak birbirinden farklıdır. Aynı içeriği paylaşır, ancak çok farklı yollarla depolanır ve kullanılır.

Fiziksel olarak, bir bilgi deposu, işlem hattını nasıl yapılandırdığınıza bağlı olarak Azure Tablo depolama veya blob depolama olarak Azure depolama hesabında oluşturulur. Azure depolama 'ya bağlanabilecek herhangi bir araç veya işlem bilgi deposunun içeriğini kullanabilir.

Projeksiyonlar bir bilgi deposundaki verileri yapılandırmak için mekanizmadır. Örneğin, projeksiyonlar aracılığıyla çıktının tek bir blob olarak kaydedilip kaydedilmediğini veya ilgili tabloların bir koleksiyonunu seçebilirsiniz. Bilgi deposu içeriğini görüntülemenin kolay bir yolu, Azure depolama için yerleşik [Depolama Gezgini](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows) kullanmaktır.

![Ardışık düzen diyagramında bilgi deposu](./media/knowledge-store-concept-intro/annotationstore_sans_internalcache.png "Ardışık düzen diyagramında bilgi deposu")

Bilgi deposu kullanmak için, bir dizin `knowledgeStore` oluşturma işlem hattında adım adım işlemleri tanımlayan bir beceri öğesine bir öğe ekleyin. Yürütme sırasında Azure Search, Azure depolama hesabınızda bir boşluk oluşturur ve işlem hattı tarafından oluşturulan tanımlar ve içerikle doldurulur.

## <a name="benefits-of-knowledge-store"></a>Bilgi deposunun avantajları

Bilgi deposu, yerleşik olarak yapılandırılmamış ve yarı yapılandırılmış veri dosyalarından, uygulanan Analize sahip görüntü dosyalarında veya hatta yeni formlara yeniden şekillenen yapılandırılmış verilere karşı bir yapı, bağlam ve gerçek içerik sağlar. Bu önizleme için yazılan [adım adım](knowledge-store-howto.md) kılavuzda, önce yoğun bir JSON belgesinin nasıl bölümlendiğini, yeni yapılara reconstituted, aksi takdirde makine gibi aşağı akış işlemlerinde nasıl kullanılabilir yapıldığını görebilirsiniz. öğrenme ve veri bilimi iş yükleri.

Bir AI tabanlı dizin oluşturma işlem hattının ne işe yarayabileceği görmek faydalı olsa da, bilgi deposunun gerçek gücü verileri yeniden şekilleyebilme özelliğidir. Temel bir beceri ile başlayabilir ve ardından, daha sonra yeni yapılar halinde birleştirebileceğiniz, daha sonra Azure Search farklı uygulamalar halinde kullanabileceğiniz daha fazla yapı düzeyleri eklemek için bunu yineleyebilirsiniz.

Numaralandırılmış, bilgi deposunun avantajları şunları içerir:

+ Arama dışındaki [analiz ve raporlama araçlarında](#tools-and-apps) zenginleştirilmiş belgeler kullanın. Power Query ile Power BI etkileyici bir seçimdir, ancak Azure depolama 'ya bağlanabilecek herhangi bir araç veya uygulama, oluşturduğunuz bir bilgi deposundan çekebilirsiniz.

+ Adımlarda ve beceri tanımlarında hata ayıklama sırasında bir AI dizin oluşturma işlem hattını daraltın. Bilgi deposu, bir AI dizin oluşturma işlem hattında bir beceri tanımının çarpımını gösterir. Bu sonuçları daha iyi bir beceri tasarlamak için kullanabilirsiniz, çünkü tam olarak neyin nasıl göründüğünü görebilirsiniz. Bir bilgi deposunun içeriğini görüntülemek için Azure Storage 'da [Depolama Gezgini](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows) kullanabilirsiniz.

+ Verileri yeni formlara şekillendirin. Yeniden şekillendirme becerileri içinde ortaklaşa bulunur, ancak nokta bir beceri artık bu özelliği sağlayabiliriz. Azure Search içindeki [yetenek başına mil](cognitive-search-skill-shaper.md) bu görevi kapsayacak şekilde genişletildi. Yeniden şekillendirme, ilişkileri korurken verilerin amaçlanan kullanımı ile hizalanan bir projeksiyon tanımlamanızı sağlar.

> [!Note]
> Bilişsel hizmetler kullanılarak AI tabanlı dizin oluşturma hakkında bilgi sahibi değil misiniz? Azure Search, görüntü dosyaları üzerinden optik karakter tanıma (OCR), metin dosyalarından varlık tanıma ve anahtar tümceciği ayıklama ve daha fazlasını kullanarak kaynak verileri ayıklamak ve zenginleştirmede bilişsel hizmetler düzenleme ve dil özellikleriyle tümleşir. Daha fazla bilgi için bkz. bilişsel [arama nedir?](cognitive-search-concept-intro.md).

## <a name="create-a-knowledge-store"></a>Bilgi deposu oluşturma

Bilgi deposu bir beceri tanımının bir parçasıdır. Bu önizlemede, oluşturmak için portalda REST API, using `api-version=2019-05-06-Preview` veya **veri içeri aktarma** Sihirbazı gerekir.

Aşağıdaki JSON, bir dizin `knowledgeStore`Oluşturucu tarafından çağrılan (gösterilmez) bir beceri parçası olan bir öğesinin bir parçası olduğunu belirtir. İçindeki projeksiyonun belirtimi, `knowledgeStore` tabloların veya nesnelerin Azure depolamada oluşturulup oluşturulmayacağını belirler.

Zaten AI tabanlı dizin oluşturma hakkında bilginiz varsa, Beceri tanımı, her zenginleştirilmiş belgenin oluşturulmasını, organizasyonunu ve birini belirler.

```json
{
  "name": "my-new-skillset",
  "description": 
  "Example showing knowledgeStore placement, supported in api-version=2019-05-06-Preview. You need at least one skill, most likely a Shaper skill if you are modulating data structures.",
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

## <a name="components-backing-a-knowledge-store"></a>Bilgi deposunu yedekleyen bileşenler

Bilgi deposu oluşturmak için aşağıdaki hizmetler ve yapıtlar gereklidir.

### <a name="1---source-data"></a>1-kaynak verileri

Zenginleştirmesini istediğiniz veriler veya belgeler Azure Search Dizin oluşturucular tarafından desteklenen bir Azure veri kaynağında bulunmalıdır: 

* [Azure SQL](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)

* [Azure Cosmos DB](search-howto-index-cosmosdb.md)

* [Azure Blob Depolama](search-howto-indexing-azure-blob-storage.md)

[Azure Tablo Depolaması](search-howto-indexing-azure-tables.md) bir bilgi deposundaki giden veriler için kullanılabilir, ancak bir AI tabanlı dizin oluşturma işlem hattına gelen veriler için kaynak olarak kullanılamaz.

### <a name="2---azure-search-service"></a>2-Azure Search hizmeti

Ayrıca, veri zenginleştirme için kullanılan nesneleri oluşturmak ve yapılandırmak için bir Azure Search hizmetine ve REST API ihtiyacınız vardır. Bilgi deposu `api-version=2019-05-06-Preview`oluşturmaya yönelik REST API.

Azure Search Dizin Oluşturucu özelliğini sağlar ve Dizin oluşturucular, tüm işlemin uçtan uca, Azure depolama 'da kalıcı ve zenginleştirilmiş belgeler elde etmek için kullanılır. Dizin oluşturucular bir veri kaynağı, dizin ve bir bilgi deposu oluşturmak ve doldurmak için gereken bir beceri-All kullanır.

| Object | REST API | Açıklama |
|--------|----------|-------------|
| veri kaynağı | [Veri Kaynağı Oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-data-source)  | Zenginleştirilmiş belgeler oluşturmak için kullanılan kaynak verileri sağlayan bir dış Azure veri kaynağını tanımlayan kaynak.  |
| Beceri | [Beceri oluşturma (api-Version = 2019-05-06)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)  | Dizin oluşturma sırasında bir zenginleştirme ardışık düzeninde kullanılan [yerleşik yeteneklerin](cognitive-search-predefined-skills.md) ve özel bilişsel [yeteneklerin](cognitive-search-custom-skill-interface.md) kullanımını koordine eden bir kaynak. |
| index | [Dizin Oluştur](https://docs.microsoft.com/rest/api/searchservice/create-index)  | Azure Search dizinini ifade eden şema. Dizindeki alanlar, kaynak verilerdeki alanlara veya zenginleştirme aşamasında üretilen alanlara (örneğin, varlık tanıma tarafından oluşturulan kuruluş adları için bir alan) eşlenir. |
| dizin oluşturucu | [Dizin Oluşturucu oluştur (api-Version = 2019-05-06)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)  | Dizin oluşturma sırasında kullanılan bileşenleri tanımlayan kaynak: veri kaynağı, Beceri, kaynak ve ara veri yapılarından hedef dizine ve dizinin kendisi de dahil olmak üzere. Dizin oluşturucuyu çalıştırmak, veri alımı ve zenginleştirme için tetikleyiciydi. Çıktı, kaynak verilerle doldurulan, becerileri aracılığıyla zenginleştirilmiş, Dizin şemasını temel alan bir arama dizinidir.  |

### <a name="3---cognitive-services"></a>3 bilişsel hizmetler

Beceri içinde belirtilen zenginler bilişsel hizmetler 'deki Görüntü İşleme ve dil özelliklerine dayalıdır. Bilişsel hizmetler işlevselliği, beceri ile dizin oluşturma sırasında yararlanılabilir. Bir beceri, yeteneklerin bir bileşim ve yetenekler belirli Görüntü İşleme ve dil özelliklerine bağlanır. Bilişsel hizmetler 'i bütünleştirmek için, bir beceri için bilişsel [Hizmetler kaynağı ekleyebilirsiniz](cognitive-search-attach-cognitive-services.md) .

### <a name="4---storage-account"></a>4-depolama hesabı

Azure depolama hesabınız kapsamında, bir beceri nasıl yapılandırdığınıza bağlı olarak bir blob kapsayıcısı veya tabloları oluşturur Azure Search. Verileriniz Azure Blob veya tablo depolama alanından kaynaklanıyorsa, zaten ayarlamış olursunuz. Aksi takdirde, bir Azure depolama hesabı oluşturmanız gerekir. Azure depolama 'daki tablolar ve nesneler, AI tabanlı dizin oluşturma işlem hattı tarafından oluşturulan zenginleştirilmiş belgeleri içerir.

Depolama hesabı beceri içinde belirtilmiştir. ' `api-version=2019-05-06-Preview`De, bir beceri tanımı, hesap bilgilerini sağlayabilmeniz için bilgi deposu tanımı içerir.

<a name="tools-and-apps"></a>

### <a name="5---access-and-consume"></a>5-erişim ve kullanım

Depolarda enzenginler varsa, Azure Blob veya tablo depolama 'ya bağlanan herhangi bir araç veya teknoloji, içeriği incelemek, çözümlemek veya kullanmak için kullanılabilir. Aşağıdaki liste bir başlangıç:

+ Zenginleştirilmiş belge yapısını ve içeriğini görüntülemek için [Depolama Gezgini](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows) . Bilgi deposu içeriğini görüntülemek için bunu taban çizgisi aracınız olarak düşünün.

+ Doğal dil sorguları için [Power Query Power BI](https://support.office.com/article/connect-to-microsoft-azure-blob-storage-power-query-f8165faa-4589-47b1-86b6-7015b330d13e) veya sayısal veriniz varsa raporlama ve analiz araçlarını kullanın.

+ Daha fazla düzenleme için [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/) .

+ Bilişsel [arama](cognitive-search-concept-intro.md)kullanarak dizinlediğiniz içerik üzerinde tam metin araması için Dizin Azure Search.

## <a name="document-persistence"></a>Belge kalıcılığı

Depolama hesabı içinde, enzenginler Azure Tablo depolama alanındaki tablolar veya Azure Blob depolama alanındaki nesneler olarak ifade edilebilir. Bu zenginleştirme, depolandıktan sonra, diğer veritabanlarına ve araçlara veri yüklemek için kaynak olarak kullanılabilir.

+ Tablo biçiminde veri şeması kullanan bir gösterimi istediğinizde tablo depolaması yararlı olur. Öğeleri yeni yollarla yeniden şekillendirmek veya yeniden birleştirmek istiyorsanız tablo depolama alanı size gerekli ayrıntı düzeyi sağlar.

+ BLOB depolama, her belgenin bir tam kapsamlı JSON temsilini oluşturur. Her iki depolama seçeneğini tek bir beceri içinde kullanarak bir dizi ifadenin tamamını alabilirsiniz.

+ Azure Search dizindeki içeriğe devam ediyor. Senaryonuzla ilgili olmayan bir araç ise, örneğin Amacınız başka bir araçla analiz alıyorsa, işlem hattının oluşturduğu dizini silebilirsiniz. Ancak, içeriğinizi de koruyabilir ve içeriklerle etkileşim kurmak için [Arama Gezgini](search-explorer.md) gibi bir yerleşik aracı (Depolama Gezgini ve analiz uygulamanızın arkasında) farklı bir ortam olarak kullanabilirsiniz.

Belge içeriğiyle birlikte, zenginleştirilmiş belgeler, zenginleştirmelerin üretilme beceri sürümünün meta verilerini içerir.  

## <a name="inside-a-knowledge-store"></a>Bilgi deposu içinde

Bilgi deposu bir ek açıklama önbelleği ve projeksiyoninden oluşur. *Önbellek* , hizmet tarafından, becerilerden sonuçları önbelleğe almak ve değişiklikleri izlemek için dahili olarak kullanılır. *Projeksiyon* , hedeflenen kullanım ile eşleşen enzenginler 'in şemasını ve yapısını tanımlar. Bilgi deposu başına bir önbellek vardır, ancak birden çok projeksiyonlar vardır. 

Önbellek her zaman bir blob kapsayıcısıdır, ancak projeksiyonlar tablo veya nesneler olarak eklenebilir:

+ Bir nesne olarak, projeksiyon BLOB depolama alanına eşlenir; burada projeksiyon, bir veri bilimi işlem hattı gibi senaryolar için JSON 'daki nesneler veya hiyerarşik gösterimler olan bir kapsayıcıya kaydedilir.

+ Tablo olarak, projeksiyon tablo depolamasına eşlenir. Tablo temsili, veri analizi veya makine öğrenimi için veri çerçeveleri olarak dışa aktarma gibi senaryolar için ilişkileri korur. Zenginleştirilmiş projeksiyonlar daha sonra diğer veri depolarına kolayca aktarılabilir. 

Bir bilgi deposunda, kuruluşunuzda çeşitli yapısal olarak uyum sağlamak için birden çok projeksiyonlar oluşturabilirsiniz. Geliştirici, zenginleştirilmiş bir belgenin tam JSON gösterimine erişim gerektirebilir, ancak veri bilimcileri veya analistleri, Beceri göre şekillendirilmiş veya modüler veri yapıları isteyebilir.

Örneğin, zenginleştirme işleminin amaçlarından biri de bir modeli eğitmek için kullanılan bir veri kümesi oluşturacaksa, verileri nesne deposuna yansıtma, veri bilimi işlem hatlarınızın verilerini kullanmanın bir yoludur. Alternatif olarak, tablo projeksiyonunu zenginleştirilmiş belgeler temelinde hızlı bir Power BI panosu oluşturmak istiyorsanız tablo projeksiyonu iyi çalışacaktır.

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

## <a name="where-do-i-start"></a>Nereden başlamalıyım?

Ücretsiz hizmeti öğrenme amaçlarıyla öneririz, ancak ücretsiz işlem sayısının her abonelik için günde 20 belge ile sınırlı olduğunu unutmayın.

Birden çok hizmet kullanırken, en iyi performans için tüm hizmetlerinizi aynı bölgede oluşturun ve maliyetleri en aza indirin. Gelen veriler için bant genişliği veya aynı bölgede başka bir hizmete giden veriler için ücretlendirilirsiniz.

**1. Adım: [Azure Search kaynağı oluşturma](search-create-service-portal.md)** 

**2. Adım: [Azure depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal)** 

**Adım 3: [Bilişsel hizmetler kaynağı oluşturma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)** 

**4. Adım: [Portalı](cognitive-search-quickstart-blob.md) kullanmaya başlayın-veya [rest ve Postman kullanarak örnek verilerle çalışmaya](knowledge-store-howto.md) başlayın** 

Bilgi deposu içeren bir `api-version=2019-05-06-Preview` AI tabanlı işlem hattı oluşturmak için REST kullanabilirsiniz. En yeni önizleme API 'sinde, Beceri nesnesi `knowledgeStore` tanımı sağlar.

## <a name="takeaways"></a>Paketler

Bilgi deposu, arama, maliyet denetimleri ve zenginleştirme sürecinizdeki bir yandan bu belgelerin kullanımını yönetme dahil olmak üzere çeşitli avantajlar sunar ancak bunlarla sınırlı değildir. Bu özelliklerin tümü, Beceri ' e bir depolama hesabı ekleyerek ve güncelleştirilmiş ifade dilini kullanarak, [bilgi deposu ile çalışmaya başlama](knowledge-store-howto.md)bölümünde açıklandığı gibi, yalnızca kullanım için kullanılabilir. 

## <a name="next-steps"></a>Sonraki adımlar

Zenginleştirilmiş belgeler oluşturmak için en basit yaklaşım, **veri alma** Sihirbazı ' nı kullanmaktır.

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Bir portal 'da bilişsel aramayı deneyin](cognitive-search-quickstart-blob.md)
