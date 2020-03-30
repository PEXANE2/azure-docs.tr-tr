---
title: Skillset kavramları ve iş akışı
titleSuffix: Azure Cognitive Search
description: Skillsets, Azure Bilişsel Arama'da bir AI zenginleştirme boru hattı yazdığınız yerdir. Skillset kompozisyonu hakkında önemli kavramları ve ayrıntıları öğrenin.
manager: nitinme
author: vkurpad
ms.author: vikurpad
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 8b45840215092281c7fbc8d499e26b095b374dd6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77191036"
---
# <a name="skillset-concepts-and-composition-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da skillset kavramları ve kompozisyonu

Bu makale, zenginleştirme boru hattının nasıl çalıştığına daha iyi bir anlayışa ihtiyaç duyan ve AI zenginleştirme süreci hakkında kavramsal bir anlayışa sahip olduğunuzu varsayan geliştiriciler içindir. Bu konseptyeniyseniz, şu şekilde başlayın:
+ [Azure Bilişsel Arama'da AI zenginleştirme](cognitive-search-concept-intro.md)
+ [Bilgi deposu (önizleme)](knowledge-store-concept-intro.md)

## <a name="specify-the-skillset"></a>Skillset'i belirtin
Skillset, Azure Bilişsel Arama'da, dizin oluşturma sırasında metin veya resim içeriğini analiz etmek, dönüştürmek ve zenginleştirmek için kullanılan bilişsel beceriler koleksiyonunu belirten yeniden kullanılabilir bir kaynaktır. Bir beceri oluşturmak, veri alma aşamasında metin ve görüntü zenginleştirmeleri eklemenize, ham içerikten yeni bilgi ve yapılar çıkarmanızı ve oluşturmanıza olanak tanır.

Bir skillset'in üç özelliği vardır:

+   ```skills```, platformun her beceri için gerekli girdilere göre yürütme sırasını belirlediği sıralanmamış bir beceri koleksiyonu
+   ```cognitiveServices```, bilişsel hizmetler anahtarı çağrılan bilişsel becerileri fatura için gerekli
+   ```knowledgeStore```, zenginleştirilmiş belgelerinizin yansıtılacağı depolama hesabı



Skillsets JSON yılında yazılır. [İfade dilini](https://docs.microsoft.com/azure/search/cognitive-search-skill-conditional)kullanarak döngü ve [dallanma](https://docs.microsoft.com/azure/search/cognitive-search-skill-conditional) ile karmaşık beceri kümeleri oluşturabilirsiniz. İfade dili, zenginleştirme ağacındaki düğümleri tanımlamak için birkaç değişiklikle [JSON Pointer](https://tools.ietf.org/html/rfc6901) yol gösterimini kullanır. Bir ```"/"``` ağaçta bir düzey daha ```"*"``` düşük geçiş ve bağlamında her biri için bir işleç olarak davranır. Bu kavramlar en iyi bir örnekle açıklanır. Bazı kavramları ve yetenekleri göstermek için, [biz otel değerlendirmeleri örnek](knowledge-store-connect-powerbi.md) becerileri ile yürüyeceğiz. Alma veri iş akışını takip ettikten sonra skillset'i görüntülemek [için, skillset'i almak](https://docs.microsoft.com/rest/api/searchservice/get-skillset)için bir REST API istemcisi kullanmanız gerekir.

### <a name="enrichment-tree"></a>Zenginleştirme ağacı

Bir becerinin belgenizi aşamalı olarak nasıl zenginleştirdigini tasavvur etmek için, herhangi bir zenginleştirmeden önce belgenin nasıl göründüğüyle başlayalım. Belge çatlama çıktısı veri kaynağına ve seçilen belirli ayrıştırma moduna bağlıdır. Bu, [alan eşlemelerinin](search-indexer-field-mappings.md) arama dizinine veri eklerken içerik kaynağı olabileceği belgenin durumudur.
![Boru hattı diyagramında bilgi deposu](./media/knowledge-store-concept-intro/annotationstore_sans_internalcache.png "Boru hattı diyagramında bilgi deposu")

Bir belge zenginleştirme boru hattına girdiğinde, bir içerik ağacı ve ilişkili zenginleştirmeler olarak temsil edilir. Bu ağaç, belge çatlama çıktısı olarak anında. Zenginleştirme ağacı biçimi zenginleştirme ardışık lığı bile ilkel veri türlerine meta veri eklemek için izin, geçerli bir JSON nesnesi değildir ama geçerli bir JSON biçimine yansıtılabilir. Aşağıdaki tablo, zenginleştirme ardışık bölümüne giren bir belgenin durumunu gösterir:

|Veri Kaynağı\Ayrışma Modu|Varsayılan|JSON, JSON Hatları & CSV|
|---|---|---|
|Blob Depolama|/belge/içerik<br>/belge/normalized_images/*<br>…|/document/{key1}<br>/document/{key2}<br>…|
|SQL|/document/{column1}<br>/document/{column2}<br>…|Yok |
|Cosmos DB|/document/{key1}<br>/document/{key2}<br>…|Yok|

 Beceriler yürütülderken, zenginleştirme ağacına yeni düğümler eklerler. Bu yeni düğümler daha sonra alt akış becerileri için girişler olarak, bilgi deposuna yansıtma veya dizin alanlarına eşleme olarak kullanılabilir. Zenginleştirmeler değişmez: oluşturulduktan sonra düğümler düzenlenemez. Sizin skillsets daha karmaşık olsun, bu yüzden zenginleştirme ağacı, ancak zenginleştirme ağacında tüm düğümleri dizin veya bilgi deposu yapmak gerekir. 

Yalnızca dizin veya bilgi deposuna zenginleştirmelerin bir alt kümesini seçen devam edebilirsiniz.
Bu belgenin geri kalanı için, biz [otel değerlendirmeleri örnek](https://docs.microsoft.com/azure/search/knowledge-store-connect-powerbi)ile çalışıyoruz varsayacağız, ancak aynı kavramlar diğer tüm veri kaynaklarından belgeleri zenginleştirmek için geçerlidir.

### <a name="context"></a>Bağlam
Her beceri bir bağlam gerektirir. Bir bağlam belirler:
+   Seçilen düğümlere bağlı olarak becerinin kaç kez yürütüldürün sayısı. Tür koleksiyonunun bağlam değerleri ```/*``` için, sonunda bir tane eklemek, koleksiyonun her örneği için bir kez çağrılan beceriyle sonuçlanır. 
+   Zenginleştirme ağacında beceri çıktıları eklenir. Çıktılar her zaman bağlam düğümünün çocukları olarak ağaca eklenir. 
+   Girdilerin şekli. Çok düzeyli koleksiyonlarda, bağlamı ana koleksiyona ayarlamak, beceri girişinin şeklini etkiler. Örneğin, her biri posta kodları nın listesini içeren durumların listesiyle zenginleştirilmiş bir ülke listesi içeren bir zenginleştirme ağacınız varsa.

|Bağlam|Giriş|Giriş Şekli|Beceri Çağırma|
|---|---|---|---|
|```/document/countries/*``` |```/document/countries/*/states/*/zipcodes/*``` |Ülkedeki tüm posta kodlarının listesi |Ülke başına bir kez |
|```/document/countries/*/states/*``` |```/document/countries/*/states/*/zipcodes/*``` |Eyaletteki posta kodlarının listesi | Ülke ve devlet kombinasyonu başına bir kez|

### <a name="sourcecontext"></a>Kaynak Bağlam

Sadece `sourceContext` beceri girişleri ve [projeksiyonları](knowledge-store-projection-overview.md)kullanılır. Çok düzeyli, iç içe nesneler oluşturmak için kullanılır. Bilgi deposuna bir beceri veya proje girişi olarak aktarmak için yeni bir nesne oluşturmanız gerekebilir. Zenginleştirme düğümleri zenginleştirme ağacında geçerli bir JSON nesnesi olmayabilir ve ağaçtaki bir düğüme atıfta bulunulduğundan, zenginleştirmeleri beceri girdileri veya projeksiyonları olarak kullanarak, yalnızca oluşturulduğunda düğümün durumu, iyi biçimlendirilmiş bir JSON nesnesi oluşturmanızı gerektirir. Yalnızca `sourceContext` bağlamı kullanıyorsanız birden çok beceri gerektiren hiyerarşik, anonim bir tür nesne oluşturmanıza olanak sağlar. Kullanma `sourceContext` sonraki bölümde gösterilir. İlkel bir tür değil, geçerli bir JSON nesnesi olup olmadığını belirlemek için bir zenginleştirme oluşturulan beceri çıktısına bakın.

### <a name="projections"></a>Yansıtmalar

Projeksiyon, bilgi deposunda kaydedilecek zenginleştirme ağacından düğümleri seçme işlemidir. Projeksiyonlar, tablo veya nesne projeksiyonları olarak çıktı alınabilen belgenin özel şekilleridir (içerik ve zenginleştirmeler). Projeksiyonlarla çalışma hakkında daha fazla bilgi edinmek [için, projeksiyonlarla çalışma](knowledge-store-projection-overview.md)hakkında bilgi edinin.

![Alan eşleme seçenekleri](./media/cognitive-search-working-with-skillsets/field-mapping-options.png "Zenginleştirme boru hattı için alan haritalama seçenekleri")

Yukarıdaki diyagram, zenginleştirme ardışık alanında nerede olduğunuza bağlı olarak birlikte çalıştığınız seçiciyi açıklar.

## <a name="generate-enriched-data"></a>Zenginleştirilmiş veriler oluşturma 

Şimdi otel değerlendirmeleri skillset üzerinden adım edelim, beceri oluşturmak veya skillset [görüntülemek](https://github.com/Azure-Samples/azure-search-postman-samples/blob/master/samples/skillset.json) için [öğretici](knowledge-store-connect-powerbi.md) takip edebilirsiniz. Biz bakacağız:

* zenginleştirme ağacı her beceri nin yürütülmesi ile nasıl gelişir 
* bağlam ve girdilerin bir becerinin kaç kez yürütüldünlerini belirlemek için nasıl çalıştığı 
* girdinin şekli nin içeriğe dayandığı. 

Dizinleyici için sınırlı metin ayrıştma modunu kullandığımızdan, zenginleştirme işlemi içindeki bir belge CSV dosyasıiçinde tek bir satırı temsil eder.

### <a name="skill-1-split-skill"></a>Beceri #1: Bölünmüş beceri 

![belge çatlama sonra zenginleştirme ağacı](media/cognitive-search-working-with-skillsets/enrichment-tree-doc-cracking.png "Belge çatlama sonra zenginleştirme ağacı ve beceri yürütme önce")

Beceri ```"/document/reviews_text"```bağlamında, bu beceri için bir `reviews_text`kez yürütecek . Beceri çıktısı, 5000 karakter segmentine bölündüğü bir `reviews_text` listedir. Bölme becerisinden elde edilen `pages` çıktı adlanır ve zenginleştirme ağacına eklenir. Bu `targetName` özellik, zenginleştirme ağacına eklenmeden önce bir beceri çıktısını yeniden adlandırmanızı sağlar.

Zenginleştirme ağacı artık beceri bağlamında yerleştirilen yeni bir düğüm vardır. Bu düğüm herhangi bir beceri, projeksiyon veya çıktı alanı eşleme için kullanılabilir.


Tüm zenginleştirmeler için kök `"/document"`düğüm . Blob dizinleyiciler ile `"/document"` çalışırken, düğüm ve . `"/document/content"` `"/document/normalized_images"` Bu örnekte olduğu gibi CSV verileriyle çalışırken, sütun adları altında `"/document"`düğümlerle eşlenecektir. Bir beceri tarafından düğüme eklenen zenginleştirmelerden herhangi birini erişmek için, zenginleştirme için tam yol gereklidir. Örneğin, ```pages``` düğümdeki metni başka bir beceriye giriş olarak kullanmak istiyorsanız, bunu ```"/document/reviews_text/pages/*"```.
 
 ![beceri #1 sonra zenginleştirme ağacı](media/cognitive-search-working-with-skillsets/enrichment-tree-skill1.png "Beceri #1 sonra zenginleştirme ağacı yürütür")

### <a name="skill-2-language-detection"></a>Dil algılama becerisi #2
 Dil algılama becerisi beceri de tanımlanan üçüncü (beceri #3) beceri iken, yürütmek için bir sonraki beceridir. Herhangi bir girdi gerektirerek engellenmediğinden, önceki beceriile paralel olarak yürütülecektir. Kendisinden önceki bölme becerisi gibi, dil algılama becerisi de her belge için bir kez çağrılır. Zenginleştirme ağacı artık dil için yeni bir düğüm eve sahiptir.
 ![beceri #2 sonra zenginleştirme ağacı](media/cognitive-search-working-with-skillsets/enrichment-tree-skill2.png "Beceri #2 sonra zenginleştirme ağacı yürütür")
 
 ### <a name="skill-3-key-phrases-skill"></a>Beceri #3: Anahtar kalıplar beceri 

Anahtar tümceciklerin bağlamı ```/document/reviews_text/pages/*``` göz önüne alındığında, `pages` koleksiyondaki öğelerin her biri için bir kez çağrılacaktır. Beceri çıktısı ilişkili sayfa öğesi altında bir düğüm olacaktır. 

 Şimdi skillset becerileri geri kalanı bakmak ve zenginleştirme ağacı her beceri yürütülmesi ile büyümeye devam edeceğini görselleştirmek gerekir. Birleştirme becerisi ve şekillendirici becerisi gibi bazı beceriler de yeni düğümler oluşturur, ancak yalnızca varolan düğümlerden gelen verileri kullanır ve net yeni zenginleştirmeler oluşturmaz.

![tüm beceriler sonra zenginleştirme ağacı](media/cognitive-search-working-with-skillsets/enrichment-tree-final.png "Tüm becerilerden sonra zenginleştirme ağacı")

Yukarıdaki ağaçtaki bağlayıcıların renkleri, zenginleştirmelerin farklı beceriler tarafından oluşturulduğunu ve düğümlerin tek tek ele alınması gerekeceğini ve üst düğümü seçerken döndürülen nesnenin bir parçası olmayacağını gösterir.

## <a name="save-enrichments-in-a-knowledge-store"></a>Bir bilgi deposunda zenginleştirmeleri kaydedin 

Beceri kümeleri, zenginleştirilmiş belgelerinizin tablo veya nesne olarak yansıtılabileceği bir bilgi deposu da tanımlar. Zenginleştirilmiş verilerinizi bilgi deposuna kaydetmek için, zenginleştirilmiş belgeniz için bir dizi projeksiyon tanımlarsınız. Bilgi deposu hakkında daha fazla bilgi edinmek için [bilgi deposuna genel bakış](knowledge-store-concept-intro.md)

### <a name="slicing-projections"></a>Projeksiyonları dilimleme

Bir tablo projeksiyon grubu tanımlanırken, zenginleştirme ağacındaki tek bir düğüm birden çok ilgili tabloya dilimlenebilir. Varolan tablo projeksiyonunun alt bölümü olan bir kaynak yolu olan bir tablo eklerseniz, ortaya çıkan alt düğüm varolan tablo projeksiyonunun bir alt bölümü olmaz, bunun yerine yeni, ilgili tabloya yansıtılır. Bu dilimleme tekniği, tüm tablo projeksiyonlarınız için kaynak olabilecek bir şekillendirici becerisinde tek bir düğüm tanımlamanızı sağlar. 

### <a name="shaping-projections"></a>Projeksiyonları şekillendirme

Bir projeksiyonu tanımlamanın iki yolu vardır. Yansıttığınız tüm zenginleştirmeler için kök düğüm olan yeni bir düğüm oluşturmak için şekillendirici becerisi kullanabilirsiniz. Daha sonra, projeksiyonlarınızda, sadece şekillendirici becerisinin çıktısını referans alırsınız. Ayrıca, projeksiyon tanımının kendisi içinde bir projeksiyonu satır içi şeklinde de şekillendirebilirsiniz.

Şekillendirici yaklaşımı satır içi şekillendirmeden daha ayrıntılıdır, ancak zenginleştirme ağacının tüm mutasyonlarının beceriler içinde yer almasını ve çıktının yeniden kullanılabilecek bir nesne olmasını sağlar. Satır altı şekillendirme, gereksinim duyduğunuz şekli oluşturmanıza olanak sağlar, ancak anonim bir nesnedir ve yalnızca tanımlandığı projeksiyon için kullanılabilir. Yaklaşımlar birlikte veya ayrı ayrı kullanılabilir. Portal iş akışında sizin için oluşturulan beceri her ikisini de içerir. Tablo projeksiyonları için bir şekillendirici becerisi kullanır, ancak anahtar tümcecikleri tablosunu yansıtmak için satır satır lı şekillendirme kullanır.

Örneği genişletmek için satır ara biçimlendirmeyi kaldırmayı ve anahtar tümcecikler için yeni bir düğüm oluşturmak için şekillendirici becerisini kullanmayı seçebilirsiniz. Üç tabloya yansıtılan bir şekil `hotelReviewsDocument`oluşturmak `hotelReviewsPages`için, yani , , ve `hotelReviewsKeyPhrases`, iki seçenek aşağıdaki bölümlerde açıklanmıştır.


#### <a name="shaper-skill-and-projection"></a>Şekillendirici beceri ve projeksiyon 

> [!Note]
> Belge tablosundaki sütunlardan bazıları kısalık nedeniyle bu örnekten kaldırılmıştır.
>
```json
{
    "@odata.type": "#Microsoft.Skills.Util.ShaperSkill",
    "name": "#5",
    "description": null,
    "context": "/document",
    "inputs": [        
        {
            "name": "reviews_text",
            "source": "/document/reviews_text",
            "sourceContext": null,
            "inputs": []
        },
        {
            "name": "reviews_title",
            "source": "/document/reviews_title",
            "sourceContext": null,
            "inputs": []
        },
        {
            "name": "AzureSearch_DocumentKey",
            "source": "/document/AzureSearch_DocumentKey",
            "sourceContext": null,
            "inputs": []
        },  
        {
            "name": "pages",
            "source": null,
            "sourceContext": "/document/reviews_text/pages/*",
            "inputs": [
                {
                    "name": "SentimentScore",
                    "source": "/document/reviews_text/pages/*/Sentiment",
                    "sourceContext": null,
                    "inputs": []
                },
                {
                    "name": "LanguageCode",
                    "source": "/document/Language",
                    "sourceContext": null,
                    "inputs": []
                },
                {
                    "name": "Page",
                    "source": "/document/reviews_text/pages/*",
                    "sourceContext": null,
                    "inputs": []
                },
                {
                    "name": "keyphrase",
                    "sourceContext": "/document/reviews_text/pages/*/Keyphrases/*",
                    "inputs": [
                        {
                            "source": "/document/reviews_text/pages/*/Keyphrases/*",
                            "name": "Keyphrases"
                        }
                    ]
                }
            ]
        }
    ],
    "outputs": [
        {
            "name": "output",
            "targetName": "tableprojection"
        }
    ]
}
```

`tableprojection` Yukarıdaki `outputs` bölümde tanımlanan düğümle, `tableprojection` düğümün parçalarını farklı tablolara yansıtmak için dilimleme özelliğini kullanabiliriz:

> [!Note]
> Bu, bilgi deposu yapılandırması içindeki projeksiyonun yalnızca bir bölümüdür.
>
```json
"projections": [
    {
        "tables": [
            {
                "tableName": "hotelReviewsDocument",
                "generatedKeyName": "Documentid",
                "source": "/document/tableprojection"
            },
            {
                "tableName": "hotelReviewsPages",
                "generatedKeyName": "Pagesid",
                "source": "/document/tableprojection/pages/*"
            },
            {
                "tableName": "hotelReviewsKeyPhrases",
                "generatedKeyName": "KeyPhrasesid",
                "source": "/document/tableprojection/pages/*/keyphrase/*"
            }
        ]
    }
]
```

#### <a name="inline-shaping-projections"></a>Projeksiyonları satır satırlı şekillendirme

Projeksiyonlar için gerekli tüm şekiller ihtiyaç duyulduğu anda oluşturulduğundan, satır altı şekillendirme yaklaşımı şekillendirici becerisi gerektirmez. Önceki örnekle aynı verileri yansıtmak için satır içinde projeksiyon seçeneği aşağıdaki gibi görünür:

```json
"projections": [
    {
        "tables": [
            {
                "tableName": "hotelReviewsInlineDocument",
                "generatedKeyName": "Documentid",
                "sourceContext": "/document",     
                "inputs": [
                    {
                        "name": "reviews_text",
                        "source": "/document/reviews_text"
                    },
                    {
                        "name": "reviews_title",
                        "source": "/document/reviews_title"
                    },
                    {
                        "name": "AzureSearch_DocumentKey",
                        "source": "/document/AzureSearch_DocumentKey"
                    }                             
                ]
            },
            {
                "tableName": "hotelReviewsInlinePages",
                "generatedKeyName": "Pagesid",
                "sourceContext": "/document/reviews_text/pages/*",
                "inputs": [
                        {
                    "name": "SentimentScore",
                    "source": "/document/reviews_text/pages/*/Sentiment"
                    },
                    {
                        "name": "LanguageCode",
                        "source": "/document/Language"
                    },
                    {
                        "name": "Page",
                        "source": "/document/reviews_text/pages/*"
                    }
                ]
            },
            {
                "tableName": "hotelReviewsInlineKeyPhrases",
                "generatedKeyName": "KeyPhraseId",
                "sourceContext": "/document/reviews_text/pages/*/Keyphrases/*",
                "inputs": [
                    {
                        "name": "Keyphrases",
                        "source": "/document/reviews_text/pages/*/Keyphrases/*"
                    }
                ]
            }
        ]
    }
]
```
  
Her iki yaklaşımdan bir gözlem `"Keyphrases"` nasıl değerleri kullanılarak `"sourceContext"`yansıtılır . Dizeleri `"Keyphrases"` bir koleksiyon içeren düğüm, kendisi sayfa metninin bir alt. Ancak, projeksiyonlar bir JSON nesnesi gerektirdiğinden ve `"sourceContext"` sayfa ilkel (dize) olduğundan, anahtar tümceciği adlandırılmış bir özelliği olan bir nesneye sarmak için kullanılır. Bu teknik, ilkel lerin bile bağımsız olarak yansıtılmasını sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Bir sonraki adım olarak, bilişsel becerileri ile ilk skillset oluşturun.

> [!div class="nextstepaction"]
> [İlk beceri oluşturun.](cognitive-search-defining-skillset.md)
