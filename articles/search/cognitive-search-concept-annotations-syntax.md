---
title: Becerileri içinde başvuru girişleri ve çıktılar
titleSuffix: Azure Cognitive Search
description: Ek açıklama sözdizimini ve Azure Bilişsel Arama içindeki bir AI zenginleştirme ardışık düzeninde bulunan bir beceri giriş ve çıktılarında ek açıklamaya nasıl başvurululacağını açıklar.
manager: nitinme
author: LuisCabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 03431d861ca6d469b894e45c36fe2a3d7904c3a2
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88935543"
---
# <a name="how-to-reference-annotations-in-an-azure-cognitive-search-skillset"></a>Azure Bilişsel Arama Beceri ek açıklamalarına başvurma

Bu makalede, çeşitli senaryoları göstermek için örnekler kullanarak yetenek tanımlarındaki ek açıklamaların nasıl başvurululacağını öğrenirsiniz. Bir belge içeriği bir yetenek kümesiyle akar, bu, ek açıklamalarla zenginleştirir. Ek açıklamalar, daha fazla aşağı akış zenginleştirme girişi olarak veya bir dizindeki bir çıkış alanına eşlenmiş olarak kullanılabilir. 
 
Bu makaledeki örneklerde, belge çözme aşamasının bir parçası olarak [Azure Blob Dizin oluşturucular](search-howto-indexing-azure-blob-storage.md) tarafından otomatik olarak oluşturulan *içerik* alanı temel alınır. Blob kapsayıcısından gelen belgelere başvuru yaparken, gibi bir biçim kullanın `"/document/content"` , burada *içerik* alanı *belgenin*bir parçasıdır. 

## <a name="background-concepts"></a>Arka plan kavramları

Söz dizimini gözden geçirmeden önce, bu makalenin ilerleyen kısımlarında belirtilen örnekleri daha iyi anlamak için birkaç önemli kavramı yeniden ziyaret edelim.

| Süre | Açıklama |
|------|-------------|
| Zenginleştirilmiş belge | Zenginleştirilmiş bir belge, işlem hattı tarafından oluşturulan ve bir belge ile ilgili tüm ek açıklamaları tutmak için kullanılan dahili bir yapıdır. Zenginleştirilmiş bir belgeyi, ek açıklama ağacı olarak düşünün. Genellikle, önceki bir ek açıklamanın oluşturduğu bir ek açıklama alt öğesi olur.<p/>Zenginleştirilmiş belgeler yalnızca beceri yürütmesinin süresi boyunca mevcuttur. İçerik arama dizinine eşlendikten sonra, zenginleştirilmiş belge artık gerekli değildir. Zenginleştirilmiş belgelerle doğrudan etkileşim kurmasanız da, Beceri oluştururken belge için bir model olması yararlı olacaktır. |
| Zenginleştirme bağlamı | Hangi öğenin zenginleştirmesi halinde, zenginleştirme 'nin gerçekleştiği bağlam. Varsayılan olarak, enzenginleştirme bağlamı `"/document"` tek tek belgeler kapsamındaki düzeyindedir. Bir yetenek çalıştırıldığında, bu beceriye ait çıktılar [tanımlanan bağlamın özellikleri](#example-2)haline gelir.|

<a name="example-1"></a>
## <a name="example-1-simple-annotation-reference"></a>Örnek 1: basit ek açıklama başvurusu

Azure Blob depolama alanında, varlık tanıma kullanarak çıkarmak istediğiniz kişilerin adlarına başvurular içeren çeşitli dosyalarınız olduğunu varsayalım. Aşağıdaki yetenek tanımında, `"/document/content"` Tüm belgenin metinsel gösterimidir ve "insanlar" kişi olarak tanımlanan varlıklar için tam adların ayıklanmasından oluşur.

Varsayılan bağlam olduğundan `"/document"` , kişiler listesine artık olarak başvurulabilir `"/document/people"` . Bu özel durumda, `"/document/people"` artık dizindeki bir alanla eşlenemeyen veya aynı beceri başka bir becerde kullanılan bir ek açıklama bulunur.

```json
  {
    "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
    "categories": [ "Person"],
    "defaultLanguageCode": "en",
    "inputs": [
      {
        "name": "text",
        "source": "/document/content"
      }
    ],
    "outputs": [
      {
        "name": "persons",
        "targetName": "people"
      }
    ]
  }
```

<a name="example-2"></a>

## <a name="example-2-reference-an-array-within-a-document"></a>Örnek 2: belge içindeki bir diziye başvuru

Bu örnekte, bir zenginleştirme adımını aynı belgede birden çok kez nasıl çağırabileceğiniz gösterilmektedir. Önceki örneğin tek bir belgeden 10 kişi adıyla bir dize dizisi oluşturulduğunu varsayın. Makul bir sonraki adım, tam bir adın soyadını çıkaran ikinci bir zenginleştirme olabilir. 10 ad olduğundan, bu adımda her kişi için bir kez olmak üzere bu belgede 10 kez çağrılabilir. 

Doğru sayıda yinelemeyi çağırmak için bağlamı olarak ayarlayın `"/document/people/*"` , burada yıldız işareti ( `"*"` ), zenginleştirilmiş belgedeki tüm düğümleri öğesinin alt öğesi olarak temsil eder `"/document/people"` . Bu yetenek yetenekler dizisinde yalnızca bir kez tanımlanmış olsa da, tüm Üyeler işlenene kadar belgedeki her üye için çağrılır.

```json
  {
    "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
    "description": "Fictitious skill that gets the last name from a full name",
    "uri": "http://names.azurewebsites.net/api/GetLastName",
    "context" : "/document/people/*",
    "defaultLanguageCode": "en",
    "inputs": [
      {
        "name": "fullname",
        "source": "/document/people/*"
      }
    ],
    "outputs": [
      {
        "name": "lastname",
        "targetName": "last"
      }
    ]
  }
```

Ek açıklamalar diziler veya dize koleksiyonları olduğunda, bir bütün olarak dizi yerine belirli üyeleri hedeflemek isteyebilirsiniz. Yukarıdaki örnek, `"last"` bağlamını temsil eden her düğüm altında adlı bir ek açıklama üretir. Bu ek açıklama ailesine başvurmak istiyorsanız söz dizimini kullanabilirsiniz `"/document/people/*/last"` . Belirli bir ek açıklamaya başvurmak isterseniz, bir açık dizin kullanabilirsiniz: `"/document/people/1/last` "belgede tanımlanan ilk kişinin soyadı. Bu söz dizimi diziminde "0 dizinli" olduğuna dikkat edin.

<a name="example-3"></a>

## <a name="example-3-reference-members-within-an-array"></a>Örnek 3: bir dizi içindeki başvuru üyeleri

Bazen belirli bir yeteneğe iletmek için belirli bir türün tüm ek açıklamalarını gruplamak gerekir. Örnek 2 ' de ayıklanan tüm son adlardan en yaygın adı tanımlayan bir kuramsal özel yetenek düşünün. Özel beceriye yalnızca soyadlarını sağlamak için, bağlamı `"/document"` ve giriş olarak belirtin `"/document/people/*/lastname"` .

Kardinalitesi `"/document/people/*/lastname"` belgeden daha büyük olduğuna dikkat edin. Bu belge için yalnızca bir belge düğümü varken 10 LastName düğümü olabilir. Bu durumda, sistem otomatik olarak  `"/document/people/*/lastname"` belgedeki tüm öğeleri içeren bir dizi oluşturur.

```json
  {
    "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
    "description": "Fictitious skill that gets the most common string from an array of strings",
    "uri": "http://names.azurewebsites.net/api/MostCommonString",
    "context" : "/document",
    "inputs": [
      {
        "name": "strings",
        "source": "/document/people/*/lastname"
      }
    ],
    "outputs": [
      {
        "name": "mostcommon",
        "targetName": "common-lastname"
      }
    ]
  }
```



## <a name="see-also"></a>Ayrıca bkz.
+ [Özel bir yeteneği bir zenginleştirme işlem hattı ile tümleştirme](cognitive-search-custom-skill-interface.md)
+ [Beceri tanımlama](cognitive-search-defining-skillset.md)
+ [Beceri oluşturma (REST)](/rest/api/searchservice/create-skillset)
+ [Zenginleştirilmiş alanları bir dizinle eşleme](cognitive-search-output-field-mapping.md)