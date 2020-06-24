---
title: Projeksiyon kavramları (Önizleme)
titleSuffix: Azure Cognitive Search
description: Zenginleştirme veri dizini oluşturma işlem hattından zenginleştirilmiş verilerinizi tam metin arama dışındaki senaryolarda kullanmak üzere bir bilgi deposuna kaydedin ve şekillendirin. Bilgi deposu Şu anda genel önizleme aşamasındadır.
manager: nitinme
author: vkurpad
ms.author: vikurpad
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/08/2020
ms.openlocfilehash: 894fc6efc99eb7fcc17f2199270c08bc3cee8e8e
ms.sourcegitcommit: 4ac596f284a239a9b3d8ed42f89ed546290f4128
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84750324"
---
# <a name="knowledge-store-projections-in-azure-cognitive-search"></a>Azure Bilişsel Arama bilgi deposu "projeksiyonlarını"

> [!IMPORTANT] 
> Bilgi deposu Şu anda genel önizleme aşamasındadır. Önizleme işlevselliği, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). [REST API sürüm 2019-05-06-önizleme](search-api-preview.md) , Önizleme özellikleri sağlar. Şu anda sınırlı sayıda portal desteği var ve .NET SDK desteği yok.

Azure Bilişsel Arama, dizin oluşturmanın bir parçası olarak yerleşik bilişsel yetenekler ve özel yetenekler aracılığıyla içerik zenginleştirme imkanı sunar. Zenginleştirme, daha önce hiçbir yerde bulunmayan yeni bilgiler oluşturur: görüntülerden bilgi ayıklama, yaklaşım, anahtar ifadeler ve metinden varlıkları birkaç kez adlandırma. Zenginleştirme Ayrıca, ayırt edilmemiş metne yapı ekler. Bu işlemlerin tümü, tam metin araması yapan belgelerde oluşur. Birçok örnekte, bilgi madenciliği gibi arama dışındaki senaryolar için zenginleştirilmiş belgeler yararlıdır.

[Bilgi deposunun](knowledge-store-concept-intro.md)bir bileşeni olan tahminler, bilgi madenciliği için fiziksel depolamaya kaydedilebilen zenginleştirilmiş belgelerin görünümleridir. Bir projeksiyon, verilerinizi, Power BI gibi araçların ek bir çaba olmadan okuyabilmesi için, ilişkilerini koruyarak, verilerinizi gereksinimlerinize göre hizalayan bir şekle "proje" sağlar.

Tahminler, Azure Tablo depolamada veya Azure Blob depolama alanında depolanan JSON nesnelerinde bulunan satırlar ve sütunlarda depolanan verilerle tablosal olabilir. Verilerinizin zenginleştirmekte olduğu haliyle birden fazla projeksiyoni tanımlayabilirsiniz. Birden çok projeksiyonlar ayrı kullanım durumları için aynı verinin farklı şekilde olmasını istediğinizde faydalıdır.

Bilgi deposu üç tür projeksiyonları destekler:

+ **Tablolar**: satır ve sütun olarak en iyi şekilde temsil edilen veriler için tablo projeksiyonları tablo depolamada şema veya projeksiyon tanımlamanızı sağlar. Yalnızca geçerli JSON nesneleri tablo olarak yansıtılmışsa, zenginleştirilmiş belge JSON nesneleri adında olmayan düğümler içerebilir ve bu nesneleri yansıtırken, Beceri başına veya satır içi şekillendirme ile geçerli bir JSON nesnesi oluşturun.

+ **Nesneler**: verileriniz ve zenginleştirme IÇIN bir JSON temsiline ihtiyacınız olduğunda, nesne projeksiyonlar blob olarak kaydedilir. Yalnızca geçerli JSON nesneleri nesneler olarak yansıtılmışsa, zenginleştirilmiş belge JSON nesneleri adında olmayan düğümler içerebilir ve bu nesneleri yansıtırken, Beceri başına veya satır içi şekillendirme ile geçerli bir JSON nesnesi oluşturun.

+ **Dosyalar**: belgelerden ayıklanan görüntüleri kaydetmeniz gerektiğinde, dosya projeksiyonlar, normalleştirilmiş görüntüleri blob depolamaya kaydetmenizi sağlar.

Bağlamda tanımlanan projeksiyonları görmek için, [bekleyen bir bilgi deposu oluşturun](knowledge-store-create-rest.md).

## <a name="projection-groups"></a>Projeksiyon grupları

Bazı durumlarda, farklı hedefleri karşılamak için zenginleştirilmiş verilerinizi farklı şekillerde proje yapmanız gerekir. Bilgi deposu, birden çok projeksiyonun grubunu tanımlamanızı sağlar. Projeksiyon grupları, karşılıklı denetim ve related'ın aşağıdaki temel özelliklerine sahiptir.

### <a name="mutual-exclusivity"></a>Karşılıklı denetim

Tek bir grupta yansıtılan tüm içerikler, diğer projeksiyon gruplarında yansıtılan verilerden bağımsızdır.
Bu bağımsızlık, aynı verilerin şekillendirilmiş bir şekilde farklı, ancak her projeksiyon grubunda tekrarlanabileceği anlamına gelir.

### <a name="relatedness"></a>Relatedlik

Projeksiyon grupları artık, yansıtma türlerinde ilişkileri korurken belgelerinizi İzdüşüm türleri arasında projenize eklemenize olanak tanır. Tek bir projeksiyon grubu içinde yansıtılan tüm içerik, yansıtma türlerinde verilerin içindeki ilişkileri korur. Tablolar içinde, ilişkiler oluşturulan bir anahtarı temel alır ve her alt düğüm üst düğüme bir başvuru tutar. Türler arasında (tablolar, nesneler ve dosyalar), tek bir düğüm farklı türler arasında yansıtıldığınızda ilişkiler korunur. Örneğin, resim ve metin içeren bir belge olduğu bir senaryo düşünün. Metni tablo veya nesnelere ve resimlerin veya nesnelerin dosya URL 'sini içeren bir sütuna/özelliğe sahip olduğu dosyalara yansıt edebilirsiniz.

## <a name="input-shaping"></a>Giriş şekillendirme

Verilerinizin doğru şekilde veya yapıda alınması, etkin kullanım için anahtar, BT tabloları veya nesneleri olmalıdır. Verilerinizi nasıl kullanabileceğinizi ve kullanabileceğinizi temel alarak verilerinizi şekillendirebilir veya yapısal hale getirme özelliği, Beceri içinde her yetenek için **mil** olarak kullanıma sunulan bir temel yetenektir.  

Projeksiyon şeması ile eşleşen zenginleştirme ağacında bir nesneniz olduğunda, projeksiyonu daha kolay tanımlanır. [Her yetenek](cognitive-search-skill-shaper.md) Için güncelleştirilmiş mil, zenginleştirme ağacının farklı düğümlerinden bir nesne oluşturmanıza ve bunların üstünü yeni bir düğüm altında oluşturmanıza olanak sağlar. **Mil başına** , iç içe geçmiş nesnelerle karmaşık türler tanımlamanıza olanak sağlar.

Projeniz için gereken tüm öğeleri içeren yeni bir şekil tanımladığınız zaman, artık bu şekli projeksiyonlarınızın kaynağı olarak veya başka bir beceriye giriş olarak kullanabilirsiniz.

## <a name="projection-slicing"></a>Projeksiyon Dilimleme

Yansıtma grubu tanımlarken, zenginleştirme ağacındaki tek bir düğüm birden çok ilişkili tabloya veya nesneye dilimlenebilir. Mevcut projeksiyonun alt düğümü olan bir kaynak yolu olan bir projeksiyon eklemek, alt düğümün üst düğümden dilimlendirime ve yeni, bununla ilgili yeni tablo ya da nesne ile yansıtılmakta olacaktır. Bu teknik, tüm projeksiyonlarınızın kaynağı olabilecek beceri başına bir mil içinde tek bir düğüm tanımlamanızı sağlar.

## <a name="table-projections"></a>Tablo projeksiyonlarını

İçeri aktarma işlemi daha da kolaylaştırdığı için Power BI ile veri araştırması için tablo projeksiyonlarını öneririz. Ayrıca, tablo tahminleri tablo ilişkileri arasındaki kardinalite değiştirilmesini sağlar. 

Dizininizdeki tek bir belgeyi birden çok tabloya proje ekleyebilirsiniz ve ilişkileri korur. Birden çok tabloya yansıtıldığınızda, bir alt düğüm aynı grup içindeki başka bir tablonun kaynağı değilse, tüm şekil her tabloya yansıtılır.

### <a name="defining-a-table-projection"></a>Tablo projeksiyonu tanımlama

Beceri öğesi içinde tablo projeksiyonu tanımlarken `knowledgeStore` , zenginleştirme ağacındaki bir düğümü tablo kaynağına eşleyerek başlayın. Genellikle bu düğüm, tablolarda proje yapmanız gereken belirli bir şekli oluşturmak için yetenekler listesine eklediğiniz **her** bir beceriye ait çıktıdır. Projeyi seçtiğiniz düğüm birden çok tablo halinde projeye dilimlenebilir. Tablolar tanımı, proje yapmak istediğiniz tabloların bir listesidir.

Her tablo üç özellik gerektirir:

+ tableName: Azure Storage 'daki tablonun adı.

+ generatedKeyName: Bu satırı benzersiz bir şekilde tanımlayan anahtarın sütun adı.

+ Kaynak: zenginleştirme ağacınızdaki düğüm, kendi zenginleştirmelerinin kaynağını oluşturur. Bu düğüm genellikle biçimlendiricilerin çıktıdır, ancak yeteneklerin herhangi birinin çıktısı olabilir.

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
          "objects": [ ]
        },
        {
            "files": [ ]
        }
      ]
    }
}
```

Bu örnekte gösterildiği gibi, anahtar tümcecikler ve varlıklar farklı tablolara modellenir ve her satır için üst (MainTable) öğesine geri bir başvuru içerir.

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
              "storageContainer": "hotelreviews", 
              "source": "/document/hotel"
            }
          ]
        },
        {
            "files": [ ]
        }
      ]
    }
}
```

Nesne projeksiyonu oluşturmak, nesneye özgü birkaç özniteliği gerektirir:

+ storageContainer: nesnelerin kaydedileceği blob kapsayıcısı
+ Kaynak: projeksiyon kökü olan zenginleştirme ağacının düğümünün yolu

## <a name="file-projection"></a>Dosya projeksiyonu

Dosya projeksiyonlar nesne projeksiyona benzer ve yalnızca koleksiyonda işlem görür `normalized_images` . Nesne projeksiybunlara benzer şekilde dosya projeksiyonları, blob kapsayıcısına belge KIMLIĞININ Base64 kodlamalı değerinin klasör öneki ile kaydedilir. Dosya projeksiyonlar, nesne projeksiyonları ile aynı kapsayıcıyı paylaşamaz ve farklı bir kapsayıcıya yansıtılmalıdır.

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
          "objects": [ ]
        },
        {
            "files": [
                 {
                  "storageContainer": "ReviewImages",
                  "source": "/document/normalized_images/*"
                }
            ]
        }
      ]
    }
}
```

## <a name="projection-lifecycle"></a>Projeksiyon yaşam döngüsü

Projeksiyonlarınızın veri kaynağınızdaki kaynak verilere bağlı bir yaşam döngüsü vardır. Verileriniz güncelleştirildiğinden ve yeniden dizinleniyorsa, tahminleriniz, projeksiyonlarınızın veri kaynağınızdaki verilerle tutarlı olmasını sağlayan zenginlerin sonuçlarıyla güncelleştirilir. Tahminler, dizininiz için yapılandırdığınız silme ilkesini alırlar. Dizin Oluşturucu veya arama hizmeti silindiğinde, projeksiyonlar silinmez.

## <a name="using-projections"></a>Projeksiyonları kullanma

Dizin Oluşturucu çalıştırıldıktan sonra, tahminler aracılığıyla belirttiğiniz kapsayıcılardaki veya tablolardaki yansıtılan verileri okuyabilirsiniz.

Analiz için Power BI araştırma, Azure Tablo depolama alanını veri kaynağı olarak ayarlamak kadar basittir. İçindeki ilişkileri kullanarak verilerinize kolayca bir görselleştirme kümesi oluşturabilirsiniz.

Alternatif olarak, bir veri bilimi ardışık düzeninde zenginleştirilmiş verileri kullanmanız gerekiyorsa, [verileri bloblardan bir Pandas DataFrame 'e yükleyebilirsiniz](../machine-learning/team-data-science-process/explore-data-blob.md).

Son olarak, verilerinizi bilgi deposundan dışarı aktarmanız gerekiyorsa Azure Data Factory, verileri dışarı aktarmak ve seçtiğiniz veritabanına eklemek için bağlayıcılar içerir. 

## <a name="next-steps"></a>Sonraki adımlar

Bir sonraki adım olarak, örnek verileri ve yönergeleri kullanarak ilk bilgi deponuzu oluşturun.

> [!div class="nextstepaction"]
> [Bekleyen bir bilgi deposu oluşturun](knowledge-store-create-rest.md).

Dilimleme, satır içi şekillendirme ve ilişkiler gibi gelişmiş projeksiyonun kavramlarını kapsayan bir öğreticide, [bir bilgi deposunda projeksiyonları tanımlamaya](knowledge-store-projections-examples.md) başlayın

> [!div class="nextstepaction"]
> [Bir bilgi deposunda projeksiyonlarını tanımlama](knowledge-store-projections-examples.md)
