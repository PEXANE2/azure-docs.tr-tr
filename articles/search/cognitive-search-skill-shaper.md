---
title: Şekillendirici bilişsel becerisi
titleSuffix: Azure Cognitive Search
description: Yapılandırılmamış verilerden meta verileri ve yapılandırılmış bilgileri ayıklayın ve Azure Bilişsel Arama'daki bir AI zenginleştirme ardışık alanında karmaşık bir tür olarak şekillendirin.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 384b79037bb30656934c5e4b596dac2b776593b0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75754125"
---
# <a name="shaper-cognitive-skill"></a>Şekillendirici bilişsel becerisi

**Shaper** becerisi, daha sonra zenginleştirme ardışık alanında başvurulan karmaşık bir [türde](search-howto-complex-data-types.md) birkaç girişi birleştirir. **Şekillendirici** becerisi, temelde bir yapı oluşturmanıza, bu yapının üyelerinin adlarını tanımlamanıza ve her üyeye değer atamanıza olanak tanır. Arama senaryolarında yararlı olan birleştirilmiş alanlara örnek olarak, ad ve soyad birin tek bir yapıda, şehir ve devlette tek bir yapıda veya benzersiz kimlik oluşturmak için ad ve doğum tarihini tek bir yapıda birleştirme verilebilir.

Ayrıca, [senaryo 3'te](#nested-complex-types) gösterilen **Şekillendirici** becerisi girişe isteğe bağlı bir *kaynakBağlam* özelliği ekler. *Kaynak* ve *kaynakBağlam* özellikleri birbirini dışlar. Giriş beceri bağlamında ise, sadece *kaynak*kullanın. Giriş beceri bağlamından *farklı* bir bağlamdaysa, *kaynak Bağlamını*kullanın. *Kaynak Bağlam,* kaynak olarak ele alınmakta olan belirli bir öğeyle iç içe bir girdi tanımlamanızı gerektirir. 

Çıktı adı her zaman "çıktı"dır. Dahili olarak, ardışık hatlar aşağıdaki örneklerde gösterildiği gibi "çözümlenmiş Metin" gibi farklı bir adı eşleyebilir, ancak **Shaper** becerisi yanıtta "çıktı" döndürür. Zenginleştirilmiş belgeleri hata ayıklıyorsanız ve adlandırma tutarsızlığını fark ediyorsanız veya özel bir beceri oluşturuyorsanız ve yanıtı kendiniz yapılıyorsanız bu önemli olabilir.

> [!NOTE]
> **Shaper** beceri Bilişsel Hizmetler API bağlı değildir ve bunu kullanmak için ücret değildir. Yine de, günlük günlük zenginleştirmeler az sayıda sizi sınırlayan **Ücretsiz** kaynak seçeneği geçersiz kılmak için, [bilişsel hizmetler kaynak eklemeniz](cognitive-search-attach-cognitive-services.md)gerekir.

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Util.ShaperBeceri

## <a name="scenario-1-complex-types"></a>Senaryo 1: karmaşık türleri

*Çözümlenmiş Metin* adlı bir yapı oluşturmak istediğiniz bir senaryo düşünün: sırasıyla *metin* ve *duyarlılık.* Bir dizinde, çok parçalı aranabilir alan *karmaşık* tür olarak adlandırılır ve genellikle kaynak verilerin eşleyen karşılık gelen karmaşık bir yapıya sahip olduğunda oluşturulur.

Ancak, karmaşık türleri oluşturmak için başka bir yaklaşım **Shaper** beceri geçer. Bu beceriyi bir beceriye dahil ederek, skillset işleme sırasındaki bellek işlemleri iç içe yapılarla veri şekilleri çıkararak dizininizdeki karmaşık bir türe eşlenebilir. 

Aşağıdaki örnekte beceri tanımı üye adlarını giriş olarak sağlar. 


```json
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
}
```

### <a name="sample-index"></a>Örnek dizini

Bir skillset bir dizinleyici tarafından çağrılır ve bir dizin oluşturma gerektirir. Dizininizdeki karmaşık bir alan gösterimi aşağıdaki örnek gibi görünebilir. 

```json

    "name": "my-index",
    "fields": [
        {   "name": "myId", "type": "Edm.String", "key": true, "filterable": true   },
        {   "name": "analyzedText", "type": "Edm.ComplexType",
            "fields": [{
                    "name": "text",
                    "type": "Edm.String",
                    "filterable": false,
                    "sortable": false,
                    "facetable": false,
                    "searchable": true  },
          {
                    "name": "sentiment",
                    "type": "Edm.Double",
                    "searchable": true,
                    "filterable": true,
                    "sortable": true,
                    "facetable": true
                },
```

### <a name="skill-input"></a>Beceri girişi

Bu **Shaper** becerisi için kullanılabilir girdi sağlayan gelen bir JSON belgesi şu olabilir:

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "text": "this movie is awesome",
                "sentiment": 0.9
            }
        }
    ]
}
```


### <a name="skill-output"></a>Beceri çıktısı

**Shaper** beceri *metin* ve *duygu*kombine unsurları ile *analizText* adlı yeni bir öğe oluşturur. Bu çıktı dizin şemasına uygundur. Azure Bilişsel Arama dizininde içe aktarılır ve dizine eklenir.

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
            "analyzedText": 
              {
                "text": "this movie is awesome" ,
                "sentiment": 0.9
              }
           }
      }
    ]
}
```

## <a name="scenario-2-input-consolidation"></a>Senaryo 2: giriş konsolidasyonu

Başka bir örnekte, boru hattı işlemenin farklı aşamalarında, bir kitabın başlığını ve kitabın farklı sayfalarındaki bölüm başlıklarını ayıkladığınızu düşünün. Artık bu çeşitli girdilerden oluşan tek bir yapı oluşturabilirsiniz.

Bu senaryonun **Şekillendirici** beceri tanımı aşağıdaki örnek gibi görünebilir:

```json
{
    "@odata.type": "#Microsoft.Skills.Util.ShaperSkill",
    "context": "/document",
    "inputs": [
        {
            "name": "title",
            "source": "/document/content/title"
        },
        {
            "name": "chapterTitles",
            "source": "/document/content/pages/*/chapterTitles/*/title"
        }
    ],
    "outputs": [
        {
            "name": "output",
            "targetName": "titlesAndChapters"
        }
    ]
}
```

### <a name="skill-output"></a>Beceri çıktısı
Bu durumda, **Shaper** tek bir dizi oluşturmak için tüm bölüm başlıkları düzleştirir. 

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "titlesAndChapters": {
                    "title": "How to be happy",
                    "chapterTitles": [
                        "Start young",
                        "Laugh often",
                        "Eat, sleep and exercise"
                    ]
                }
            }
        }
    ]
}
```

<a name="nested-complex-types"></a>

## <a name="scenario-3-input-consolidation-from-nested-contexts"></a>Senaryo 3: iç içe geçmiş bağlamlardan giriş konsolidasyonu

Bir kitabın başlığına, bölümlerine ve içeriğine sahip olduğunuzu ve varlık tanıma ve içeriğinde anahtar tümcecikler çalıştırdığınızı ve şimdi farklı becerilerin sonuçlarını bölüm adı, varlıklar ve anahtar tümceciklerle tek bir şekle toplamanız gerektiğini düşünün.

Bu senaryonun **Şekillendirici** beceri tanımı aşağıdaki örnek gibi görünebilir:

```json
{
    "@odata.type": "#Microsoft.Skills.Util.ShaperSkill",
    "context": "/document",
    "inputs": [
        {
            "name": "title",
            "source": "/document/content/title"
        },
        {
            "name": "chapterTitles",
            "sourceContext": "/document/content/pages/*/chapterTitles/*",
            "inputs": [
              {
                  "name": "title",
                  "source": "/document/content/pages/*/chapterTitles/*/title"
              },
              {
                  "name": "number",
                  "source": "/document/content/pages/*/chapterTitles/*/number"
              }
            ]
        }

    ],
    "outputs": [
        {
            "name": "output",
            "targetName": "titlesAndChapters"
        }
    ]
}
```

### <a name="skill-output"></a>Beceri çıktısı
Bu durumda, **Şekillendirici** karmaşık bir tür oluşturur. Bu yapı bellekte var. Bir [bilgi deposuna](knowledge-store-concept-intro.md)kaydetmek istiyorsanız, becerinizde depolama özelliklerini tanımlayan bir projeksiyon oluşturmalısınız.

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "titlesAndChapters": {
                    "title": "How to be happy",
                    "chapterTitles": [
                      { "title": "Start young", "number": 1},
                      { "title": "Laugh often", "number": 2},
                      { "title": "Eat, sleep and exercise", "number: 3}
                    ]
                }
            }
        }
    ]
}
```

## <a name="see-also"></a>Ayrıca bkz.

+ [Yerleşik yetenekler](cognitive-search-predefined-skills.md)
+ [Bir skillset nasıl tanımlanır?](cognitive-search-defining-skillset.md)
+ [Karmaşık türleri nasıl kullanılır?](search-howto-complex-data-types.md)
+ [Bilgi deposu (önizleme)](knowledge-store-concept-intro.md)
+ [REST'te bir bilgi deposu oluşturun](knowledge-store-create-rest.md)