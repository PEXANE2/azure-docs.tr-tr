---
title: Bir bilgi deposunda (Önizleme) projeksiyonlarla çalışma-Azure Search
description: Arama dışındaki senaryolarda kullanılmak üzere zenginleştirilmiş verilerinizi AI dizin oluşturma ardışık düzeninde kaydedin ve şekillendirin
manager: nitinme
author: vkurpad
services: search
ms.service: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: vikurpad
ms.openlocfilehash: c5fb547b18bc4014f91341070f49c4af84c01005
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2019
ms.locfileid: "71265178"
---
# <a name="working-with-projections-in-a-knowledge-store-in-azure-search"></a>Azure Search bir bilgi deposunda projeksiyonlarla çalışma

> [!Note]
> Bilgi deposu önizleme aşamasındadır ve üretim kullanımı için tasarlanmamıştır. [REST API sürüm 2019-05-06-önizleme](search-api-preview.md) bu özelliği sağlar. Şu anda .NET SDK desteği yok.
>

Azure Search, dizin oluşturmanın bir parçası olarak AI bilişsel beceriler ve özel yetenekler aracılığıyla içerik zenginleştirmesini mümkün. Zenginleştirme belgelerinize yapı ekler ve daha etkili bir şekilde arama yapın. Birçok örnekte, zenginleştirilmiş belgeler, bilgi madenciliği gibi arama dışındaki senaryolar için yararlıdır.

[Bilgi deposunun](knowledge-store-concept-intro.md)bir bileşeni olan tahminler, bilgi madenciliği için fiziksel depolamaya kaydedilebilen zenginleştirilmiş belgelerin görünümleridir. Bir projeksiyon, verilerinizi, Power BI gibi araçların ek bir çaba olmadan okuyabilmesi için, ilişkilerini koruyarak, verilerinizi gereksinimlerinize göre hizalayan bir şekle "proje" sağlar. 

Tahminler, Azure Tablo depolamada veya Azure Blob depolama alanında depolanan JSON nesnelerinde bulunan satırlar ve sütunlarda depolanan verilerle tablosal olabilir. Verilerinizin zenginleştirmekte olduğu haliyle birden fazla projeksiyoni tanımlayabilirsiniz. Bu, bireysel kullanım durumları için aynı verilerin farklı şekilde şekillendirilmiş olmasını istediğinizde faydalıdır. 

Bilgi deposu iki tür projeksiyonu destekler:

+ **Tablolar**: satır ve sütun olarak en iyi şekilde temsil edilen veriler için tablo projeksiyonları tablo depolamada şema veya projeksiyon tanımlamanızı sağlar. 

+ **Nesneler**: verileriniz ve zenginleştirme IÇIN bir JSON temsiline ihtiyacınız olduğunda, nesne projeksiyonlar blob olarak kaydedilir.

Bağlamda tanımlanan projeksiyonları görmek için [bilgi deposu ile çalışmaya](knowledge-store-howto.md)başlayın.

## <a name="projection-groups"></a>Projeksiyon grupları

Bazı durumlarda, farklı hedefleri karşılamak için zenginleştirilmiş verilerinizi farklı şekillerde proje yapmanız gerekir. Bilgi deposu, birden çok projeksiyonun grubunu tanımlamanızı sağlar. Projeksiyon grupları, karşılıklı denetim ve related'ın aşağıdaki temel özelliklerine sahiptir.

### <a name="mutually-exclusivity"></a>Birbirini dışlayan denetim

Tek bir grupta yansıtılan tüm içerikler, diğer projeksiyon gruplarında yansıtılan verilerden bağımsızdır. Bu, aynı verilerin şekillendirilmiş bir şekilde farklı, ancak her projeksiyon grubunda tekrarlanabileceği anlamına gelir. 

Projeksiyon gruplarında uygulanan bir kısıtlama, projeksiyon türlerindeki karşılıklı denetim karşılıklı bir gruptur. Tek bir grup içinde yalnızca tablo projeksiyonlarını veya nesne projeksiyonlarını tanımlayabilirsiniz. Hem tablo hem de nesne istiyorsanız, tablolar için bir yansıtma grubu ve nesneler için ikinci bir projeksiyon grubu tanımlayın.

### <a name="relatedness"></a>Relatedlik

Tek bir projeksiyon grubu içinde yansıtılan tüm içerik, veriler içindeki ilişkileri korur. İlişkiler oluşturulan bir anahtara dayalıdır ve her bir alt düğüm üst düğüme bir başvuru tutar. İlişkiler, yansıtma gruplarını ve bir projeksiyon grubunda oluşturulan tabloları veya nesneleri diğer projeksiyon gruplarında oluşturulan verilerle hiçbir ilişkiye sahip değildir.

## <a name="input-shaping"></a>Giriş şekillendirme
Verilerinizin doğru şekilde veya yapıda alınması, etkin kullanım için anahtar, BT tabloları veya nesneleri olmalıdır. Verilerinizi nasıl kullanabileceğinizi ve kullanabileceğinizi temel alarak verilerinizi şekillendirebilir veya yapısal hale getirme özelliği, Beceri içinde her yetenek için **mil** olarak kullanıma sunulan bir temel yetenektir.  

Projeksiyon şeması ile eşleşen zenginleştirme ağacında bir nesneniz olduğunda, projeksiyonu daha kolay tanımlanır. [Her yetenek](cognitive-search-skill-shaper.md) Için güncelleştirilmiş mil, zenginleştirme ağacının farklı düğümlerinden bir nesne oluşturmanıza ve bunların üstünü yeni bir düğüm altında oluşturmanıza olanak sağlar. **Mil başına** , iç içe geçmiş nesnelerle karmaşık türler tanımlamanıza olanak sağlar.

Projeniz için gereken tüm öğeleri içeren yeni bir şekil tanımladığınız zaman, artık bu şekli projeksiyonlarınızın kaynağı olarak veya başka bir beceriye giriş olarak kullanabilirsiniz.

## <a name="table-projections"></a>Tablo projeksiyonlarını

İçeri aktarma işlemi daha da kolaylaştırdığı için Power BI ile veri araştırması için tablo projeksiyonlarını öneririz. Ayrıca, tablo projeksiyonları değiştirmeye izin veren tablo ilişkisi arasındaki kardinalite değiştirme. 

Dizininizdeki tek bir belgeyi birden çok tabloya proje ekleyebilirsiniz ve ilişkileri korur. Birden çok tabloya yansıtıldığınızda, bir alt düğüm aynı grup içindeki başka bir tablonun kaynağı değilse, tüm şekil her tabloya yansıtılır.

### <a name="defining-a-table-projection"></a>Tablo projeksiyonu tanımlama

Beceri `knowledgeStore` öğesi içinde tablo projeksiyonu tanımlarken, zenginleştirme ağacındaki bir düğümü tablo kaynağına eşleyerek başlayın. Genellikle bu düğüm, tablolarda proje yapmanız gereken belirli bir şekli oluşturmak için yetenekler listesine eklediğiniz **her** bir beceriye ait çıktıdır. Projeyi seçtiğiniz düğüm birden çok tablo halinde projeye dilimlenebilir. Tablolar tanımı, proje yapmak istediğiniz tabloların bir listesidir. 

#### <a name="projection-slicing"></a>Projeksiyon Dilimleme
Bir tablo projeksiyon grubu tanımlarken, zenginleştirme ağacındaki tek bir düğüm birden çok ilişkili tabloya dilimlenebilir. Var olan bir tablo projeksiyonunun alt düğümü olan bir kaynak yolu içeren bir tablo eklemek, alt düğümün üst düğümden dilimlenmiş ve yeni, bununla ilişkili yeni tabloya yansıtılmakta olacaktır. Bu, tüm tablo projeksiyonlarınızın kaynağı olabilecek her bir şekilde mil halinde tek bir düğüm tanımlamanızı sağlar.

Her tablo üç özellik gerektirir:

+ tableName: Azure Storage 'daki tablonun adı.

+ generatedKeyName: Bu satırı benzersiz bir şekilde tanımlayan anahtarın sütun adı.

+ Kaynak: zenginleştirme ağacınızdaki düğüm, kendi zenginleştirmelerinin kaynağını oluşturur. Bu genellikle biçimlendiricilerin çıktıdır, ancak yeteneklerin herhangi birinin çıktısı olabilir.

Aşağıda tablo projeksiyonlarını örnek verilmiştir.

```json
{
    "name": "your-skillset",
    "skills": [
      …your skills
      
    ],
"cognitiveServices": {
… your cognitive services key info
    },

    "knowledgeStore": {
      "storageConnectionString": "an Azure storage connection string",
      "projections" : [
        {
          "tables": [
            { "tableName": "MainTable", "generatedKeyName": "SomeId", "source": "/document/EnrichedShape" },
            { "tableName": "KeyPhrases", "generatedKeyName": "KeyPhraseId", "source": "/document/EnrichedShape/*/KeyPhrases/*" },
            { "tableName": "Entities", "generatedKeyName": "EntityId", "source": "/document/EnrichedShape/*/Entities/*" }
          ]
        },
        {
          "objects": [
            
          ]
        }
      ]
    }
}
```
Bu örnekte gösterildiği gibi, anahtar tümcecikler ve varlıklar farklı tablolara modellenir ve her satır için üst (MainTable) öğesine geri bir başvuru içerir. 

Aşağıdaki çizimde, [bilgi deposu ile çalışmaya başlama makalesindeki](knowledge-store-howto.md)Caselaw alıştırmasına yönelik bir başvuru yer alır. Bir durumda birden çok opvaya sahip olan bir senaryoda ve her bir görüşün içinde yer alan varlıkları tanımlayarak zenginleştirilerek, bu tahminleri burada gösterildiği gibi modelleyebilirsiniz.

![Tablolardaki varlıklar ve ilişkiler](media/knowledge-store-projection-overview/TableRelationships.png "Tablo projeksiyonda ilişkileri modelleme")

## <a name="object-projections"></a>Nesne projeksiyonları

Nesne projeksiyonları herhangi bir düğümden kaynaksız bir şekilde zenginleştirme ağacının JSON temsilleridir. Çoğu durumda, bir tablo projeksiyonu oluşturan her yetenek için aynı **mil** , nesne projeksiyonu oluşturmak için kullanılabilir. 

```json
{
 
    "name": "your-skillset",
    "skills": [
      …your skills
      
    ],
"cognitiveServices": {
… your cognitive services key info
    },

    "knowledgeStore": {
      "storageConnectionString": "an Azure storage connection string",
      "projections" : [
        {
          "tables": [ ]
        },
        {
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

Nesne projeksiyonu oluşturmak, nesneye özgü birkaç özniteliği gerektirir:

+ storageContainer: nesnelerin kaydedileceği kapsayıcı
+ Kaynak: projeksiyon kökü olan zenginleştirme ağacının düğümünün yolu
+ anahtar: depolanacak nesnenin benzersiz bir anahtarını temsil eden bir yol. Kapsayıcıda Blobun adını oluşturmak için kullanılır.

## <a name="projection-lifecycle"></a>Projeksiyon yaşam döngüsü

Projeksiyonlarınızın veri kaynağınızdaki kaynak verilere bağlı bir yaşam döngüsü vardır. Verileriniz güncelleştirildiğinden ve yeniden dizinlendiğinden, tahminleriniz, projeksiyonlarınızın veri kaynağınızdaki verilerle tutarlı olmasını sağlayan zenginlerin sonuçlarıyla güncelleştirilir. Tahminler, dizininiz için yapılandırdığınız silme ilkesini alırlar. 

## <a name="using-projections"></a>Projeksiyonları kullanma

Dizin Oluşturucu çalıştırıldıktan sonra, tahminler aracılığıyla belirttiğiniz kapsayıcılardaki veya tablolardaki yansıtılan verileri okuyabilirsiniz. 

Analiz için Power BI araştırma, Azure Tablo depolama alanını veri kaynağı olarak ayarlamak kadar basittir. İçindeki ilişkilerden yararlanarak verilerinize kolayca bir görselleştirme kümesi oluşturabilirsiniz.

Alternatif olarak, bir veri bilimi ardışık düzeninde zenginleştirilmiş verileri kullanmanız gerekiyorsa, [verileri bloblardan bir Pandas DataFrame 'e yükleyebilirsiniz](../machine-learning/team-data-science-process/explore-data-blob.md).

Son olarak, verilerinizi bilgi deposundan dışarı aktarmanız gerekiyorsa Azure Data Factory, verileri dışarı aktarmak ve seçtiğiniz veritabanına eklemek için bağlayıcılar içerir. 

## <a name="next-steps"></a>Sonraki adımlar

Bir sonraki adım olarak, örnek verileri ve yönergeleri kullanarak ilk bilgi deponuzu oluşturun.

> [!div class="nextstepaction"]
> [Bilgi deposu oluşturma](knowledge-store-howto.md).
