---
title: Bilgi deposuna giriş (Önizleme)
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama ve diğer uygulamalarda zenginleştirilmiş belgeleri görüntüleyebileceğiniz, yeniden şekillendirbileceğiniz ve kullanabileceğiniz Azure depolama 'ya zenginleştirilmiş belgeler gönderin. Bu özellik genel önizleme aşamasındadır.
author: HeidiSteen
manager: nitinme
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 12/11/2019
ms.openlocfilehash: 9a6fa62384615f60da88bb41da8ad3538d34e62a
ms.sourcegitcommit: 380e3c893dfeed631b4d8f5983c02f978f3188bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2020
ms.locfileid: "75754102"
---
# <a name="introduction-to-knowledge-stores-in-azure-cognitive-search"></a>Azure Bilişsel Arama bilgi depolarına giriş

> [!IMPORTANT] 
> Bilgi deposu Şu anda genel önizleme aşamasındadır. Önizleme işlevselliği, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). [REST API sürüm 2019-05-06-önizleme](search-api-preview.md) , Önizleme özellikleri sağlar. Şu anda sınırlı sayıda portal desteği var ve .NET SDK desteği yok.

Bilgi deposu, bağımsız analiz veya aşağı akış işleme için bir [AI zenginleştirme ardışık düzeninde](cognitive-search-concept-intro.md) çıkışın devam eden bir Azure bilişsel arama özelliğidir. *Zenginleştirilmiş bir belge* , yapay, yapılandırılmış ve AI süreçlerini kullanılarak çözümlenen içerikten oluşturulan bir ardışık düzen çıktıdır. Standart bir AI ardışık düzeninde, zenginleştirilmiş belgeler yalnızca dizin oluşturma sırasında kullanılır ve sonra atılır. Bilgi deposu ile zenginleştirilmiş belgeler korunur. 

Daha önce bilişsel becerileri kullandıysanız, *becerileri* bir belgeyi bir dizi enzenginleştirme aracılığıyla taşıyabileceğinizi zaten anlarsınız. Sonuç bir bilgi deposundaki bir arama dizini veya (Bu önizlemede yeni) projeksiyonu olabilir. İki çıkış, arama dizini ve bilgi deposu, aynı işlem hattının çarpımlarından oluşur; aynı girdilerden türetilmiş, ancak aynı şekilde yapılandırılmış, depolanmış ve çok farklı yollarla kullanılan çıktının sonucu.

Azure [depolama](https://docs.microsoft.com/azure/storage/common/storage-account-overview), Azure Tablo depolama, Azure Blob depolama ya da her ikisi de fiziksel olarak bir bilgi deposu. Azure depolama 'ya bağlanabilecek herhangi bir araç veya işlem bilgi deposunun içeriğini kullanabilir.

![Ardışık düzen diyagramında bilgi deposu](./media/knowledge-store-concept-intro/knowledge-store-concept-intro.svg "Ardışık düzen diyagramında bilgi deposu")

## <a name="benefits-of-knowledge-store"></a>Bilgi deposunun avantajları

Bilgi deposu, yerleşik olarak yapılandırılmamış ve yarı yapılandırılmış veri dosyalarından, uygulanan Analize sahip görüntü dosyalarında veya hatta yapılandırılmış veriler, yeni formlara yeniden şekillendirilecek şekilde yapı, bağlam ve gerçek içerik sağlar. [Adım adım](knowledge-store-create-rest.md)bir kılavuzda, en çok yoğun bir JSON belgesinin alt yapılara nasıl bölümlendiğini, yeni yapılara reconstituted ve makine öğrenimi ve veri bilimi iş yükleri gibi aşağı akış işlemlerinde nasıl kullanılabilir yapıldığını görebilirsiniz.

Bir AI zenginleştirme ardışık düzeninin ne işe yarayabileceği görmek faydalı olsa da, bir bilgi deposunun gerçek potansiyeli verileri yeniden şekillendirmeye olanak tanır. Temel bir beceri ile başlayabilir ve daha sonra yeni yapılar halinde birleştirebileceğiniz, daha sonra Azure Bilişsel Arama yanı sıra diğer uygulamalarda kullanabileceğiniz daha fazla yapı düzeyleri eklemek için bunu yineleyebilirsiniz.

Numaralandırılmış, bilgi deposunun avantajları şunları içerir:

+ Arama dışındaki [analiz ve raporlama araçlarında](#tools-and-apps) zenginleştirilmiş belgeler kullanın. Power Query ile Power BI etkileyici bir seçimdir, ancak Azure depolama 'ya bağlanabilecek herhangi bir araç veya uygulama, oluşturduğunuz bir bilgi deposundan çekebilirsiniz.

+ Adımlarda ve beceri tanımlarında hata ayıklama sırasında bir AI dizin oluşturma işlem hattını daraltın. Bilgi deposu, bir AI dizin oluşturma işlem hattında bir beceri tanımının çarpımını gösterir. Bu sonuçları daha iyi bir beceri tasarlamak için kullanabilirsiniz, çünkü tam olarak neyin nasıl göründüğünü görebilirsiniz. Bir bilgi deposunun içeriğini görüntülemek için Azure Storage 'da [Depolama Gezgini](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows) kullanabilirsiniz.

+ Verileri yeni formlara şekillendirin. Yeniden şekillendirme becerileri içinde ortaklaşa bulunur, ancak nokta bir beceri artık bu özelliği sağlayabiliriz. Azure Bilişsel Arama 'de [beceri başına mil](cognitive-search-skill-shaper.md) bu görevi kapsayacak şekilde genişletildi. Yeniden şekillendirme, ilişkileri korurken verilerin amaçlanan kullanımı ile hizalanan bir projeksiyon tanımlamanızı sağlar.

> [!Note]
> AI zenginleştirme ve bilişsel becerileri yenilikleri nelerdir? Azure Bilişsel Arama, görüntü dosyaları üzerinden optik karakter tanıma (OCR), metin dosyalarından varlık tanıma ve anahtar tümceciği ayıklama ve daha fazlasını kullanarak kaynak verileri ayıklamak ve zenginleştirmek için bilişsel hizmetler düzenleme ve dil özellikleriyle tümleşir. Daha fazla bilgi için bkz. [Azure bilişsel arama 'de AI zenginleştirme](cognitive-search-concept-intro.md).

## <a name="physical-storage"></a>Fiziksel depolama alanı

Bilgi deposunun fiziksel ifadesi, Beceri içindeki bir `knowledgeStore` tanımının `projections` öğesi aracılığıyla ifade edilir. Projeksiyon, bir çıktının yapısını, amaçlanan kullanım ile eşleşecek şekilde tanımlar.

Projeksiyonlar tablo, nesne veya dosya olarak ifade edilebilir.

```json
"knowledgeStore": { 
    "storageConnectionString": "<YOUR-AZURE-STORAGE-ACCOUNT-CONNECTION-STRING>", 
    "projections": [ 
        { 
            "tables": [ ], 
            "objects": [ ], 
            "files": [ ]
        },
                { 
            "tables": [ ], 
            "objects": [ ], 
            "files": [ ]
        }
```

Bu yapıda belirttiğiniz projeksiyonun türü, bilgi deposu tarafından kullanılan depolama türünü belirler.

+ `tables`tanımladığınızda tablo depolaması kullanılır. Analitik araçlara giriş veya diğer veri depolarına veri çerçeveleri olarak dışarı aktarma için tablo Raporlama yapılarına ihtiyacınız olduğunda tablo projeksiyonu tanımlayın. Zenginleştirilmiş belgelerin alt kümesini veya çapraz bölümünü almak için birden çok `tables` belirtebilirsiniz. Aynı projeksiyon grubu içinde, tüm bunlarla çalışabilmeniz için tablo ilişkileri korunur.

+ BLOB depolama, `objects` veya `files`tanımlarken kullanılır. `object` fiziksel temsili, zenginleştirilmiş bir belgeyi temsil eden hiyerarşik bir JSON yapısıdır. `file`, bir belgeden ayıklanan ve BLOB depolama alanına bozulmadan aktarılan bir görüntüdür.

Tek bir projeksiyon nesnesi, bir dizi `tables`, `objects`, `files`ve birçok senaryo için bir projeksiyon oluşturmak yeterli olabilir. 

Ancak, -`file` projeksiyonlar `object``table`-birden çok kümesi oluşturmak mümkündür ve farklı veri ilişkileri istiyorsanız bunu yapabilirsiniz. Bir küme içinde, bu ilişkilerin mevcut olduğu ve algılanabilmesi durumunda veriler birbiriyle ilgilidir. Ek kümeler oluşturursanız, her bir gruptaki belgeler hiçbir şekilde ilgili değildir. Birden çok projeksiyon grubu kullanmanın bir örneği, aynı verilerin bir çevrimiçi sisteminizle birlikte kullanılması ve bunun belirli bir şekilde temsil edilebilmesi için de tasarlanan bir veri bilimi ardışık düzeninde kullanılmak üzere aynı verilerin de olmasını istiyorsanız olabilir. ele.

## <a name="requirements"></a>Gereksinimler 

[Azure depolama alanı](https://docs.microsoft.com/azure/storage/) gereklidir. Fiziksel depolama alanı sağlar. BLOB depolama, tablo depolama veya her ikisini de kullanabilirsiniz. BLOB depolama, genellikle çıkış aşağı akış işlemlerine giderken, genellikle zenginleştirilmiş belgeler için kullanılır. Tablo depolama, genellikle analiz ve raporlama için kullanılan, zenginleştirilmiş belgelerin dilimlerine yöneliktir.

[Beceri](cognitive-search-working-with-skillsets.md) gereklidir. `knowledgeStore` tanımını içerir ve zenginleştirilmiş bir belgenin yapısını ve birleşimini belirler. Boş bir beceri kullanarak bilgi deposu oluşturamazsınız. Beceri içinde en az bir beceriye sahip olmanız gerekir.

[Dizin Oluşturucu](search-indexer-overview.md) gereklidir. Bir beceri, yürütmeyi yönlendiren bir Dizin Oluşturucu tarafından çağrılır. Dizin oluşturucular kendi gereksinimler ve öznitelikleri kümesiyle gelir. Bu özniteliklerin birkaçı bir bilgi deposunda doğrudan bir pul alır:

+ Dizin oluşturucular [desteklenen bir Azure veri kaynağı](search-indexer-overview.md#supported-data-sources) gerektirir (Azure 'da desteklenen bir kaynaktan veri çekilerek bilgi deposunu oluşturan işlem hattı başlatılır). 

+ Dizin oluşturucular bir arama dizini gerektirir. Bir Dizin Oluşturucu, hiç bir şekilde kullanmayı planlamıyorsanız bile bir dizin şeması sağlamanızı gerektirir. En az dizinde anahtar olarak atanan bir dize alanı bulunur.

+ Dizin oluşturucular, hedef alana kaynak alanı diğer adı için kullanılan isteğe bağlı alan eşlemeleri sağlar. Bir varsayılan alan eşlemesinin değiştirilmesine ihtiyacı varsa (farklı bir ad veya tür kullanmak için), bir dizin oluşturucu içinde [alan eşlemesi](search-indexer-field-mappings.md) oluşturabilirsiniz. Bilgi deposu çıktısı için, hedef bir blob nesnesi veya tablosunda bir alan olabilir.

+ Dizin oluşturucular zamanlamalar ve çeşitli veri kaynakları tarafından sunulan değişiklik algılama mekanizmaları gibi diğer özellikleri de bir bilgi deposuna uygulanabilir. Örneğin, içeriği yenilemek için düzenli aralıklarla zenginleştirme [zamanlayabilirsiniz](search-howto-schedule-indexers.md) . 

## <a name="how-to-create-a-knowledge-store"></a>Bilgi deposu oluşturma

Bilgi deposu oluşturmak için portalı veya önizleme REST API (`api-version=2019-05-06-Preview`) kullanın.

### <a name="use-the-azure-portal"></a>Azure Portal’ı kullanma

**Veri Içeri aktarma** Sihirbazı, bilgi deposu oluşturma seçeneklerini içerir. İlk araştırma için, [ilk bilgi deponuzu dört adımda oluşturun](knowledge-store-connect-power-bi.md).

1. Desteklenen bir veri kaynağı seçin.

1. Zenginleştirme belirtin: kaynak iliştirme, beceriler seçme ve bir bilgi deposu belirtme. 

1. Dizin şeması oluşturun. Sihirbaz için gereklidir ve bir tane olabilir.

1. Sihirbazı çalıştırın. Ayıklama, zenginleştirme ve depolama bu son adımda oluşur.

### <a name="use-create-skillset-and-the-preview-rest-api"></a>Create beceri ve Preview REST API kullanın

Bir `knowledgeStore`, bir [Dizin Oluşturucu](search-indexer-overview.md)tarafından çağrılan bir [beceri](cognitive-search-working-with-skillsets.md)içinde tanımlanır. Zenginleştirme sırasında Azure Bilişsel Arama, Azure depolama hesabınızda bir alan oluşturur ve yapılandırmanıza bağlı olarak, belgeleri Bloblar veya tablolar olarak zenginleştirir.

Şu anda önizleme REST API, programlama yoluyla bilgi deposu oluşturabileceğiniz tek mekanizmadır. Araştırmanın kolay bir yolu, [Postman ve REST API kullanarak ilk bilgi deponuzi oluşturmaktır](knowledge-store-create-rest.md).

Bu önizleme özelliği için başvuru içeriği, bu makalenin [API başvurusu](#kstore-rest-api) bölümünde bulunur. 

<a name="tools-and-apps"></a>

## <a name="how-to-connect-with-tools-and-apps"></a>Araçlar ve uygulamalar ile bağlanma

Depolarda enzenginler varsa, Azure Blob veya tablo depolama 'ya bağlanan herhangi bir araç veya teknoloji, içeriği incelemek, çözümlemek veya kullanmak için kullanılabilir. Aşağıdaki liste bir başlangıç:

+ Zenginleştirilmiş belge yapısını ve içeriğini görüntülemek için [Depolama Gezgini](knowledge-store-view-storage-explorer.md) . Bilgi deposu içeriğini görüntülemek için bunu taban çizgisi aracınız olarak düşünün.

+ Raporlama ve analiz için [Power BI](knowledge-store-connect-power-bi.md) . 

+ Daha fazla düzenleme için [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/) .

<a name="kstore-rest-api"></a>

## <a name="api-reference"></a>API başvurusu

Bu bölüm, bir `knowledgeStore` tanımı içerecek şekilde değiştirilen [Create beceri (REST API)](https://docs.microsoft.com/rest/api/searchservice/create-skillset) başvuru belgesi 'nin bir sürümüdür. 

### <a name="example---knowledgestore-embedded-in-a-skillset"></a>Örnek-knowledgeStore Embedded beceri

Aşağıdaki örnekte, bir beceri tanımının en altında `knowledgeStore` gösterilmektedir. 

* İsteği formülleştirmek için **Post** veya **PUT** kullanın.
* Bilgi deposu işlevine erişmek için REST API `api-version=2019-05-06-Preview` sürümünü kullanın. 

```http
POST https://[servicename].search.windows.net/skillsets?api-version=2019-05-06-Preview
api-key: [admin key]
Content-Type: application/json
```

İsteğin gövdesi, `knowledgeStore`içeren bir beceri tanımlayan bir JSON belgesidir.

```json
{
  "name": "my-skillset-name",
  "description": "Extract organization entities and generate a positive-negative sentiment score from each document.",
  "skills":
  [
    {
      "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
      "categories": [ "Organization" ],
      "defaultLanguageCode": "en",
      "inputs": [
        {
          "name": "text",
          "source": "/document/content"
        }
      ],
      "outputs": [
        {
          "name": "organizations",
          "targetName": "organizations"
        }
      ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Text.SentimentSkill",
      "inputs": [
        {
          "name": "text",
          "source": "/document/content"
        }
      ],
      "outputs": [
        {
          "name": "score",
          "targetName": "mySentiment"
        }
      ]
    },
  ],
  "cognitiveServices": 
    {
    "@odata.type": "#Microsoft.Azure.Search.CognitiveServicesByKey",
    "description": "mycogsvcs resource in West US 2",
    "key": "<YOUR-COGNITIVE-SERVICES-KEY>"
    },
    "knowledgeStore": { 
        "storageConnectionString": "<YOUR-AZURE-STORAGE-ACCOUNT-CONNECTION-STRING>", 
        "projections": [ 
            { 
                "tables": [  
                { "tableName": "Organizations", "generatedKeyName": "OrganizationId", "source": "/document/organizations*"}, 
                { "tableName": "Sentiment", "generatedKeyName": "SentimentId", "source": "/document/mySentiment"}
                ], 
                "objects": [ ], 
                "files": [  ]       
            }    
        ]     
    } 
}
```

### <a name="request-body-syntax"></a>İstek gövdesi sözdizimi  

Aşağıdaki JSON, bir `indexer` (gösterilmez) tarafından çağrılan [`skillset`](https://docs.microsoft.com/rest/api/searchservice/create-skillset)bir parçası olan `knowledgeStore`belirtir. AI zenginleştirme hakkında zaten bilgi sahibiyseniz bir beceri, zenginleştirilmiş bir belgenin birleşimini belirler. Bir beceri, veri yapılarını modüle ediyorsanız en az bir yetenek, büyük olasılıkla yetenek başına bir yetenek içermelidir.

İstek yükünü yapılandırmak için sözdizimi aşağıdaki gibidir.

```json
{   
    "name" : "Required for POST, optional for PUT requests which sets the name on the URI",  
    "description" : "Optional. Anything you want, or null",  
    "skills" : "Required. An array of skills. Each skill has an odata.type, name, input and output parameters",
    "cognitiveServices": "A key to Cognitive Services, used for billing.",
    "knowledgeStore": { 
        "storageConnectionString": "<YOUR-AZURE-STORAGE-ACCOUNT-CONNECTION-STRING>", 
        "projections": [ 
            { 
                "tables": [ 
                    { "tableName": "<NAME>", "generatedKeyName": "<FIELD-NAME>", "source": "<DOCUMENT-PATH>" },
                    { "tableName": "<NAME>", "generatedKeyName": "<FIELD-NAME>", "source": "<DOCUMENT-PATH>" },
                    . . .
                ], 
                "objects": [ 
                    {
                    "storageContainer": "<BLOB-CONTAINER-NAME>", 
                    "source": "<DOCUMENT-PATH>", 
                    }
                ], 
                "files": [ 
                    {
                    "storageContainer": "<BLOB-CONTAINER-NAME>",
                    "source": "/document/normalized_images/*"
                    }
                ]  
            },
            {
                "tables": [ ],
                "objects": [ ],
                "files":  [ ]
            }  
        ]     
    } 
}
```

`knowledgeStore` iki özelliğe sahiptir: bir Azure depolama hesabına `storageConnectionString` ve fiziksel depolamayı tanımlayan `projections`. Herhangi bir depolama hesabını kullanabilirsiniz, ancak aynı bölgedeki hizmetleri kullanmak uygun maliyetli olabilir.

`projections` koleksiyonu İzdüşüm nesneleri içerir. Her projeksiyon nesnesinin, belirtilen veya null olan `tables`, `objects`, `files` (her biri) sahip olması gerekir. Yukarıdaki sözdizimi iki nesne gösterir, biri tam olarak belirtilir ve diğeri tamamen null. Yansıtma nesnesi içinde, depolama alanında ifade edildikten sonra veriler arasındaki ilişkiler korunur. 

Yalıtımı ve belirli senaryoları desteklemek için ihtiyacınız olan çok sayıda yansıtma nesnesi oluşturun (örneğin, araştırma için kullanılan veri yapıları ve bir veri bilimi iş yükünde gerekenlere karşı). `source` ve `storageContainer` ya da bir nesne içindeki farklı değerlere `table` ayarlayarak belirli senaryolar için yalıtım ve özelleştirme sağlayabilirsiniz. Daha fazla bilgi ve örnek için bkz. [bir bilgi deposunda projeksiyonlarla çalışma](knowledge-store-projection-overview.md).

|Özellik      | Uygulandığı öğe: | Açıklama|  
|--------------|------------|------------|  
|`storageConnectionString`| `knowledgeStore` | Gereklidir. Şu biçimde: `DefaultEndpointsProtocol=https;AccountName=<ACCOUNT-NAME>;AccountKey=<ACCOUNT-KEY>;EndpointSuffix=core.windows.net`|  
|`projections`| `knowledgeStore` | Gereklidir. `tables`, `objects`, `files` ve ilgili özelliklerinden oluşan özellik nesnelerinin bir koleksiyonu. Kullanılmayan projeksiyonlar null olarak ayarlanabilir.|  
|`source`| Tüm projeksiyler| Projeksiyonun kökü olan zenginleştirme ağacının düğümünün yolu. Bu düğüm, Beceri içindeki yeteneklerin herhangi birinin çıktıdır. Yollar, zenginleştirilmiş belgeyi temsil eden, ancak `/document/content/` veya belge ağacı içindeki düğümlere genişletilebilen `/document/`başlar. Örnekler: `/document/countries/*` (tüm ülkeler) veya `/document/countries/*/states/*` (tüm ülkelerde tüm durumlar). Belge yolları hakkında daha fazla bilgi için bkz. [beceri kavramları ve bileşimi](cognitive-search-working-with-skillsets.md).|
|`tableName`| `tables`| Azure Tablo depolamada oluşturulacak bir tablo. |
|`storageContainer`| `objects`, `files`| Azure Blob depolamada oluşturulacak kapsayıcının adı. |
|`generatedKeyName`| `tables`| Bir belgeyi benzersiz bir şekilde tanımlayan tabloda oluşturulan sütun. Zenginleştirme ardışık düzeni, bu sütunu üretilen değerlerle doldurur.|


### <a name="response"></a>Yanıt  

 Başarılı bir istek için "201 oluşturuldu" durum kodunu görmeniz gerekir. Varsayılan olarak, yanıt gövdesi oluşturulan beceri tanımı için JSON 'u içerir. Bilgi deposunun, bu beceri başvuran bir Dizin Oluşturucu çağırana kadar oluşturulmadığından emin olarak unutmayın.

## <a name="next-steps"></a>Sonraki adımlar

Bilgi deposu, bir Azure depolama hesabına erişim özellikli herhangi bir istemci uygulaması tarafından tüketimine yönelik bir beceri tasarımı veya yeni yapıların oluşturulması için yararlı olan belgelerin kalıcılığını, yararlı bir şekilde veya yeni yapıların oluşturulmasını sağlar.

Zenginleştirilmiş belgeler oluşturmak için en basit yaklaşım [Portal üzerinden](knowledge-store-create-portal.md)yapılır, ancak nesnelerin nasıl oluşturulduğuna ve başvurulduğunu öğrenmek isterseniz daha kullanışlı olan Postman ve REST API de kullanabilirsiniz.

> [!div class="nextstepaction"]
> [Postman ve REST kullanarak bilgi deposu oluşturma](knowledge-store-create-rest.md)
