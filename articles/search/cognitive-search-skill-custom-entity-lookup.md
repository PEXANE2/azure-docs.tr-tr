---
title: Özel Varlık Arama bilişsel arama becerisi
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama bilişsel arama ardışık ardışık bir metinden farklı özel varlıklar ayıklayın. Bu beceri şu anda genel önizlemede.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/30/2020
ms.openlocfilehash: 3659070d4ffd4346a8827d2748e67db436fc15b3
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82085748"
---
#     <a name="custom-entity-lookup-cognitive-skill-preview"></a>Özel Varlık Arama bilişsel beceri (Önizleme)

> [!IMPORTANT] 
> Bu beceri şu anda genel önizlemede. Önizleme işlevi hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın. Şu anda portal veya .NET SDK desteği yok.

**Özel Varlık Arama** becerisi, özel, kullanıcı tarafından tanımlanan sözcük ve tümcecikler listesinden metin arar. Bu listeyi kullanarak, tüm belgeleri eşleşen varlıklarla etiketler. Beceri aynı zamanda benzer ama oldukça kesin olmayan eşleşmeleri bulmak için uygulanabilir bulanık eşleştirme bir dereceyi destekler.  

Bu beceri Bilişsel Hizmetler API'sine bağlı değildir ve önizleme süresi boyunca ücretsiz olarak kullanılabilir. Yine de günlük zenginleştirme sınırını geçersiz kılmak için [bir Bilişsel Hizmetler kaynağı eklemeniz](https://docs.microsoft.com/azure/search/cognitive-search-attach-cognitive-services)gerekir. Günlük sınır, Azure Bilişsel Arama aracılığıyla erişildiğinde Bilişsel Hizmetlere ücretsiz erişim için geçerlidir.

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.CustomEntityLookupSkill 

## <a name="data-limits"></a>Veri sınırları
+ Desteklenen maksimum giriş kaydı boyutu 256 MB'dır. Verilerinizi özel varlık arama becerisine göndermeden önce ayırmanız gerekiyorsa, [Metin Bölme becerisini](cognitive-search-skill-textsplit.md)kullanmayı düşünün.
+ Desteklenen maksimum varlıklar tanım tablosu, *varlıklarDefinitionUri* parametresi kullanılarak sağlanırsa 10 MB'dır. 
+ Varlıklar satır satırda tanımlanırsa, *inlineNtitiesDefinition* parametresi kullanılarak, desteklenen maksimum boyut 10 KB'dir.

## <a name="skill-parameters"></a>Beceri parametreleri

Parametreler büyük/küçük harfe duyarlıdır.

| Parametre adı     | Açıklama |
|--------------------|-------------|
| varlıklarDefinitionUri    | Eşleşecek tüm hedef metni içeren bir JSON veya CSV dosyasına giden yol. Bu varlık tanımı bir dizinleyici çalışmasının başında okunur; bu dosyanın orta vadede herhangi bir güncelleştirme sonraki çalıştırmaları kadar gerçekleştirilmeyecek. Bu config HTTPS üzerinden erişilebilir olmalıdır. Beklenen CSV veya JSON şeması için aşağıdaki [Özel Varlık Tanımı](#custom-entity-definition-format) Biçimi"ne bakın.|
|inlineEntitiesDefinition | Satır Satırlı JSON varlık tanımları. Bu parametre varsa varlıklarDefinitionUri parametre yerini. En fazla 10 KB yapılandırma satır içinde sağlanabilir. Beklenen JSON şeması için aşağıdaki [Özel Varlık Tanımı'na](#custom-entity-definition-format) bakın. |
|defaultLanguageCode |    (İsteğe bağlı) Giriş metnini belirteçleştirmek ve demi yapmak için kullanılan giriş metninin dil kodu. Aşağıdaki diller desteklenir: `da, de, en, es, fi, fr, it, ko, pt`. Varsayılan İngilizce (`en`). Bir languagecode-countrycode biçimini geçerseniz, biçimin yalnızca languagecode bölümü kullanılır.  |


## <a name="skill-inputs"></a>Beceri girdileri

| Giriş adı      | Açıklama                   |
|---------------|-------------------------------|
| metin          | Çözümlemek için metin.          |
| languageCode    | İsteğe bağlı. `"en"` varsayılan değerdir.  |


## <a name="skill-outputs"></a>Beceri çıktıları


| Çıktı adı      | Açıklama                   |
|---------------|-------------------------------|
| Varlık | Bulunan eşleşmeler ve ilgili meta veriler hakkında bilgi içeren bir dizi nesne. Tanımlanan varlıkların her biri aşağıdaki alanları içerebilir:  <ul> <li> *isim*: Tanımlanan üst düzey varlık. Varlık "normalleştirilmiş" formu temsil eder. </li> <li> *id*: Kullanıcı tarafından "Özel Varlık Tanımı Biçimi"nde tanımlanan varlık için benzersiz bir tanımlayıcıdır.</li> <li> *açıklaması*: Kullanıcı tarafından "Özel Varlık Tanımı Biçimi"nde tanımlanan varlık açıklaması. </li> <li> *türü:* Kullanıcı tarafından "Özel Varlık Tanımı Biçimi"nde tanımlanan varlık türü.</li> <li> *alt türü:* "Özel Varlık Tanımı Biçimi"nde kullanıcı tarafından tanımlanan varlık alt türü.</li>  <li> *eşleşmeler*: Kaynak metinde o varlığın eşleşmelerinin her birini açıklayan koleksiyon. Her maç aşağıdaki üyelere sahip olacaktır: </li> <ul> <li> *metin*: Kaynak belgedeki ham metin eşleşir. </li> <li> *ofset*: Metnin bulunduğu yer. </li> <li> *uzunluk*: Eşleşen metnin uzunluğu. </li> <li> *matchDistance*: Bu eşleşmeden farklı karakter sayısı özgün varlık adından veya diğer addan dı.  </li> </ul> </ul>
  |

## <a name="custom-entity-definition-format"></a>Özel Varlık Tanım Biçimi

Özel Varlık Arama becerisine özel varlıkların listesini sağlamanın 3 farklı yolu vardır. Listeyi bir ' de sağlayabilirsiniz. CSV dosyası, bir . JSON dosyası veya beceri tanımının bir parçası olarak satır içinde tanım olarak.  

Tanım dosyası bir . CSV veya . JSON dosyası, dosyanın yolunu *varlıklarDefinitionUri* parametrenin bir parçası olarak sağlanmalıdır. Bu durumda, dosya her dizinleyici çalıştırın başında bir kez indirilir. Dizinleyici nin çalışması amaçlandığı sürece dosyaya erişilebilir olmalıdır. Ayrıca, dosya UTF-8 kodlanmış olmalıdır.

Tanım satır satır olarak sağlanıyorsa, *inlineNtitiesDefinition* beceri parametresi içeriği olarak satır içinde sağlanmalıdır. 

### <a name="csv-format"></a>CSV formatı

Virgülden Ayrılmış Değer (CSV) dosyasında, dosyanın yolunu sağlayarak ve *varlıklarDefinitionUri* beceri parametresinde ayarlayarak, arayacağınız özel varlıkların tanımını sağlayabilirsiniz. Yol bir https konumunda olmalıdır. Tanım dosyası 10 MB boyutuna kadar olabilir.

CSV formatı basittir. Her satır, aşağıda gösterildiği gibi benzersiz bir varlığı temsil eder:

```
Bill Gates, BillG, William H. Gates
Microsoft, MSFT
Satya Nadella 
```

Bu durumda, bulunan varlıklar olarak döndürülebilecek üç varlık vardır (Bill Gates, Satya Nadella, Microsoft), ancak satırdaki terimlerden herhangi biri (diğer adlar) metinde eşleşirse bunlar tanımlanır. Örneğin, "William H. Gates" dizesi bir belgede bulunursa, "Bill Gates" varlığı için bir eşleşme döndürülür.

### <a name="json-format"></a>JSON biçimi

Bir JSON dosyasında da aramak için özel varlıkların tanımını sağlayabilirsiniz. JSON biçimi, dönem başına eşleşen kuralları tanımlamanıza olanak sağladığından size biraz daha fazla esneklik sağlar. Örneğin, her dönem için bulanık eşleşen mesafeyi (Damerau-Levenshtein mesafesi) veya eşleştirmenin büyük/küçük harf duyarlı olup olmaması gerektiğini belirtebilirsiniz. 

 CSV dosyalarında olduğu gibi, JSON dosyasına giden yolu sağlamanız ve *varlıklarıDefinitionUri* beceri parametresi olarak ayarlamanız gerekir. Yol bir https konumunda olmalıdır. Tanım dosyası 10 MB boyutuna kadar olabilir.

En temel JSON özel varlık listesi tanımı eşleşecek varlıkların listesi olabilir:

```json
[ 
    { 
        "name" : "Bill Gates"
    }, 
    { 
        "name" : "Microsoft"
    }, 
    { 
        "name" : "Satya Nadella"
    }
]
```

JSON tanımının daha karmaşık bir örneği isteğe bağlı olarak her varlığın kimliğini, açıklamasını, türünü ve alt türünü ve diğer *diğer diğer adları*sağlayabilir. Bir takma ad terimi eşleşirse, varlık da döndürülür:

```json
[ 
    { 
        "name" : "Bill Gates",
        "description" : "Microsoft founder." ,
        "aliases" : [ 
            { "text" : "William H. Gates", "caseSensitive" : false },
            { "text" : "BillG", "caseSensitive" : true }
        ]
    }, 
    { 
        "name" : "Xbox One", 
        "type": "Harware",
        "subtype" : "Gaming Device",
        "id" : "4e36bf9d-5550-4396-8647-8e43d7564a76",
        "description" : "The Xbox One product"
    }, 
    { 
        "name" : "LinkedIn" , 
        "description" : "The LinkedIn company", 
        "id" : "differentIdentifyingScheme123", 
        "fuzzyEditDistance" : 0 
    }, 
    { 
        "name" : "Microsoft" , 
        "description" : "Microsoft Corporation", 
        "id" : "differentIdentifyingScheme987", 
        "defaultCaseSensitive" : false, 
        "defaultFuzzyEditDistance" : 1, 
        "aliases" : [ 
            { "text" : "MSFT", "caseSensitive" : true }
        ]
    } 
] 
```

Aşağıdaki tablolar, varlıkları eşleşecek şekilde tanımlarken ayarlayabileceğiniz farklı yapılandırma parametrelerini daha ayrıntılı olarak açıklayınız:

|  Alan adı  |        Açıklama  |
|--------------|----------------------|
| ad | Üst düzey varlık tanımlayıcısı. Beceri çıkışındaki eşleşmeler bu ada göre gruplandırılır ve bulunan metnin "normalleştirilmiş" biçimini temsil etmelidir.  |
| açıklama  | (İsteğe bağlı) Bu alan, eşleşen metin(ler) hakkında özel meta veriler için bir geçiş olarak kullanılabilir. Bu alanın değeri, beceri çıktısında kendi varlığının her eşleşmesi ile görünür. |
| type | (İsteğe bağlı) Bu alan, eşleşen metin(ler) hakkında özel meta veriler için bir geçiş olarak kullanılabilir. Bu alanın değeri, beceri çıktısında kendi varlığının her eşleşmesi ile görünür. |
| Alt | (İsteğe bağlı) Bu alan, eşleşen metin(ler) hakkında özel meta veriler için bir geçiş olarak kullanılabilir. Bu alanın değeri, beceri çıktısında kendi varlığının her eşleşmesi ile görünür. |
| id | (İsteğe bağlı) Bu alan, eşleşen metin(ler) hakkında özel meta veriler için bir geçiş olarak kullanılabilir. Bu alanın değeri, beceri çıktısında kendi varlığının her eşleşmesi ile görünür. |
| Casesensitive | (İsteğe bağlı) Varsayılan olarak false. Boolean değeri, varlık adı ile karşılaştırmaların karakter kasasına duyarlı olup olmadığını belirtir. Örnek durumda duyarsız maçlar "Microsoft" olabilir: microsoft, microSoft, MICROSOFT |
| bulanıkEditDistance | (İsteğe bağlı) Varsayılan olarak 0'a kadar. Maksimum değeri 5. Yine de varlık adı ile eşleşecek farklı karakter kabul edilebilir sayıda gösterir. Herhangi bir eşleşme için mümkün olan en küçük bulanıklık döndürülür.  Örneğin, edit mesafesi 3 olarak ayarlanmışsa, "Windows 10" yine de "Windows", "Windows10" ve "windows 7" ile eşleşir. <br/> Durum hassasiyeti yanlış olarak ayarlandığında, durum farklılıkları fuzziness toleransdoğru sayılmaz, ama aksi takdirde yok. |
| defaultCaseSensitive | (İsteğe bağlı) Bu varlık için varsayılan servis talebi duyarlılık değerini değiştirir. Tüm diğer adlar için duyarlı değerlerin varsayılan değerini değiştirmek için kullanılır. |
| varsayılanFuzzyEditDistance | (İsteğe bağlı) Bu varlık için varsayılan bulanık edit uzaklık değerini değiştirir. Tüm diğer adların varsayılan değerini değiştirmek için kullanılabilir bulanıkEditDistance değerleri. |
| Takma | (İsteğe bağlı) Kök varlık adı ile alternatif yazımları veya eşanlamlıları belirtmek için kullanılabilecek karmaşık nesneler dizisi. |

| Diğer ad özellikleri | Açıklama |
|------------------|-------------|
| metin  | Bazı hedef varlık adının alternatif yazım veya gösterimi.  |
| Casesensitive | (İsteğe bağlı) Yukarıdaki kök varlık "caseSensitive" parametresi ile aynı şekilde davranır, ancak yalnızca bu diğer ad için geçerlidir. |
| bulanıkEditDistance | (İsteğe bağlı) Yukarıdaki kök varlık "bulanıkEditDistance" parametresi ile aynı şekilde davranır, ancak yalnızca bu diğer ad için geçerlidir. |


### <a name="inline-format"></a>Satır biçiminde

Bazı durumlarda, satır satırlarını doğrudan beceri tanımına eşleştirmek için özel varlıkların listesini sağlamak daha uygun olabilir. Bu durumda, yukarıda açıklanana benzer bir JSON biçimi kullanabilirsiniz, ancak beceri tanımında çizgilidir.
Yalnızca 10 KB boyutundan (seri boyutu) daha küçük yapılandırmalar satır içinde tanımlanabilir. 

##    <a name="sample-definition"></a>Örnek tanımı

Satır altı biçimi kullanan örnek bir beceri tanımı aşağıda gösterilmiştir:

```json
  {
    "@odata.type": "#Microsoft.Skills.Text.CustomEntityLookupSkill",
    "context": "/document",
    "inlineEntitiesDefinition": 
    [
      { 
        "name" : "Bill Gates",
        "description" : "Microsoft founder." ,
        "aliases" : [ 
            { "text" : "William H. Gates", "caseSensitive" : false },
            { "text" : "BillG", "caseSensitive" : true }
        ]
      }, 
      { 
        "name" : "Xbox One", 
        "type": "Harware",
        "subtype" : "Gaming Device",
        "id" : "4e36bf9d-5550-4396-8647-8e43d7564a76",
        "description" : "The Xbox One product"
      }
    ],    
    "inputs": [
      {
        "name": "text",
        "source": "/document/content"
      }
    ],
    "outputs": [
      {
        "name": "entities",
        "targetName": "matchedEntities"
      }
    ]
  }
```
Alternatif olarak, varlıklar tanım dosyasına bir işaretçi sağlamaya karar verirseniz, varlıklarDefinitionUri biçimini kullanarak örnek bir beceri tanımı aşağıda gösterilmiştir:

```json
  {
    "@odata.type": "#Microsoft.Skills.Text.CustomEntityLookupSkill",
    "context": "/document",
    "entitiesDefinitionUri": "https://myblobhost.net/keyWordsConfig.csv",    
    "inputs": [
      {
        "name": "text",
        "source": "/document/content"
      }
    ],
    "outputs": [
      {
        "name": "entities",
        "targetName": "matchedEntities"
      }
    ]
  }

```

##    <a name="sample-input"></a>Örnek giriş

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
             "text": "The company microsoft was founded by Bill Gates. Microsoft's gaming console is called Xbox",
             "languageCode": "en"
           }
      }
    ]
}
```

##    <a name="sample-output"></a>Örnek çıktı

```json
  { 
    "values" : 
    [ 
      { 
        "recordId": "1", 
        "data" : { 
          "entities": [
            { 
              "name" : "Microsoft", 
              "description" : "This document refers to Microsoft the company", 
              "id" : "differentIdentifyingScheme987", 
              "matches" : [ 
                { 
                  "text" : "microsoft", 
                  "offset" : 13, 
                  "length" : 9, 
                  "matchDistance" : 0 
                }, 
                { 
                  "text" : "Microsoft",
                  "offset" : 49, 
                  "length" : 9, 
                  "matchDistance" : 0
                }
              ] 
            },
            { 
              "name" : "Bill Gates",
              "description" : "William Henry Gates III, founder of Microsoft.", 
              "matches" : [
                { 
                  "text" : "Bill Gates",
                  "offset" : 37, 
                  "length" : 10,
                  "matchDistance" : 0 
                }
              ]
            }
          ] 
        } 
      } 
    ] 
  } 
```

## <a name="errors-and-warnings"></a>Hatalar ve uyarılar

### <a name="warning-reached-maximum-capacity-for-matches-skipping-all-further-duplicate-matches"></a>Uyarı: Diğer yinelenen eşleşmeleri atlayarak maçlar için maksimum kapasiteye ulaşıldı.

Algılanan eşleşme sayısı izin verilen maksimumdan büyükse, bu uyarı yayımlanır. Bu durumda, yinelenen eşleşmeleri dahil etmeyi durduracağız. Bu sizin için kabul edilemezse, lütfen bir [destek bileti](https://ms.portal.azure.com/#create/Microsoft.Support) gönderin, böylece size kişisel kullanım durumunuzda yardımcı olabiliriz.

## <a name="see-also"></a>Ayrıca bkz.

+ [Yerleşik yetenekler](cognitive-search-predefined-skills.md)
+ [Bir skillset nasıl tanımlanır?](cognitive-search-defining-skillset.md)
+ [Varlık Tanıma becerisi (tanınmış varlıkları aramak için)](cognitive-search-skill-entity-recognition.md)
