---
title: Bilgi deposundaki projeksiyonlar (önizleme)
titleSuffix: Azure Cognitive Search
description: Zenginleştirilmiş verilerinizi AI zenginleştirme dizini boru hattından tam metin arama dışındaki senaryolarda kullanılmak üzere bir bilgi deposuna kaydedin ve şekillendirin. Bilgi deposu şu anda genel önizlemede.
manager: nitinme
author: vkurpad
ms.author: vikurpad
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/08/2020
ms.openlocfilehash: d264768bf27967d1a778400ae4e9e6f2e054d746
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78942980"
---
# <a name="projections-in-a-knowledge-store-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da bir bilgi deposunda projeksiyonlar

> [!IMPORTANT] 
> Bilgi deposu şu anda genel önizlemede. Önizleme işlevi hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın. [REST API sürümü 2019-05-06-Önizleme](search-api-preview.md) önizleme özellikleri sağlar. Şu anda sınırlı portal desteği ve .NET SDK desteği yoktur.

Azure Bilişsel Arama, dizin oluşturmanın bir parçası olarak yerleşik bilişsel beceriler ve özel beceriler aracılığıyla içerik zenginleştirme sağlar. Zenginleştirmeler, daha önce hiç kimsenin var olmadığı yeni bilgiler oluşturur: resimlerden bilgi ayıklamak, duyguları algılamak, anahtar tümcecikleri ve varlıkları metinden, birkaçı adlandırmak için. Zenginleştirmeler, farklılaşmamış metne yapı da ekler. Tüm bu işlemler, tam metin aramayı daha etkili hale getiren belgelerle sonuçlanır. Birçok durumda, zenginleştirilmiş belgeler, bilgi madenciliği gibi arama dışındaki senaryolar için yararlıdır.

Projeksiyonlar, bilgi [deposunun](knowledge-store-concept-intro.md)bir bileşeni, bilgi madenciliği amacıyla fiziksel depolama kaydedilebilir zenginleştirilmiş belgelerin görünümleridir. Projeksiyon, verilerinizi gereksinimlerinizle uyumlu bir şekle "yansıtmanızı" sağlar ve Power BI gibi araçların verileri ek bir çaba harcamadan okuyabilmesi için ilişkileri korur.

Azure Tablo depolamasında satır ve sütunlarda depolanan veriler veya Azure Blob depolama alanında depolanan JSON nesneleri ile projeksiyonlar tablolu olabilir. Zenginleştirilirken verilerinizin birden çok projeksiyonunu tanımlayabilirsiniz. Aynı verilerin tek tek kullanım örnekleri için farklı biçimde biçimlendirin.

Bilgi deposu üç tür projeksiyonu destekler:

+ **Tablolar**: Satır lar ve sütunlar olarak en iyi şekilde temsil edilen veriler için tablo projeksiyonları Tablo depolama alanında şematize edilmiş bir şekil veya projeksiyon tanımlamanıza olanak sağlar. Yalnızca geçerli JSON nesneleri tablo olarak yansıtılabilir, zenginleştirilmiş belge JSON nesneleri olarak adlandırılmayan düğümler içerebilir ve bu nesneleri yansıtırken, şekillendirici becerisi veya satır altı şekillendirme ile geçerli bir JSON nesnesi oluşturabilir.

+ **Nesneler**: Verilerinizin ve zenginleştirmelerinizin JSON temsiline ihtiyaç duyduğunuzda, nesne projeksiyonları blob olarak kaydedilir. Yalnızca geçerli JSON nesneleri nesne olarak yansıtılabilir, zenginleştirilmiş belge JSON nesneleri olarak adlandırılmayan düğümler içerebilir ve bu nesneleri yansıtırken, şekillendirici becerisi veya satır altı şekillendirme ile geçerli bir JSON nesnesi oluşturabilir.

+ **Dosyalar**: Belgelerden çıkarılan görüntüleri kaydetmeniz gerektiğinde, dosya projeksiyonları normalleştirilmiş görüntüleri blob depolamaalanına kaydetmenize olanak sağlar.

Bağlamda tanımlanan projeksiyonları görmek için [REST'te bir bilgi deposu oluştur'a](knowledge-store-create-rest.md)geçin.

## <a name="projection-groups"></a>Projeksiyon grupları

Bazı durumlarda, farklı hedeflere ulaşmak için zenginleştirilmiş verilerinizi farklı şekillerde yansıtmanız gerekir. Bilgi deposu, birden çok projeksiyon grubu tanımlamanıza olanak tanır. Projeksiyon grupları karşılıklı münhasırlık ve ilgililiğin aşağıdaki temel özelliklerine sahiptir.

### <a name="mutual-exclusivity"></a>Karşılıklı münhasırlık

Tek bir gruba yansıtılan tüm içerik, diğer projeksiyon gruplarına yansıtılan verilerden bağımsızdır.
Bu bağımsızlık, aynı verilerin farklı biçimde şekillendirilen, ancak her projeksiyon grubunda tekrarlanınabileceği anlamına gelir.

### <a name="relatedness"></a>İlgililik

Projeksiyon grupları artık projeksiyon türleri arasındaki ilişkileri korurken belgelerinizi projeksiyon türlerine yansıtmanızı sağlar. Tek bir projeksiyon grubu içinde yansıtılan tüm içerik, projeksiyon türleri arasındaki veriler içindeki ilişkileri korur. Tablolar içinde, ilişkiler oluşturulan bir anahtara dayanır ve her alt düğüm üst düğüme bir başvuru tutar. Türler arasında (tablolar, nesneler ve dosyalar) tek bir düğüm farklı türlere yansıtıldığında ilişkiler korunur. Örneğin, resim ve metin içeren bir belgenin olduğu bir senaryo düşünün. Metni tablolara veya nesnelere, görüntüleri de tabloların veya nesnelerin dosya URL'sini içeren bir sütuna/özelliğine sahip olduğu dosyalara yansıtabilirsiniz.

## <a name="input-shaping"></a>Giriş şekillendirme

Verilerinizi doğru şekle veya yapıya getirmek, tablolar veya nesneler olsun, etkili bir kullanım için anahtardır. Verilerinizi nasıl erişmeyi ve kullanmayı planladığınıza göre şekillendirme veya yapılandırma becerisi, beceri içinde **Shaper** becerisi olarak ortaya çıkarılan önemli bir yetenektir.  

Zenginleştirme ağacında projeksiyon şemasıyla eşleşen bir nesne olduğunda projeksiyonları tanımlamak daha kolaydır. Güncelleştirilmiş [Shaper becerisi,](cognitive-search-skill-shaper.md) zenginleştirme ağacının farklı düğümlerinden bir nesne oluşturmanıza ve bunları yeni bir düğüm altında ebeveyne oluşturmanıza olanak tanır. **Şekillendirici** becerisi, iç içe nesnelerle karmaşık türleri tanımlamanıza olanak tanır.

Yansıtmanız gereken tüm öğeleri içeren yeni bir şekil tanımlandığında, artık bu şekli projeksiyonlarınız için kaynak olarak veya başka bir beceriye giriş olarak kullanabilirsiniz.

## <a name="projection-slicing"></a>Projeksiyon dilimleme

Bir projeksiyon grubu tanımlanırken, zenginleştirme ağacındaki tek bir düğüm birden çok ilgili tablo veya nesne olarak dilimlenebilir. Varolan bir projeksiyonun alt öğesi olan bir kaynak yolu içeren bir projeksiyon eklemek, alt düğümün üst düğümden dilimlenerek yeni ancak ilişkili tabloya veya nesneye yansıtılan olmasına neden olur. Bu teknik, tüm projeksiyonlar için kaynak olabilir bir şekillendirici beceri tek bir düğüm tanımlamanızı sağlar.

## <a name="table-projections"></a>Tablo projeksiyonları

Alma işlemi nin kolaylaştırılmasından, Power BI ile veri arama için tablo projeksiyonları öneririz. Ayrıca, tablo projeksiyonları tablo ilişkileri arasındaki önemliliği değiştirmeye olanak sağlar. 

Dizininizdeki tek bir belgeyi ilişkileri koruyarak birden çok tabloya yansıtabilirsiniz. Birden çok tabloya yansıtılırken, alt düğüm aynı grup taki başka bir tablonun kaynağı olmadığı sürece, tam şekil her tabloya yansıtılır.

### <a name="defining-a-table-projection"></a>Tablo projeksiyonu tanımlama

Skillset `knowledgeStore` öğesi içinde bir tablo projeksiyon tanımlarken, tablo kaynağına zenginleştirme ağacı üzerinde bir düğüm eşleyerek başlayın. Genellikle bu düğüm, tablolara yansıtmanız gereken belirli bir şekil üretmek için beceriler listesine eklediğiniz bir **Şekillendirici** becerisinin çıktısI. Projelemeyi seçtiğiniz düğüm, birden çok tabloya yansıtılmak üzere dilimlenebilir. Tablo tanımı, yansıtmak istediğiniz tabloların listesidir.

Her tablo üç özellik gerektirir:

+ tablo Adı: Azure Depolama'daki tablonun adı.

+ oluşturulanKeyName: Bu satırı benzersiz olarak tanımlayan anahtarın sütun adı.

+ kaynak: Zenginleştirme ağacından gelen düğüm. Bu düğüm genellikle bir şekillendiricinin çıktısI, ancak herhangi bir beceri çıktısı olabilir.

Aşağıda tablo projeksiyonlarına bir örnek verilmiştir.

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

Bu örnekte gösterildiği gibi, anahtar tümcecikler ve varlıklar farklı tablolarhalinde modellenir ve her satır için üst tabloya (Ana Tablo) bir başvuru içerir.

## <a name="object-projections"></a>Nesne projeksiyonları

Nesne projeksiyonları, herhangi bir düğümden kaynaklanabilen zenginleştirme ağacının JSON gösterimleridir. Çoğu durumda, bir nesne projeksiyonu oluşturmak için bir tablo projeksiyonu oluşturan aynı **Şekillendirici** becerisi kullanılabilir. 

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

Nesne projeksiyonu oluşturmak birkaç nesneye özgü öznitelik gerektirir:

+ storageContainer: Nesnelerin kaydedileceği blob konteyneri
+ kaynak: Projeksiyonun kökü olan zenginleştirme ağacının düğümüne giden yol

## <a name="file-projection"></a>Dosya projeksiyonu

Dosya projeksiyonları nesne projeksiyonlarına benzer ve `normalized_images` yalnızca koleksiyonda hareket eder. Nesne projeksiyonlarına benzer şekilde, dosya projeksiyonları belge kimliğinin base64 kodlanmış değerinin klasör önekiyle blob kapsayıcısına kaydedilir. Dosya projeksiyonları nesne projeksiyonları ile aynı kapsayıcıyı paylaşamaz ve farklı bir kapsayıcıya yansıtılması gerekir.

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

Projeksiyonlarınız, veri kaynağınızdaki kaynak verilere bağlı bir yaşam döngüsüne sahiptir. Verileriniz güncelleştirildikçe ve yeniden dizine eklendikçe, tahminleriniz, projeksiyonlarınızın sonunda veri kaynağınızdaki verilerle tutarlı olmasını sağlayan zenginleştirmelerin sonuçlarıyla güncellenir. Projeksiyonlar, dizininiz için yapılandırdığınız silme ilkesini devralır. Dizinleyici veya arama hizmetinin kendisi silindiğinde projeksiyonlar silinmez.

## <a name="using-projections"></a>Projeksiyonları kullanma

Dizinleyici çalıştırıldıktan sonra, projeksiyonlar aracılığıyla belirttiğiniz kapsayıcılarda veya tablolarda öngörülen verileri okuyabilirsiniz.

Analitik için Power BI'deki keşifler, Veri kaynağı kadar Azure Table depolamasını ayarlamak kadar basittir. İçindeki ilişkileri kullanarak verilerinizde kolayca bir dizi görselleştirme oluşturabilirsiniz.

Alternatif olarak, zenginleştirilmiş verileri bir veri bilimi ardışık alanında kullanmanız gerekiyorsa, [lekelerdeki verileri Pandas DataFrame'e yükleyebilirsiniz.](../machine-learning/team-data-science-process/explore-data-blob.md)

Son olarak, verilerinizi bilgi deposundan dışa aktarmanız gerekiyorsa, Azure Veri Fabrikası verileri dışa aktarmak ve seçtiğiniz veritabanına aktarmak için bağlayıcılara sahiptir. 

## <a name="next-steps"></a>Sonraki adımlar

Bir sonraki adım olarak, örnek verileri ve yönergeleri kullanarak ilk bilgi deponuzu oluşturun.

> [!div class="nextstepaction"]
> [REST'te bir bilgi deposu oluşturun.](knowledge-store-create-rest.md)

Dilimleme, satır ara şekillendirme ve ilişkiler gibi gelişmiş projeksiyonkavramlarını kapsayan bir öğretici için, [bir bilgi deposundaki projeksiyonları tanımlamaile](knowledge-store-projections-examples.md) başlayın

> [!div class="nextstepaction"]
> [Projeksiyonları bilgi deposunda tanımlama](knowledge-store-projections-examples.md)
